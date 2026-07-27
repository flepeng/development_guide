# 错误处理

## `error` 作为函数的值返回，必须对 `error` 进行处理，或将返回值赋值给明确忽略。对于 `defer xx.Close()` 可以不用显式处理。

```go
func load() error {}


// bad
load()


// good
_ = load()
```


## `error` 作为函数的值返回且有多个返回值时，`error` 必须是最后一个参数。

```go
// bad
func load() (error, int) 


// good
func load() (int, error) 
```

 
## 尽早进行错误处理，并尽早返回，减少嵌套。

```go
// bad
if err != nil {
	// error code
} else {
	// normal code
}

// good
if err != nil {
	// error handling
	return err
}
// normal code
```


## 如果需要在 if 之外使用函数调用的结果，则应采用下面的方式。

```go
// bad
if v, err := foo(); err != nil {
	// error handling
}

// good
v, err := foo()
if err != nil {
	// error handling
}
```


## 错误要单独判断，不与其他逻辑组合判断。

```go
// bad
v, err := foo()
if err != nil || v  == nil {
	// error handling
	return err
}


// good
v, err := foo()
if err != nil {
	// error handling
	return err
}

if v == nil {
	// error handling
	return errors.New("invalid value v")
}
```


## 如果返回值需要初始化，则采用下面的方式。

```go
v, err := f()
if err != nil {
    // error handling
    return // or continue.
}
```


## 错误描述建议

*   错误描述用小写字母开头，结尾不要加标点符号，例如：

    ```go
        // bad
        errors.New("Redis connection failed")
        errors.New("redis connection failed.")
    
        // good
        errors.New("redis connection failed")
    ```

*   告诉用户他们可以做什么，而不是告诉他们不能做什么。
*   当声明一个需求时，用 must 而不是 should。例如，`must be greater than 0、must match regex '[a-z]+'`。
*   当声明一个格式不对时，用 must not。例如，`must not contain`。
*   当声明一个动作时用 may not。例如，`may not be specified when otherField is empty、only name may be specified`。
*   引用文字字符串值时，请在单引号中指示文字。例如，`ust not contain '..'`。
*   当引用另一个字段名称时，请在反引号中指定该名称。例如，must be greater than `request`。
*   指定不等时，请使用单词而不是符号。例如，`must be less than 256、must be greater than or equal to 0 (不要用 larger than、bigger than、more than、higher than)`。
*   指定数字范围时，请尽可能使用包含范围。
*   建议 Go 1.13 以上，error 生成方式为 `fmt.Errorf("module xxx: %w", err)`。


# 错误类型

声明错误的选项很少。在选择最适合您的用例的选项之前，请考虑以下事项。

*   返回错误消息时，错误消息是否为静态字符串，还是需要上下文信息的动态字符串？
    如果是静态字符串，使用 [`errors.New`]，如果是动态字符串，使用 [`fmt.Errorf`] 或自定义错误类型。

*   我们是否正在传递由下游函数返回的新错误？
    如果是这样，请参阅[错误包装部分](#错误包装)。

*   我们是否需要 匹配错误 以便处理它？ 
    如果是，我们必须通过声明顶级错误变量或自定义类型来支持 [`errors.Is`] 或 [`errors.As`] 函数。


[`errors.Is`]: https://pkg.go.dev/errors/#Is
[`errors.As`]: https://pkg.go.dev/errors/#As

| 错误匹配？ | 错误消息    | 指导                                  |
|-------|---------|-------------------------------------|
| No    | static  | [`errors.New`]                      |
| No    | dynamic | [`fmt.Errorf`]                      |
| Yes   | static  | top-level `var` with [`errors.New`] |
| Yes   | dynamic | custom `error` type                 |

[`errors.New`]: https://pkg.go.dev/errors/#New
[`fmt.Errorf`]: https://pkg.go.dev/fmt/#Errorf

例如，使用 [`errors.New`] 表示带有静态字符串的错误。

如果调用者需要匹配并处理此错误，则将此错误导出为变量以支持将其与 `errors.Is` 匹配。

```go
// 无错误匹配
// package foo

func Open() error {
    return errors.New("could not open")
}


// package bar
if err := foo.Open(); err != nil {
    // Can't handle the error.
    panic("unknown error")
}


// 错误匹配
// package foo
var ErrCouldNotOpen = errors.New("could not open")

func Open() error {
    return ErrCouldNotOpen
}

// package bar
if err := foo.Open(); err != nil {
    if errors.Is(err, foo.ErrCouldNotOpen) {
        // handle the error
    } else {
        panic("unknown error")
    }
}
```

对于动态字符串的错误，如果调用者不需要匹配它，则使用 [`fmt.Errorf`]，如果调用者确实需要匹配它，则自定义 `error`。

```go
// 无错误匹配
// package foo
func Open(file string) error {
    return fmt.Errorf("file %q not found", file)
}


// package bar
if err := foo.Open("testfile.txt"); err != nil {
    // Can't handle the error.
    panic("unknown error")
}



// 错误匹配
// package foo
type NotFoundError struct {
    File string
}

func (e *NotFoundError) Error() string {
    return fmt.Sprintf("file %q not found", e.File)
}

func Open(file string) error {
    return &NotFoundError{File: file}
}


// package bar
if err := foo.Open("testfile.txt"); err != nil {
  var notFound *NotFoundError
  if errors.As(err, &notFound) {
    // handle the error
  } else {
    panic("unknown error")
  }
}
```


请注意，如果您从包中导出错误变量或类型，它们将成为包的公共 API 的一部分。


# 错误包装

如果调用其他方法时出现错误, 通常有三种处理方式可以选择：

*   将原始错误原样返回
*   使用 `fmt.Errorf` 搭配 `%w` 将错误添加进上下文后返回
*   使用 `fmt.Errorf` 搭配 `%v` 将错误添加进上下文后返回

如果没有要添加的其他上下文，则按原样返回原始错误。这非常适合底层错误消息它是来自哪里。

否则，尽可能在错误消息中添加上下文，这样就不会出现诸如“连接被拒绝”之类的模糊错误，您会收到更多有用的错误，例如“调用服务 foo：连接被拒绝”。

使用 `fmt.Errorf` 为你的错误添加上下文，根据调用者是否应该能够匹配和提取根本原因，在 `%w` 或 `%v` 动词之间进行选择。

*   如果调用者可以访问底层错误，使用 `%w`。这是一个很好的默认值，但请注意，调用者可能会开始依赖此行为。因此，对于包装错误是已知 `var`或类型的情况，请将其作为函数契约的一部分进行记录和测试。
*   使用 `%v` 来混淆底层错误。调用者将无法匹配它。

在为返回的错误添加上下文时，通过避免使用"failed to"之类的短语来保持上下文简洁，当错误通过堆栈向上渗透时，它会一层一层被堆积起来：


```go
// Bad：failed to x: failed to y: failed to create new store: the error
s, err := store.New()
if err != nil {
    return fmt.Errorf("failed to create new store: %w", err)
}


// Good：x: y: new store: the error
s, err := store.New()
if err != nil {
    return fmt.Errorf("new store: %w", err)
}
```

然而，一旦错误被发送到另一个系统，应该清楚消息是一个错误（例如 `err` 标签或日志中的"Failed"前缀）。

另见 [不要只检查错误，优雅地处理它们]。

[`"pkg/errors".Cause`]: https://pkg.go.dev/github.com/pkg/errors#Cause
[不要只检查错误，优雅地处理它们]: https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully


# 错误命名

对于存储为全局变量的错误值，根据是否导出，使用前缀 `Err` 或 `err`。

请看指南 [对于未导出的顶层常量和变量，使用_作为前缀](#对于未导出的顶层常量和变量使用_作为前缀)。

```go
var (
  // 导出以下两个错误，以便此包的用户可以将它们与 errors.Is 进行匹配。
  ErrBrokenLink = errors.New("link is broken")
  ErrCouldNotOpen = errors.New("could not open")

  // 这个错误没有被导出，因为我们不想让它成为我们公共 API 的一部分。 我们可能仍然在带有错误的包内使用它。
  errNotFound = errors.New("not found")
)
```

对于自定义错误类型，请改用后缀 `Error`。

```go
// 同样，这个错误被导出，以便这个包的用户可以将它与 errors.As 匹配。
type NotFoundError struct {
    File string
}


func (e *NotFoundError) Error() string {
    return fmt.Sprintf("file %q not found", e.File)
}

// 并且这个错误没有被导出，因为我们不想让它成为公共 API 的一部分。 我们仍然可以在带有 errors.As 的包中使用它。
type resolveError struct {
    Path string
}

func (e *resolveError) Error() string {
    return fmt.Sprintf("resolve %q", e.Path)
}
```


# 一次处理错误

当调用方从被调用方接收到错误时，它可以根据对错误的了解，以各种不同的方式进行处理。

其中包括但不限于：

*   如果被调用者约定定义了特定的错误，则将错误与 `errors.Is` 或 `errors.As` 匹配，并以不同的方式处理分支
*   如果错误是可恢复的，则记录错误并正常降级
*   如果该错误表示特定于域的故障条件，则返回定义明确的错误
*   返回错误，无论是 [wrapped](#错误包装) 还是逐字逐句

无论调用方如何处理错误，它通常都应该只处理每个错误一次。例如，调用方不应该记录错误然后返回，因为 *its* 调用方也可能处理错误。

例如，考虑以下情况：

```go
// Bad: 记录错误并将其返回
// 堆栈中的调用程序可能会对该错误采取类似的操作。这样做会在应用程序日志中造成大量噪音，但收效甚微。
u, err := getUser(id)
if err != nil {
    // BAD: See description
    log.Printf("Could not get user %q: %v", id, err)
    return err
}


// Good: 将错误换行并返回
// 堆栈中更靠上的调用程序将处理该错误。使用`%w`可确保它们可以将错误与`errors.Is`或`errors.As`相匹配 （如果相关）。
u, err := getUser(id)
if err != nil {
    return fmt.Errorf("get user %q: %w", id, err)
}


// Good: 记录错误并正常降级
// 如果操作不是绝对必要的，我们可以通过从中恢复来提供降级但不间断的体验。
if err := emitMetrics(); err != nil {
    // Failure to write metrics should not
    // break the application.
    log.Printf("Could not emit metrics: %v", err)
}


// Good: 匹配错误并适当降级
// 如果被调用者在其约定中定义了一个特定的错误，并且失败是可恢复的，则匹配该错误案例并正常降级。对于所有其他案例，请包装错误并返回。
// 堆栈中更靠上的调用程序将处理其他错误。
tz, err := getUserTimeZone(id)
if err != nil {
    if errors.Is(err, ErrUserNotFound) {
        // User doesn't exist. Use UTC.
        tz = time.UTC
    } else {
        return fmt.Errorf("get user %q: %w", id, err)
    }
}
```


# 我的

service 层：

service 返回的错误应该是自定义的，因为这个错误要返回给用户。
service 的错误在输出的时候要输出全。

其它层：

所有的错误原封不动的向上返回。
在输出错误的时候可以增加一些额外信息。