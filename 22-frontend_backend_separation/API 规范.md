## 接口定义

接口定义规范参见 [`RESTful 设计指南`](/22-frontend_backend_separation/RESTful%20设计指南.md)


## 接口数据格式

### 使用 JSON 格式传输数据

在请求头和响应头申明 `Content-Type`。

返回的数据结构应该做到尽可能简单，不要过于包装。

响应状态应该包含在响应头中。

**Request**

```
Accept: application/json
Content-Type: application/json;charset=UTF-8
```

**Response**

```
Content-Type: application/json;charset=UTF-8
```


### JSON 属性名建议

1.  属性名应该是具有语义的有意义的名称；

2.  首字母不使用大写(大小写友好)；

3.  使用下划线（或小驼峰）命名，由组内开发成员习惯而定；

4.  属性和字符串值必须使用双引号`""`；

5.  包含“多个结果”的字段，注意名词的复数形式的使用
    1.  当 value 为普通数组时，使用“名词复数”的形式命名 key
    2.  当 value 为嵌套了 JSON 对象的数组时，使用名词 + list 结尾的形式来命名 key
    ```
    {
      "tags": ["食物", "粤菜", "卤水"],
      "memberList": [
        {
          "uid": 111,
          "name": "张三",
          "age": 22
        },
        {
          "uid": 222,
          "name": "李四",
          "age": 27
        }
      ]
    }
    ```
6.  避免使用重复的 key


### JSON 属性值准则

属性值必须是 Unicode 的 booleans（布尔）, 数字(numbers), 字符串(strings), 对象(objects), 数组(arrays) 或 null；


### 空值处理

接口遵循“输入宽容，输出严格”原则，输出的数据结构中空字段的值一律为 `null`, 是 `null` 不是字符串 `"null"`；


#### 后端给前端传递

当数据为空的时候，基本上有以下三种处理方案

1.  接口返回 null。当一条记录不存在的时候，接口可以返回一个null值给前端，前端判断为null的时候，就知道这条数据是没有的。

2.  接口不返回该字段（目前运用比较广泛的处理方案）。前端读取到这个字段的时候会返回 undefined，前端同学在需要用到这个字段的时候，可以灵活处理数据：
    ```
    //直接使用该字段的时候，如果字段不存在，可以预设一个默认值。
    const KEY = data.key || 'key';
    
    //涉及需要二次处理该字段数据的时候，则加上一个判断
    if ( 'key' in data ) {
      console.log(data.key);
    }
    ```
3.  接口返回该字段类型的默认“空值”(也是一个运用的比较广泛的方法)。这里的“空值”是指，根据原来设定的数据类型，返回一个初始默认值，如果原来是个数组，那么“空值”就应该是[]


#### 前端给后端传递

*   在添加时，若前端未传递某个字段，后端把该字段置空。

*   在更新时，若前端未传递某个字段，后端应保持该字段原有值不变。如果想把该字段置空，则应传递该字段的空值。

参考

1.  阿里巴巴Java开发手册，手册明确要求：  
    “接口入参应明确必填/选填，未传选填参数时，后端需保持原有值不变。”  
    该规范旨在减少因字段缺失导致的业务逻辑错误，尤其适用于订单、用户信息等关键数据操作。  

2.  Spring 框架的默认行为
    使用Spring MVC时，若实体类字段未标注@RequestParam(required=false)且前端未传值，框架会忽略该字段而非置空。例如：

    ```java
    @PutMapping("/user")
    public ResponseEntity<?> updateUser(@RequestBody User user) {
        // 若前端未传user.getNickname()，则数据库中该字段值不变
        userRepository.save(user); 
        return ResponseEntity.ok().build();
    }
    ```

3.  JSON-API规范：“客户端未包含的字段应被视为未修改，服务端需保留原值。”
    该规范被GitHub、Shopify等公司广泛采用，确保API行为的可预测性。


## Response 数据格式建议

### 基本数据格式

```
{
    "code": 200,
    "data": "",
    "message": "success"
}
```


### 实体格式

```
{
    "code": 200,
    "message": "success",
    "data": {
        "entity": {
            "id": 1,
            "name": "XXX",
            "phone": "XXX"
        }
    }
}
```

*entity: 响应返回的实体数据*


### 列表格式

```
{
    "code": 200,
    "message": "success",
    "data": {
        "list":[
            {
                "id": 1,
                "name": "XXX",
                "code": "XXX"
            },
            {
                "id": 2,
                "name": "XXX",
                "code": "XXX"
            }
        ]
    }
}
```

*list: 响应返回的列表数据*


### 分页格式

```
{
    "code": 200,
    "message":"success",
    "data": {
        "totalCount": 2, // 总记录数
        "totalPage": 1  // 总页数
        "pageNo": 1, // 当前页码
        "pageSize": 10, // 每页大小
        "list":[
            {
                "id": 1,
                "name": "XXX",
                "code": "XXX"
            },
            {
                "id": 2,
                "name": "XXX",
                "code": "XXX"
            }
        ],
    }
}
```


### 特殊数据格式

对于特定组件数据格式由后端统一处理后返回前端，如（echart、ztree等组件）


### 特殊内容规范

*   布尔类型：一律返回 `BOOLEN` 类型值
*   日期格式：一律使用字符串，具体日期格式（使用时间戳还是日期）因业务而定


## reference

*   https://github.com/paddingme/Frontend-Backend-Separation/blob/master/api.md
*   https://google.aip.dev/193