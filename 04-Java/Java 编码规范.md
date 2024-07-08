---
title: Java 编码规范
subtitle: 
author: Lepeng
catalog: true
comments: true
header-img: img/header_img/archive-bg.jpg
tags:
- Python
categories:
- Python
date: 2024-03-30
link:
---

## (一) 命名风格

1.  【强制】代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。

    *   反例：`_name` / `__name` / `$name` / `name_` / `name$`
    *   正例：name

2.  【强制】代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。 

    说明：正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式也要避免采用。

    *   正例：inspur、CNY 等国际通用的名称，可视同英文。
    *   反例：`DaZhePromotion [打折]` / `getPingfenByName() [评分]` / `int 某变量 = 3` 

3.  【强制】类名使用 UpperCamelCase 风格，但以下情形例外：`DO / BO / DTO / VO / AO / PO / UID` 等。

    *   正例：`MarcoPolo / UserDO / XmlService / TcpUdpDeal`
    *   反例：`macroPolo / UserDo / XMLService / TCPUDPDeal`

4.  【强制】方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从驼峰形式。

    *   正例：`localValue / getHttpMessage() / inputUserId`

5.  【强制】常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。

    *   正例：`MAX_STOCK_COUNT`（最大库存量）
    *   反例：`MAX_COUNT`

6.  【强制】抽象类命名使用 Abstract开头；异常类命名使用 Exception 结尾；测试类命名以它要测试的类的名称开始，以 Test 结尾。

7.  【强制】类型与中括号紧挨相连来表示数组。

    *   正例：定义整形数组 `int[] arrayDemo`; 
    *   反例：在 main 参数中，使用 `String args[]` 来定义。 

8.  【强制】POJO 类中布尔类型的变量，都不要加 is 前缀，否则部分框架解析会引起序列化错误。

    *   反例：定义为基本数据类型 Boolean isDeleted 的属性，它的方法也是 `isDeleted()`，RPC框架在反向解析的时候，"误以为"对应的属性名称是 deleted，导致属性获取不到，进而抛 出异常。

9.  【强制】包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。

    *   正例：应用工具类包名为 com.inspur.util、类名为 MessageUtils（此规则参考 spring的框架结构）

10. 【强制】避免在子父类的成员变量之间、或者不同代码块的局部变量之间采用完全相同的命名，使可读性降低。

    说明：子类、父类成员变量名相同，即使是public类型的变量也是能够通过编译，而局部变量在同一方法内的不同代码块中同名也是合法的，但是要避免使用。对于非 `setter/getter` 的参数名称也要避免与成员变量名称相同。

    *   反例：
    
    ```plain
    public class ConfusingName {  
        public int age;  // 非setter/getter的参数名称，不允许与本类成员变量同名  
        public void getData(String alibaba) {  
            if(condition) {  
                final int money = 531;  // ...  
            }   
            for (int i = 0; i < 10; i++) {  
                // 在同一方法体中，不允许与其它代码块中的money命名相同  
                final int money = 615;  // ...  
            }  
        } 
    }  
    class Son extends ConfusingName {  
        // 不允许与父类的成员变量名称相同
        public int age; 
    } 
    ```

11. 【强制】杜绝完全不规范的缩写，避免望文不知义。

    说明：像 impl、info、del、msg 等这种约定俗成的可以简写，其他的不能简写。
    
    *   反例：AbstractClass "缩写"命名成 AbsClass；condition "缩写"命名成 condi，此类随意缩写严重降低了代码的可阅读性。

12. 【推荐】为了达到代码自解释的目标，任何自定义编程元素在命名时，使用尽量完整的单词组合来表达其意。

    *   正例：在JDK中，表达原子更新的类名为：AtomicReferenceFieldUpdater。
    *   反例：int a，b，c 诸如此类的随意命名方式。

13. 【推荐】如果模块、接口、类、方法使用了设计模式，在命名时需体现出具体模式。

    说明：将设计模式体现在名字中，有利于阅读者快速理解架构设计理念。
    
    *   正例：public class OrderFactory; public class LoginProxy; public class ResourceObserver;

14. 【推荐】接口类中的方法和属性不要加任何修饰符号（public 也不要加），保持代码的简洁性，并加上有效的 Javadoc 注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定是 与接口方法相关，并且是整个应用的基础常量。

    说明：JDK8 中接口允许有默认实现，那么这个 default 方法，是对所有实现类都有价值的默认实现。

    *   正例：接口方法签名 void commit(); 接口基础常量 `String COMPANY = "inspur"`;
    *   反例：接口方法定义 public abstract void f();

15. 接口和实现类的命名有两套规则：

    1.  【强制】对于 Service 和 DAO 类，基于 SOA 的理念，暴露出来的服务一定是接口，内部的实现类用 Impl 的后缀与接口区别。

        *   正例：CacheServiceImpl 实现 CacheService 接口。

    2.  【推荐】如果是形容能力的接口名称，取对应的形容词为接口名（通常是–able 的形式）。

        *   正例：AbstractTranslator 实现 Translatable 接口。

16. 【强制】枚举成员名称需要全大写，单词间用下划线隔开。

    说明：枚举其实就是特殊的类，域成员均为常量，且构造方法被默认强制是私有。

    *   正例：成员名称：`SUCCESS / UNKNOWN_REASON`。

17. 【参考】各层命名规约：

    1.  【推荐】包名和类名的命名规约，项目源码包以 `com.lepeng.项目名` 为根目录，下面包含 controller、 service、 repository 、 entity 4个子包， util、config 、constant根据需要添加。

        包结构概况：

        controller 控制器
        service    数据服务层
        repository 数据访问层
        entity     实体类
        util       工具类包
        config    配置类包
        constant  应用级常量类包
        
        说明：包中的类要以相应的包名为后缀，如果里面的类比较多，可以根据功能拆分成子包。如 controller 里面分为结算、账单、充值等功能，可以拆分成 cost、account、charge等子包。

    2. 【推荐】Service/DAO 层方法命名规约

        1.  获取单个对象的方法用 get 做前缀。
        2.  获取多个对象的方法用 list / get / find 做前缀，复数形式结尾如：listObjects。
        3.  获取统计值的方法用 count 做前缀。
        4.  插入的方法用 save / create 做前缀。
        5.  删除的方法用 delete 做前缀。
        6.  修改的方法用 update 做前缀。

## (二) 常量定义

1.  【强制】不允许任何魔法值（未经预先定义的常量）直接出现在代码中。以下场景除外：仅会出现一次的可以不定义常量。

2.  【强制】在 long 或者 Long 赋值时，数值后使用大写的 L，不能是小写的 l，小写容易跟数字1 混淆，造成误解。

    说明：`Long a = 2l`; 写的是数字的 21，还是 Long 型的 2，不易区分。

3.  【推荐】不要使用一个常量类维护所有常量，要按常量功能进行归类，分开维护。

    说明：大而全的常量类，杂乱无章，使用查找功能才能定位到修改的常量，不利于理解和维护。

    *   正例：缓存相关常量放在类 CacheConsts 下；系统配置相关常量放在类 ConfigConsts 下。

4. 【推荐】如果变量值仅在一个固定范围内变化用 enum 类型来定义。

    说明：如果存在名称之外的延伸属性应使用 enum 类型，下面正例中的数字就是延伸信息，表 示一年中的第几个季节。

    *   正例：
        
        ```plain
        public enum Season {  
            SPRING(1), SUMMER(2), AUTUMN(3), WINTER(4);   
            private int seq;  
            Season(int seq) {  
                this.seq = seq;  
            }  
            public int getSeq() {  
                return seq;  
            } 
        }
    ```


## (三) 代码格式（提交代码前，必须格式化）

1.  【强制】大括号的使用约定。如果是大括号内为空，则简洁地写成 `{}` 即可，不需要换行；如果 是非空代码块则：

    1.  左大括号前不换行。
    2.  左大括号后换行。
    3.  右大括号前换行。
    4.  右大括号后还有 else 等代码则不换行；表示终止的右大括号后必须换行。

    正例：
    
    ```plain
    Object response;
    if (response == null) {
      ...
    } else {
      ...
    }
    ```

2.  【强制】左小括号和字符之间不出现空格；同样，右小括号和字符之间也不出现空格；而左大括号前需要空格。详见第 5 条下方正例提示。

    *   反例：`if (空格 a == b 空格)`

3.  【强制】`if/for/while/switch/do` 等保留字与括号之间都必须加空格。

4.  【强制】任何二目、三目运算符的左右两边都需要加一个空格。

    说明：运算符包括赋值运算符=、逻辑运算符&&、加减乘除符号等。

5.  【强制】采用 4 个空格缩进，禁止使用 tab 字符。

    说明：如果使用 tab 缩进，必须设置 1 个 tab 为 4 个空格。IDEA 设置 tab 为 4 个空格时， 请勿勾选 Use tab character；而在 eclipse 中，必须勾选 insert spaces for tabs。

    *   正例： （涉及 1-5 点）
    
    ```plain
    public static void main(String[] args) { 
        // 缩进4个空格 
        String say = "hello"; 
        // 运算符的左右必须有一个空格 
        int flag = 0; 
        // 关键词if与括号之间必须有一个空格，括号内的f与左括号，0与右括号不需要空格 
        if (flag == 0) { 
            System.out.println(say); 
        }  
        // 左大括号前加空格且不换行；左大括号后换行 
        if (flag == 1) { 
            System.out.println("world"); 
        // 右大括号前换行，右大括号后有else，不用换行 
        } else { 
            System.out.println("ok"); 
        // 在右大括号后直接结束，则必须换行 
        } 
    }
    ```

6.  【强制】注释的双斜线与注释内容之间有且仅有一个空格。

    ```text
    // 这是示例注释，请注意在双斜线之后有一个空格
    String ygb = new String();
    ```

7.  【强制】在进行类型强制转换时，右括号与强制转换值之间不需要任何空格隔开。

    *   正例： `long first = 1000000000000L; int second = (int)first + 2;`

8.  【强制】单行字符数限制不超过 120 个，超出需要换行，换行时遵循如下原则：

    1.  第二行相对第一行缩进 4 个空格，从第三行开始，不再继续缩进，参考示例。
    2.  运算符与下文一起换行。
    3.  方法调用的点符号与下文一起换行。
    4.  方法调用中的多个参数需要换行时，在逗号后进行。
    5.  在括号前不要换行，见反例。

    *   正例：
    
        ```plain
        StringBuffer sb = new StringBuffer();
        // 超过 120 个字符的情况下，换行缩进 4 个空格，点号和方法名称一起换行
        sb.append("li").append("si")...
            .append("inspur")...
            .append("inspur")...
            .append("inspur"); 
        ```

    *   反例：
        
        ```plain
        StringBuffer sb = new StringBuffer();
        // 超过 120 个字符的情况下，不要在括号前换行
        sb.append("zi").append("xin")...append ("huang"); 
        // 参数很多的方法调用可能超过 120 个字符，不要在逗号前换行
        method(args1, args2, args3, ...
        , argsX);
        ```

9.  【强制】方法参数在定义和传入时，多个参数逗号后边必须加空格。

    *   正例：下例中实参的 args1，后边必须要有一个空格。method(args1, args2, args3);

10. 【强制】IDE 的 text file encoding 设置为 UTF-8; IDE 中文件的换行符使用 Unix 格式， 不要使用 Windows 格式。

11. 【强制】单个方法的总行数不超过 100 行。

    说明：包括方法签名、结束右大括号、方法内代码、注释、空行、回车及任何不可见字符的总行数不超过 80 行。

    *   正例：代码逻辑分清红花和绿叶，个性和共性，绿叶逻辑单独出来成为额外方法，使主干代码更加清晰；共性逻辑抽取成为共性方法，便于复用和维护。

12. 【推荐】没有必要增加若干空格来使某一行的字符与上一行对应位置的字符对齐。

    *   正例：

        ```plain
        int one = 1;
        long two = 2L;
        float three = 3F;
        StringBuffer sb = new StringBuffer();
        ```

    说明：增加 sb 这个变量，如果需要对齐，则给 a、b、c 都要增加几个空格，在变量比较多的 情况下，是非常累赘的事情。

13. 【推荐】不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性。

    说明：任何情形，没有必要插入多个空行进行隔开。方法与方法之间也只有1行。


## (四) OOP 规约

1.  【强制】避免通过一个类的对象引用访问此类的静态变量或静态方法，无谓增加编译器解析成本，直接用类名来访问即可。

2.  【强制】所有的覆写方法，必须加 `@Override` 注解。

    说明：`getObject()`与 `get0bject()` 的问题。一个是字母的 O，一个是数字的 0，加 `@Override` 可以准确判断是否覆盖成功。另外，如果在抽象类中对方法签名进行修改，其实现类会马上编译报错。

3.  【强制】相同参数类型，相同业务含义，才可以使用 Java 的可变参数，避免使用 Object。

    说明：可变参数必须放置在参数列表的最后。（提倡同学们尽量不用可变参数编程）

    *   正例：`public List<User> listUsers(String type, Long... ids) {...}`

4.  【强制】外部正在调用或者二方库依赖的接口，不允许修改方法签名，避免对接口调用方产生影响。接口过时必须加 `@Deprecated` 注解，并清晰地说明采用的新接口或者新服务是什么。

5.  【强制】不能使用过时的类或方法。

    说明：`java.net.URLDecoder` 中的方法 `decode(String encodeStr)` 这个方法已经过时，应该使用双参数 `decode(String source, String encode)`。接口提供方既然明确是过时接口，那么有义务同时提供新的接口；作为调用方来说，有义务去考证过时方法的新实现是什么。

6.  【强制】Object 的 equals 方法容易抛空指针异常，应使用常量或确定有值的对象来调用equals。

    *   正例：`"test".equals(object);`
    *   反例：`object.equals("test");`

    说明：推荐使用 `java.util.Objects#equals`（JDK7 引入的工具类）

7.  【强制】所有的相同类型的包装类对象之间值的比较，全部使用 equals 方法比较。

    说明：对于 `Integer var = ?` 在 \-128 至 127 范围内的赋值，Integer 对象是在 IntegerCache.cache 产生，会复用已有对象，这个区间内的 Integer 值可以直接使用==进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑，推荐使用 equals 方法进行判断。

8.  【强制】浮点数之间的等值判断，基本数据类型不能用 `==` 来比较，包装数据类型不能用 `equals` 来判断。

    说明：浮点数采用“尾数+阶码”的编码方式，类似于科学计数法的“有效数字+指数”的表示方式。二进制无法精确表示大部分的十进制小数，具体原理参考《码出高效》。

    *   反例：
        
        ```plain
            float a = 1.0f - 0.9f;  
            float b = 0.9f - 0.8f; 
          
            if (a == b) {  
                // 预期进入此代码快，执行其它业务逻辑  
                // 但事实上a==b的结果为false  
            }   
        
            Float x = Float.valueOf(a);  
            Float y = Float.valueOf(b);  
            if (x.equals(y)) {  
                // 预期进入此代码快，执行其它业务逻辑  
                // 但事实上equals的结果为false  
            } 
        ```

    1.  指定一个误差范围，两个浮点数的差值在此范围之内，则认为是相等的。
    
        ```plain
        float a = 1.0f - 0.9f; 
        float b = 0.9f - 0.8f; 
        float diff = 1e-6f; 
        if (Math.abs(a - b) < diff) { 
            System.out.println("true"); 
        }
        ```

    2.  使用BigDecimal来定义值，再进行浮点数的运算操作。 
        
        ```plain
        BigDecimal a = new BigDecimal("1.0"); 
        BigDecimal b = new BigDecimal("0.9"); 
        BigDecimal c = new BigDecimal("0.8"); 
        BigDecimal x = a.subtract(b); 
        BigDecimal y = b.subtract(c); 
        if (x.equals(y)) { 
            System.out.println("true"); 
        }
        ```

9.  【强制】定义数据对象DO类时，属性类型要与数据库字段类型相匹配。

    *   正例：数据库字段的bigint必须与类属性的Long类型相对应。
    *   反例：某个案例的数据库表id字段定义类型bigint unsigned，实际类对象属性为Integer，随着id越来越大，超过Integer的表示范围而溢出成为负数。

10. 【强制】为了防止精度损失，禁止使用构造方法 `BigDecimal(double)` 的方式把double值转化为BigDecimal对象。

    说明：`BigDecimal(double)` 存在精度损失风险，在精确计算或值比较的场景中可能会导致业务逻辑异常。
   
    如：`BigDecimal g = new BigDecimal(0.1f)`; 实际的存储值为：0.10000000149

    *   正例：优先推荐入参为String的构造方法，或使用BigDecimal的valueOf方法，此方法内部其实执行了Double的toString，而Double的toString按double的实际能表达的精度对尾数进行了截断。

        `BigDecimal recommend1 = new BigDecimal("0.1");`
        `BigDecimal recommend2 = BigDecimal.valueOf(0.1);`

11. 关于基本数据类型与包装数据类型的使用标准如下：

    1.  【强制】所有的 POJO 类属性必须使用包装数据类型。  
    2.  【强制】RPC 方法的返回值和参数必须使用包装数据类型。  
    3.  【推荐】所有的局部变量使用基本数据类型。
    
    说明：POJO 类属性没有初值是提醒使用者在需要使用时，必须自己显式地进行赋值，任何NPE 问题，或者入库检查，都由使用者来保证。
    
    *   正例：数据库的查询结果可能是 null，因为自动拆箱，用基本数据类型接收有 NPE 风险。 
    *   反例：比如显示成交总额涨跌情况，即正负 x%，x 为基本数据类型，调用的 RPC 服务，调用 不成功时，返回的是默认值，页面显示为 0%，这是不合理的，应该显示成中划线。所以包装 数据类型的 null 值，能够表示额外的信息，如：远程调用失败，异常退出。

12. 【强制】序列化类新增属性时，请不要修改 serialVersionUID 字段，避免反序列失败；如果完全不兼容升级，避免反序列化混乱，那么请修改 serialVersionUID 值。

    说明：注意 serialVersionUID 不一致会抛出序列化运行时异常。

13. 【强制】构造方法里面禁止加入任何业务逻辑，如果有初始化逻辑，请放在 init 方法中。

14. 【推荐】POJO 类推荐写 toString 方法。使用 IDE 中的工具：`source> generate toString` 时，如果继承了另一个 POJO 类，注意在前面加一下 super.toString。

    说明：在方法执行抛出异常时，可以直接调用 POJO 的 toString()方法打印其属性值，便于排 查问题。

15. 【强制】禁止在 POJO 类中，同时存在对应属性 xxx 的 isXxx()和 getXxx()方法。

    说明：框架在调用属性 xxx 的提取方法时，并不能确定哪个方法一定是被优先调用到。

16. 【推荐】使用索引访问用String的split方法得到的数组时，需做最后一个分隔符后有无内容的检查，否则会有抛IndexOutOfBoundsException的风险。

    说明： `String str = "a,b,c,,"; String[] ary = str.split(","); // 预期大于3，结果是3 System.out.println(ary.length);`

17. 【推荐】当一个类有多个构造方法，或者多个同名方法，这些方法应该按顺序放置在一起，便于阅读，此条规则优先于下一条。

18. 【推荐】类内方法定义的顺序依次是：`公有方法或保护方法 > 私有方法 > getter/setter 方法`。

    说明：公有方法是类的调用者和维护者最关心的方法，首屏展示最好；保护方法虽然只是子类 关心，也可能是"模板设计模式"下的核心方法；而私有方法外部一般不需要特别关心，是一个 黑盒实现；因为承载的信息价值较低，所有 Service 和 DAO 的 getter/setter 方法放在类体最后。

19. 【强制】setter 方法中，参数名称与类成员变量名称一致，`this.成员名 = 参数名`。在getter/setter 方法中，不要增加业务逻辑，增加排查问题的难度。

    *   反例：
    
        ```plain
        public Integer getData() { 
            if (condition) { 
                return this.data + 100; 
            } else { 
                return this.data - 100; 
            } 
        } 
        ```

20. 【推荐】循环体内，字符串的连接方式，使用 StringBuilder 的 append 方法进行扩展。

    说明：下例中，反编译出的字节码文件显示每次循环都会 new 出一个 StringBuilder 对象， 然后进行 append 操作，最后通过 toString 方法返回 String 对象，造成内存资源浪费。

    *   反例：
        
        ```plain
        String str = "start";
        for (int i = 0; i < 100; i++) { 
            str = str + "hello";
        }
        ```

21. 【推荐】final 可以声明类、成员变量、方法、以及本地变量，下列情况使用 final 关键字：
    
    1.  不允许被继承的类，如：String 类。
    2.  不允许修改引用的域对象。
    3.  不允许被重写的方法，如：POJO 类的 setter 方法。
    4.  不允许运行过程中重新赋值的局部变量。
    5. 避免上下文重复使用一个变量，使用 final 描述可以强制重新定义一个变量，方便更好 地进行重构。

22. 【推荐】慎用 Object 的 clone 方法来拷贝对象。

    说明：对象的 clone 方法默认是浅拷贝，若想实现深拷贝需要重写 clone 方法实现域对象的 深度遍历式拷贝。

23. 【推荐】类成员与方法访问控制从严：

    1.  如果不允许外部直接通过new来创建对象，那么构造方法必须是private。
    2.  工具类不允许有public或default构造方法。
    3.  类非static成员变量并且与子类共享，必须是protected。
    4. 类非static成员变量并且仅在本类使用，必须是private。
    5.  类static成员变量如果仅在本类使用，必须是private。
    6.  若是static成员变量，考虑是否为final。
    7.  类成员方法只供类内部调用，必须是private。
    8.  类成员方法只对继承类公开，那么限制为protected。

    说明：任何类、方法、参数、变量，严控访问范围。过于宽泛的访问范围，不利于模块解耦。思考：如果是一个private的方法，想删除就删除，可是一个public的service成员方法或成员变量，删除一下，不得手心冒点汗吗？变量像自己的小孩，尽量在自己的视线内，变量作用域太大，无限制的到处跑，那么你会担心的。


## (五) 集合处理

1.  【强制】关于 hashCode 和 equals 的处理，遵循如下规则：

    1.  只要重写 equals，就必须重写 hashCode。
    2.  因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的 对象必须重写这两个方法。
    3.  如果自定义对象作为 Map 的键，那么必须重写 hashCode 和 equals。

    ```plain
    Student st1 = new Student("wei","man");
    Student st2 = new Student("wei","man"); 
    
    // 正常理解这两个对象再存入到hashMap中应该是相等的，但如果不重写 hashcode（）方法的话，比较仅仅是其地址，不相等！
    
    // HashMap中的比较key是这样的，先求出key的hashcode(),比较其值是否相等，若相等再比较equals(),若相等则认为他们是相等 的。若equals()不相等则认为他们不相等。如果只重写hashcode()不重写equals()方法，当比较equals()时只是看他们是否为 同一对象（即进行内存地址的比较）,所以必定要两个方法一起重写。
    ```

    说明：String 重写了 hashCode 和 equals 方法，所以我们可以非常愉快地使用 String 对象 作为 key 来使用。

2.  【强制】ArrayList 的 subList 结果不可强转成 ArrayList，否则会抛出 ClassCastException 异常，即 java.util.RandomAccessSubList cannot be cast to java.util.ArrayList。

    说明：subList 返回的是 ArrayList 的内部类 SubList，并不是 ArrayList 而是 ArrayList 的一个视图，对于 SubList 子列表的所有操作最终会反映到原列表上。

3.  【强制】使用Map的方法keySet()/values()/entrySet()返回集合对象时，不可以对其进行添加元素操作，否则会抛出UnsupportedOperationException异常。

4.  【强制】Collections类返回的对象，如：emptyList()/singletonList()等都是immutable list，不可对其进行添加或者删除元素的操作。

    *    反例：如果查询无结果，返回Collections.emptyList()空集合对象，调用方一旦进行了添加元素的操作，就会触发UnsupportedOperationException异常。

5.  【强制】在 subList 场景中，高度注意对原集合元素的增加或删除，均会导致子列表的遍历、 增加、删除产生 ConcurrentModificationException 异常。

6.  【强制】使用集合转数组的方法，必须使用集合的toArray(T\[\] array)，传入的是类型完全一致、长度为0的空数组。

    *   反例：直接使用toArray无参方法存在问题，此方法返回值只能是Object\[\]类，若强转其它类型数组将出现ClassCastException错误。

    *   正例： List<String> list = new ArrayList<>(2); list.add("guan"); list.add("bao"); String\[\] array = list.toArray(new String\[0\]);

    说明：使用toArray带参方法，数组空间大小的length：

    1.  等于0，动态创建与size相同的数组，性能最好。
    2.  大于0但小于size，重新创建大小等于size的数组，增加GC负担。
    3.  等于size，在高并发情况下，数组创建完成之后，size正在变大的情况下，负面影响与上相同。
    4.  大于size，空间浪费，且在size处插入null值，存在NPE隐患。

7.  【强制】在使用Collection接口任何实现类的addAll()方法时，都要对输入的集合参数进行NPE判断。

    说明：在ArrayList#addAll方法的第一行代码即Object\[\] a = c.toArray(); 其中c为输入集合参数，如果为null，则直接抛出异常。

8.  【强制】使用工具类Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方法，它的add/remove/clear方法会抛出UnsupportedOperationException异常。

    说明：asList的返回对象是一个Arrays内部类，并没有实现集合的修改方法。Arrays.asList体现的是适配器模式，只是转换接口，后台的数据仍是数组。

    `String[] str = new String[] { "yang", "hao" }; List list = Arrays.asList(str);`

    第一种情况：`list.add("yangguanbao")`; 运行时异常。

    第二种情况：`str[0] = "changed"`; 也会随之修改，反之亦然。

9.  【强制】泛型通配符<? extends T>来接收返回的数据，此写法的泛型集合不能使用add方法，而<? super T>不能使用get方法，作为接口调用赋值时易出错。验证下

    说明：扩展说一下PECS(Producer Extends Consumer Super)原则：第一、频繁往外读取内容的，适合用<? extends T>。第二、经常往里插入的，适合用<? super T>

10. 【强制】在无泛型限制定义的集合赋值给泛型限制的集合时，在使用集合元素时，需要进行instanceof判断，避免抛出ClassCastException异常。

    说明：毕竟泛型是在JDK5后才出现，考虑到向前兼容，编译器是允许非泛型集合与泛型集合互相赋值。

    *   反例： 

        ```text
        List<String> generics = null; 
        List notGenerics = new ArrayList(10); 
        notGenerics.add(new Object()); 
        notGenerics.add(new Integer(1)); 
        generics = notGenerics; 
        // 此处抛出ClassCastException异常 
        String string = generics.get(0);
        ```

11. 【强制】不要在 foreach 循环里进行元素的 remove/add 操作。remove 元素请使用 Iterator方式，如果并发操作，需要对 Iterator 对象加锁。

    *   正例：

        ```text
        List<String> list = new ArrayList<>(); 
        list.add("1");
        list.add("2");
        Iterator<String> iterator = list.iterator(); 
        while (iterator.hasNext()) {
            String item = iterator.next(); 
            if (删除元素的条件) {
                iterator.remove();
            }
        }
        ```

    *   反例：

        ```text
        for (String item : list) { 
            if ("1".equals(item)) {
                list.remove(item);
            }
        }
        ```

    说明：以上代码的执行结果肯定会出乎大家的意料，那么试一下把"1"换成"2"，会是同样的结果吗？

12. 【强制】 在 JDK7 版本及以上，Comparator 实现类要满足如下三个条件，不然 Arrays.sort， Collections.sort 会报 IllegalArgumentException 异常。

    说明：三个条件如下
    
    1.  x，y 的比较结果和 y，x 的比较结果相反。
    2.  x>y，y>z，则 x>z。
    3.  x=y，则 x，z 比较结果和 y，z 比较结果相同。

    *   反例：下例中没有处理相等的情况，交换两个对象判断结果并不互反，不符合第一个条件，在实际使用中 可能会出现异常。
    
        ```text
        new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) { 
                return o1.getId() > o2.getId() ? 1 : -1;
            }
        };
        ```

13. 【推荐】集合泛型定义时，在JDK7及以上，使用diamond语法或全省略。   

    说明：菱形泛型，即 diamond，直接使用<>来指代前边已经指定的类型。   

    *   正例：
        ```
        // diamond 方式，即<>  
        HashMap<String, String> userCache = new HashMap<>(16);
        
        // 全省略方式
        
        ArrayList<User> users = new ArrayList(10);
        ```

14. 【推荐】集合初始化时，指定集合初始值大小。

    说明：HashMap 使用 HashMap(int initialCapacity) 初始化。  

    *   正例：initialCapacity = (需要存储的元素个数 / 负载因子) + 1。注意负载因子（即 loader factor）默认为 0.75，如果暂时无法确定初始值大小，请设置为 16（即默认值）。   
    *   反例：HashMap 需要放置 1024 个元素，由于没有设置容量初始大小，随着元素不断增加，容 量 7 次被迫扩大，resize 需要重建 hash 表，严重影响性能。

15. 【推荐】使用 entrySet 遍历 Map 类集合 KV，而不是 keySet 方式进行遍历。   

    说明：keySet 其实是遍历了 2 次，一次是转为 Iterator 对象，另一次是从 hashMap 中取出 key 所对应的 value。而 entrySet 只是遍历了一次就把 key 和 value 都放到了 entry 中，效 率更高。如果是 JDK8，使用 Map.foreach 方法。  

    *   正例：values()返回的是 V 值集合，是一个 list 集合对象；keySet()返回的是 K 值集合，是 一个 Set 集合对象；entrySet()返回的是 K-V 值组合集合。

16. 【推荐】高度注意 Map 类集合 K/V 能不能存储 null 值的情况，如下表格：

    <table class="drag-preview-element" style="min-width: 0px;"><colgroup><col><col><col><col><col></colgroup><tbody><tr><td><div class="child" data-type="editor-container" data-container-id="_TQFJXn6p" id="ones-editor-container-_TQFJXn6p"><div class="container-blocks"><div class="text-block" id="_KYDC3ZNY" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text style-bold">集合类</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_H7qYhY2y" id="ones-editor-container-_H7qYhY2y"><div class="container-blocks"><div class="text-block" id="_Wg23vKmr" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_jFyMDQAc"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text style-bold">Key</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_MTCXaooe" id="ones-editor-container-_MTCXaooe"><div class="container-blocks"><div class="text-block" id="_7fwFdeyo" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_NtXoV62D"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text style-bold">Value</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_W8kRKWnR" id="ones-editor-container-_W8kRKWnR"><div class="container-blocks"><div class="text-block" id="_V21S1RrL" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_8nfiffbr"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text style-bold">Super</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_ADaBowQh" id="ones-editor-container-_ADaBowQh"><div class="container-blocks"><div class="text-block" id="_Lge5om1J" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text style-bold">说明</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_VfeQzmpG" id="ones-editor-container-_VfeQzmpG"><div class="container-blocks"><div class="text-block" id="_ABRi8qwj" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_NcKYMRiy"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">Hashtable</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_5XuknST7" id="ones-editor-container-_5XuknST7"><div class="container-blocks"><div class="text-block" id="_7vtSNqJ4" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 69, 0);">不允许为</span><span class="text"> </span><span class="text" style="color: rgb(255, 69, 0);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_Scp6eHo1" id="ones-editor-container-_Scp6eHo1"><div class="container-blocks"><div class="text-block" id="_5aGXcVzb" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 69, 0);">不允许为</span><span class="text"> </span><span class="text" style="color: rgb(255, 69, 0);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_PWLCKfSS" id="ones-editor-container-_PWLCKfSS"><div class="container-blocks"><div class="text-block" id="_MTHz8cMY" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_u2p91XLU"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">Dictionary</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_Lxsh9G3K" id="ones-editor-container-_Lxsh9G3K"><div class="container-blocks"><div class="text-block" id="_Q3uxjuL8" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">线程安全</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_2oHftZpn" id="ones-editor-container-_2oHftZpn"><div class="container-blocks"><div class="text-block" id="_JCkZYUgD" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_bMPFcicQ"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">ConcurrentHashMap</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_QU4QuYeS" id="ones-editor-container-_QU4QuYeS"><div class="container-blocks"><div class="text-block" id="_B7jDcmWF" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 69, 0);">不允许为</span><span class="text"> </span><span class="text" style="color: rgb(255, 69, 0);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_2U9Nyv8T" id="ones-editor-container-_2U9Nyv8T"><div class="container-blocks"><div class="text-block" id="_DuLhLAa5" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 69, 0);">不允许为</span><span class="text"> </span><span class="text" style="color: rgb(255, 69, 0);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_CYzHNSwU" id="ones-editor-container-_CYzHNSwU"><div class="container-blocks"><div class="text-block" id="_THYEtE4f" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_XMK6bvmD"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">AbstractMap</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_5JMs3whi" id="ones-editor-container-_5JMs3whi"><div class="container-blocks"><div class="text-block" id="_FeGcYHFo" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">锁分段技术（JDK8:CAS）</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_QAEEJBdf" id="ones-editor-container-_QAEEJBdf"><div class="container-blocks"><div class="text-block" id="_5X4eWu47" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_SqBPL8KS"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">TreeMap</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_5yfxJuvD" id="ones-editor-container-_5yfxJuvD"><div class="container-blocks"><div class="text-block" id="_KNUgz3Cj" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 0, 0);">不允许为</span><span class="text"> </span><span class="text" style="color: rgb(255, 0, 0);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_5eet9h53" id="ones-editor-container-_5eet9h53"><div class="container-blocks"><div class="text-block" id="_57cSXLaJ" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(0, 0, 255);">允许为</span><span class="text"> </span><span class="text" style="color: rgb(0, 0, 255);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_K9dyiQji" id="ones-editor-container-_K9dyiQji"><div class="container-blocks"><div class="text-block" id="_GnUqtKBi" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_NnhMbSXK"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">AbstractMap</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_EmJFYQjQ" id="ones-editor-container-_EmJFYQjQ"><div class="container-blocks"><div class="text-block" id="_YDsgGteM" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">线程不安全</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_797dNJaB" id="ones-editor-container-_797dNJaB"><div class="container-blocks"><div class="text-block" id="_LZUHc2d2" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_QzaCfKRF"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">HashMap</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_J4GJhETf" id="ones-editor-container-_J4GJhETf"><div class="container-blocks"><div class="text-block" id="_SU7ZJeGv" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(0, 0, 255);">允许为</span><span class="text"> </span><span class="text" style="color: rgb(0, 0, 255);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_XKAsY4Gv" id="ones-editor-container-_XKAsY4Gv"><div class="container-blocks"><div class="text-block" id="_41mER38N" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(0, 0, 255);">允许为</span><span class="text"> </span><span class="text" style="color: rgb(0, 0, 255);">null</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_PL1c7fnu" id="ones-editor-container-_PL1c7fnu"><div class="container-blocks"><div class="text-block" id="_13PMa7Cb" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="br-box" data-type="editor-box" data-box-type="br" id="_MPo9mkEF"><span class="box-space-start"></span><span data-type="box-content"><img src="assets/1712625354-765a4ea26cc5f9535c6fb22513f55a4d.svg"><br></span><span class="box-space-start"></span></span><span class="text">AbstractMap</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_TNMmrPVr" id="ones-editor-container-_TNMmrPVr"><div class="container-blocks"><div class="text-block" id="_EjxhMMyd" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">线程不安全</span></div></div></div></div></td></tr></tbody></table>

    *   反例： 由于 HashMap 的干扰，很多人认为 ConcurrentHashMap 是可以置入 null 值，而事实上， 存储 null 值时会抛出 NPE 异常。

17. 【参考】合理利用好集合的有序性(sort)和稳定性(order)，避免集合的无序性(unsort)和 不稳定性(unorder)带来的负面影响。   

    说明：有序性是指遍历的结果是按某种比较规则依次排列的。稳定性指集合每次遍历的元素次 序是一定的。如：ArrayList 是 order/unsort；HashMap 是 unorder/unsort；TreeSet 是order/sort。

18. 【参考】利用 Set 元素唯一的特性，可以快速对一个集合进行去重操作，避免使用 List 的contains 方法进行遍历、对比、去重操作。


## (六) 并发处理

1. 【强制】获取单例对象需要保证线程安全，其中的方法也要保证线程安全。

说明：资源驱动类、工具类、单例工厂类都需要注意。

2. 【强制】创建线程或线程池时请指定有意义的线程名称，方便出错时回溯。

正例：自定义线程工厂，并且根据外部特征进行分组，比如机房信息。

```plain
public class UserThreadFactory implements ThreadFactory {  
	private final String namePrefix; 
	private final AtomicInteger nextId = new AtomicInteger(1);  

	// 定义线程组名称，在jstack问题排查时，非常有帮助 
	UserThreadFactory(String whatFeaturOfGroup) {  
		namePrefix = "From UserThreadFactory's " + whatFeaturOfGroup + "-Worker-"; 
	} 
	
	@Override public Thread newThread(Runnable task) {
		String name = namePrefix + nextId.getAndIncrement();  
		Thread thread = new Thread(null, task, name, 0, false); 
		System.out.println(thread.getName());  
		return thread; 
	} 
} 
```

Java

  

3. 【强制】线程资源必须通过线程池提供，不允许在应用中自行显式创建线程。

说明：使用线程池的好处是减少在创建和销毁线程上所消耗的时间以及系统资源的开销，解决 资源不足的问题。如果不使用线程池，有可能造成系统创建大量同类线程而导致消耗完内存或 者"过度切换"的问题。

4.【强制】线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让研发人员更加明确线程池的运行规则，规避资源耗尽的风险。

说明：Executors 返回的线程池对象的弊端如下：

1）FixedThreadPool 和 SingleThreadPool:

允许的请求队列长度为 Integer.MAX\_VALUE，可能会堆积大量的请求，从而导致 OOM。

2）CachedThreadPool 和 ScheduledThreadPool:

允许的创建线程数量为 Integer.MAX\_VALUE，可能会创建大量的线程，从而导致 OOM。

5\. 【强制】SimpleDateFormat 是线程不安全的类，一般不要定义为static变量，如果定义为static，必须加锁，或者使用DateUtils工具类。

正例：注意线程安全，使用DateUtils。亦推荐如下处理：

```plain
private static final ThreadLocal<DateFormat> df = new ThreadLocal<DateFormat>() { 
	@Override 
	protected DateFormat initialValue() { 
		return new SimpleDateFormat("yyyy-MM-dd"); 
	} 
};
```

Java

说明：如果是JDK8的应用，可以使用Instant代替Date，LocalDateTime代替Calendar，DateTimeFormatter代替SimpleDateFormat，官方给出的解释：simple beautiful strong immutable thread-safe。

6\. 【强制】必须回收自定义的ThreadLocal变量，尤其在线程池场景下，线程经常会被复用，如果不清理自定义的 ThreadLocal变量，可能会影响后续业务逻辑和造成内存泄露等问题。尽量在代理中使用try-finally块进行回收。

正例： 

```plain
objectThreadLocal.set(userInfo); 
try { 
	// ... 
} finally { 
	objectThreadLocal.remove(); 
}
```

Java

7. 【强制】高并发时，同步调用应该去考量锁的性能损耗。能用无锁数据结构，就不要用锁；能 锁区块，就不要锁整个方法体；能用对象锁，就不要用类锁。

说明：尽可能使加锁的代码块工作量尽可能的小，避免在锁代码块中调用 RPC 方法。

8. 【强制】对多个资源、数据库表、对象同时加锁时，需要保持一致的加锁顺序，否则可能会造成死锁。

说明：线程一需要对表 A、B、C 依次全部加锁后才可以进行更新操作，那么线程二的加锁顺序 也必须是 A、B、C，否则可能出现死锁。

9\. 【强制】在使用阻塞等待获取锁的方式中，必须在try代码块之外，并且在加锁方法与try代码块之间没有任何可能抛出异常的方法调用，避免加锁成功后，在finally中无法解锁。

说明一：如果在lock方法与try代码块之间的方法调用抛出异常，那么无法解锁，造成其它线程无法成功获取锁。

说明二：如果lock方法在try代码块之内，可能由于其它方法抛出异常，导致在finally代码块中，unlock对未加锁的对象解锁，它会调用AQS的tryRelease方法（取决于具体实现类），抛出IllegalMonitorStateException异常。

说明三：在Lock对象的lock方法实现中可能抛出unchecked异常，产生的后果与说明二相同。

正例： 

```plain
Lock lock = new XxxLock(); 
// ... 
lock.lock(); 
try { 
	doSomething(); 
	doOthers(); 
} finally { 
	lock.unlock(); 
}
```

Java

反例： 

```plain
Lock lock = new XxxLock(); 
// ... 
try { 
	// 如果此处抛出异常，则直接执行finally代码块 
	doSomething(); 
	// 无论加锁是否成功，finally代码块都会执行 
	lock.lock(); 
	doOthers(); 
} finally { 
	lock.unlock(); }
```

Java

  

10\. 【强制】在使用尝试机制来获取锁的方式中，进入业务代码块之前，必须先判断当前线程是否持有锁。锁的释放规则与锁的阻塞等待方式相同。

说明：Lock对象的unlock方法在执行时，它会调用AQS的tryRelease方法（取决于具体实现类），如果当前线程不持有锁，则抛出IllegalMonitorStateException异常。

正例： 

```plain
Lock lock = new XxxLock(); 
// ... 
boolean isLocked = lock.tryLock(); 
if (isLocked) { 
	try { 
		doSomething(); 
		doOthers(); 
	} finally { 
		lock.unlock(); 
	} 
}
```

Java

11\. 【强制】并发修改同一记录时，避免更新丢失，需要加锁。要么在应用层加锁，要么在缓存加锁，要么在数据库层使用乐观锁，使用 version 作为更新依据。   


说明：如果每次访问冲突概率小于 20%，推荐使用乐观锁，否则使用悲观锁。乐观锁的重试次数不得小于 3 次。

12\. 【强制】多线程并行处理定时任务时，Timer运行多个TimeTask时，只要其中之一没有捕获抛出的异常，其它任务便会自动终止运行，如果在处理定时任务时使用ScheduledExecutorService则没有这个问题。

13\. 【推荐】资金相关的金融敏感信息，使用悲观锁策略。 说明：乐观锁在获得锁的同时已经完成了更新操作，校验逻辑容易出现漏洞，另外，乐观锁对冲突的解决策略有较复杂的要求，处理不当容易造成系统压力或数据异常，所以资金相关的金融敏感信息不建议使用乐观锁更新。

14\. 【推荐】使用 CountDownLatch 进行异步转同步操作，每个线程退出前必须调用 countDown 方法，线程执行代码注意 catch 异常，确保 countDown 方法被执行到，避免主线程无法执行 至 await 方法，直到超时才返回结果。  


说明：注意，子线程抛出异常堆栈，不能在主线程 try-catch 到。

15\. 【推荐】避免Random实例被多线程使用，虽然共享该实例是线程安全的，但会因竞争同一seed 导致的性能下降。

说明：Random实例包括java.util.Random 的实例或者 Math.random()的方式。

正例：在JDK7之后，可以直接使用API ThreadLocalRandom，而在 JDK7之前，需要编码保证每个线程持有一个实例。

16\. 【推荐】在并发场景下，通过双重检查锁（double-checked locking）实现延迟初始化的优化问题隐患(可参考 The "Double-Checked Locking is Broken" Declaration)，推荐解 决方案中较为简单一种（适用于 JDK5 及以上版本），将目标属性声明为 volatile 型。   


反例：

```plain
class LazyInitDemo {
	private Helper helper = null; 
	public Helper getHelper() {
	if (helper == null) synchronized(this) { 
		if (helper == null)
			helper = new Helper();
		}
		return helper;
	}
	// other methods and fields...
}
```

Java

17\. 【参考】volatile解决多线程内存不可见问题。对于一写多读，是可以解决变量同步问题，但是如果多写，同样无法解决线程安全问题。

说明：如果是count++操作，使用如下类实现：AtomicInteger count = new AtomicInteger(); count.addAndGet(1);

 如果是JDK8，推荐使用LongAdder对象，比AtomicLong性能更好（减少乐观锁的重试次数）。

18. 【参考】 HashMap 在容量不够进行 resize 时由于高并发可能出现死链，导致 CPU 飙升，在 开发过程中可以使用其它数据结构或加锁来规避此风险。

19. 【参考】ThreadLocal对象使用static修饰，来避免内存浪费。 

说明：这个变量是针对一个线程内所有操作共享的，所以设置为静态变量，所有此类实例共享 此静态变量 ，也就是说在类第一次被使用时装载，只分配一块存储空间，所有此类的对象(只 要是这个线程内定义的)都可以操控这个变量。

每个线程往ThreadLocal中读写数据是线程隔离，互相之间不会影响的，所以ThreadLocal无法解决共享对象的更新问题。


## (七) 控制语句

1\. 【强制】在一个switch块内，每个case要么通过continue/break/return等来终止，要么注释说明程序将继续执行到哪一个case为止；在一个switch块内，都必须包含一个default语句并且放在最后，即使它什么代码也没有。

说明：注意break是退出switch语句块，而return是退出方法体。

2\. 【强制】当switch括号内的变量类型为String并且此变量为外部参数时，必须先进行null判断。

反例：猜猜下面的代码输出是什么？ 

  

```plain
public class SwitchString { 
	public static void main(String[] args) { 
		method(null); 
	} 
	public static void method(String param) { 
		switch (param) { 
			// 肯定不是进入这里 
			case "sth": 
				System.out.println("it's sth"); 
				break; 
			// 也不是进入这里 
			case "null": 
				System.out.println("it's null"); 
				break; 
			// 也不是进入这里 
			default: 
				System.out.println("default"); 
		} 
	} 
}
```

Java

  

3\. 【强制】在if/else/for/while/do语句中必须使用大括号。

说明：即使只有一行代码，避免采用单行的编码方式：if (condition) statements;

4. 【强制】在高并发场景中，避免使用"等于"判断作为中断或退出的条件。

说明：如果并发控制没有处理好，容易产生等值判断被"击穿"的情况，使用大于或小于的区间 判断条件来代替。

反例：判断剩余奖品数量等于 0 时，终止发放奖品，但因为并发处理错误导致奖品数量瞬间变 成了负数，这样的话，活动无法终止。

5. 【推荐】表达异常的分支时，少用 if-else 方式，这种方式可以改写成：

```plain
if (condition) {
	...
	return obj;
}
// 接着写 else 的业务逻辑代码;
```

Java

说明：如果非得使用 if()...else if()...else...方式表达逻辑，避免后续代码维 护困难，请勿超过 3 层。

正例：超过3层的 if-else 的逻辑判断代码可以使用卫语句、策略模式、状态模式等来实现，其中卫语句即代码逻辑先考虑失败、异常、中断、退出等直接返回的情况，以方法多个出口的方式，解决代码中判断分支嵌套的问题，这是逆向思维的体现。 实例如下：

```plain
public void findBoyfriend(Man man) {  
	if (man.isUgly()) {  
		System.out.println("本姑娘是外貌协会的资深会员");  
		return;  
	}   
	if (man.isPoor()) {  
		System.out.println("贫贱夫妻百事哀");  
		return;  
	}   
	if (man.isBadTemper()) {  
		System.out.println("银河有多远，你就给我滚多远");  
		return;  
	}  
	System.out.println("可以先交往一段时间看看"); 
} 
```

Java

  

6. 【推荐】除常用方法（如 getXxx/isXxx）等外，不要在条件判断中执行其它复杂的语句，将 复杂逻辑判断的结果赋值给一个有意义的布尔变量名，以提高可读性。

说明：很多 if 语句内的逻辑相当复杂，阅读者需要分析条件表达式的最终结果，才能明确什么 样的条件执行什么样的语句，那么，如果阅读者分析逻辑表达式错误呢？

正例：

```plain
// 伪代码如下
final boolean existed = (file.open(fileName, "w") != null) && (...) || (...); 
if (existed) {
	...
}
```

Java

反例：

```plain
if ((file.open(fileName, "w") != null) && (...) || (...)) {
	...
}
```

Java

  
7\. 【推荐】不要在其它表达式（尤其是条件表达式）中，插入赋值语句。

说明：赋值点类似于人体的穴位，对于代码的理解至关重要，所以赋值语句需要清晰地单独成为一行。

反例： public Lock getLock(boolean fair) { // 算术表达式中出现赋值操作，容易忽略count值已经被改变 threshold = (count = Integer.MAX\_VALUE) - 1; // 条件表达式中出现赋值操作，容易误认为是sync==fair return (sync = fair) ? new FairSync() : new NonfairSync(); }

8. 【推荐】循环体中的语句要考量性能，以下操作尽量移至循环体外处理，如定义对象、变量、 获取数据库连接，进行不必要的 try-catch 操作（这个 try-catch 是否可以移至循环体外）。

9. 【推荐】避免采用取反逻辑运算符。

说明：取反逻辑不利于快速理解，并且取反逻辑写法必然存在对应的正向逻辑写法。

正例：使用 if (x < 628) 来表达 x 小于 628。

反例：使用 if (!(x \>= 628)) 来表达 x 小于 628。

10. 【参考】下列情形，需要进行参数校验：

1） 需要极高稳定性和可用性的方法。

2） 对外提供的开放接口，不管是 RPC/API/HTTP 接口。

3） 敏感权限入口。

11. 【参考】下列情形，不需要进行参数校验：

1） 极有可能被循环调用的方法，减少不必要的性能损耗。但在方法说明里必须注明外部参数检查要求。

2） 被声明成 private 只会被自己代码所调用的方法，如果能够确定调用方法的代码传入参 数已经做过检查或者肯定不会有问题，此时可以不校验参数。


## (八) 注释规约

 【推荐】代码中的注释全部使用英文，禁止使用中文。

1. 【强制】类、类属性、类方法的注释必须使用 Javadoc 规范，使用 /\*内容\*/ 格式，不得使用 // xxx 方式。

说明：在 IDE 编辑窗口中，Javadoc 方式会提示相关注释，生成 Javadoc 可以正确输出相应注释；在 IDE 中，工程调用方法时，不进入方法即可悬浮提示方法、参数、返回值的意义，提高 阅读效率。

2. 【强制】所有的抽象方法（包括接口中的方法）必须要用 Javadoc 注释、除了返回值、参数、 异常说明外，还必须指出该方法做什么事情，实现什么功能。

说明：对子类的实现要求，或者调用注意事项，请一并说明。

3. 【强制】所有的类都必须添加创建者和创建日期。

4. 【强制】方法内部单行注释，在被注释语句上方另起一行，使用 // 注释 。方法内部多行注释 使用 /\* \*/ 注释，注意与代码对齐。

5. 【强制】所有的枚举类型字段必须要有注释，说明每个数据项的用途。

6\. 【推荐】与其“半吊子”英文来注释，不如用中文注释把问题说清楚。专有名词与关键字保持英文原文即可。

反例：“TCP连接超时”解释成“传输控制协议连接超时”，理解反而费脑筋。

7. 【强制】代码修改的同时，注释也要进行相应的修改，尤其是参数、返回值、异常、核心逻辑 等的修改。

说明：代码与注释更新不同步，就像路网与导航软件更新不同步一样，如果导航软件严重滞后， 就失去了导航的意义。

8. 【参考】谨慎注释掉代码。在上方详细说明，而不是简单地注释掉。如果无用，则删除。

说明：代码被注释掉有两种可能性：

1）后续会恢复此段代码逻辑。

2）永久不用。

前者如果没有备注信息，难以知晓注释动机。后者建议直接删掉（代码仓库保存了历史代码）。

9. 【参考】对于注释的要求：

第一、能够准确反应设计思想和代码逻辑；

第二、能够描述业务含 义，使别的程序员能够迅速了解到代码背后的信息。完全没有注释的大段代码对于阅读者形同 天书，注释是给自己看的，即使隔很长时间，也能清晰理解当时的思路；注释也是给继任者看 的，使其能够快速接替自己的工作。

10. 【参考】好的命名、代码结构是自解释的，注释力求精简准确、表达到位。避免出现注释的 一个极端：过多过滥的注释，代码的逻辑一旦修改，修改注释是相当大的负担。

反例：

```plain
// put elephant into fridge
put(elephant, fridge);
```

Java

方法名 put，加上两个有意义的变量名 elephant 和 fridge，已经说明了这是在干什么，语义清晰的代码不需要额外的注释。

11. 【参考】特殊注释标记，请注明标记人与标记时间。注意及时处理这些标记，通过标记扫描， 经常清理此类标记。线上故障有时候就是来源于这些标记处的代码。

1） 待办事宜（TODO）:（标记人，标记时间，\[预计处理时间\]）

表示需要实现，但目前还未实现的功能。这实际上是一个Javadoc的标签，目前的Javadoc还没 有实现，但已经被广泛使用。只能应用于类，接口和方法（因为它是一个Javadoc标签）。

2） 错误，不能工作（FIXME）:（标记人，标记时间，\[预计处理时间\]）

在注释中用FIXME标记某代码是错误的，而且不能工作，需要及时纠正的情况。


## (九) 其它

1. 【强制】在使用正则表达式时，利用好其预编译功能，可以有效加快正则匹配速度。

说明：不要在方法体内定义：Pattern pattern \= Pattern.compile("规则");

2. 【强制】注意 Math.random() 这个方法返回是 double 类型，注意取值的范围 0≤x<1（能够 取到零值，注意除零异常），如果想获取整数类型的随机数，不要将 x 放大 10 的若干倍然后 取整，直接使用 Random 对象的 nextInt 或者 nextLong 方法。

3. 【强制】获取当前毫秒数 System.currentTimeMillis(); 而不是 new Date().getTime();

说明：如果想获取更加精确的纳秒级时间值，使用 System.nanoTime()的方式。在 JDK8 中， 针对统计时间等场景，推荐使用 Instant 类。

4\. 【强制】日期格式化时，传入pattern中表示年份统一使用小写的y。

说明：日期格式化时，yyyy表示当天所在的年，而大写的YYYY代表是week in which year（JDK7之后引入的概念），意思是当天所在的周属于的年份，一周从周日开始，周六结束，只要本周跨年，返回的YYYY就是下一年。

另外需要注意：

a. 表示月份是大写的M

b. 表示分钟则是小写的m

c. 24小时制的是大写的H

d.12小时制的则是小写的h

正例：表示日期和时间的格式如下所示： new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

5. 【推荐】任何数据结构的构造或初始化，都应指定大小，避免数据结构无限增长吃光内存。

6. 【推荐】及时清理不再使用的代码段或配置信息。

说明：对于垃圾代码或过时配置，坚决清理干净，避免程序过度臃肿，代码冗余。

正例：对于暂时被注释掉，后续可能恢复使用的代码片断，在注释代码上方，统一规定使用三个斜杠 (///) 来说明注释掉代码的理由。

二、异常日志

A


## (一) 异常处理

A

1. 【强制】Java 类库中定义的可以通过预检查方式规避的 RuntimeException 异常不应该通过 catch 的方式来处理，比如：NullPointerException，IndexOutOfBoundsException 等等。

说明：无法通过预检查的异常除外，比如，在解析字符串形式的数字时，不得不通过 catch NumberFormatException 来实现。

正例：if (obj != null) {...}

反例：try { obj.method(); } catch (NullPointerException e) {…}

2. 【强制】异常不要用来做流程控制，条件控制。

说明：异常设计的初衷是解决程序运行中的各种意外情况，且异常的处理效率比条件判断方式 要低很多。

3. 【强制】catch 异常时，请分清稳定代码和非稳定代码，稳定代码指的是无论如何不会出错的代码。 对于非稳定代码的 catch 尽可能进行区分异常类型，再做对应的异常处理。

说明：对大段代码进行 try\-catch，使程序无法根据不同的异常做出正确的应激反应，也不利 于定位问题，这是一种不负责任的表现。

正例：用户注册的场景中，如果用户输入非法字符，或用户名称已存在，或用户输入密码过于 简单，在程序上作出分门别类的判断，并提示给用户。

4. 【强制】捕获异常是为了处理它，不要捕获了却什么都不处理而抛弃之，如果不想处理它，请 将该异常抛给它的调用者。最外层的业务使用者，必须处理异常，将其转化为用户可以理解的 内容。

5. 【强制】有 try 块放到了事务代码中，catch 异常后，如果需要回滚事务，一定要注意手动回滚事务。

6. 【强制】finally 块必须对资源对象、流对象进行关闭，有异常也要做 try-catch。

说明：如果 JDK7 及以上，可以使用 try-with-resources 方式。

7. 【强制】不要在 finally 块中使用 return。

说明：try块中的return语句执行成功后，并不马上返回，而是继续执行finally块中的语句，如果此处存在return语句，则在此直接返回，无情丢弃掉try块中的返回点。

反例： 

```text
private int x = 0; 
public int checkReturn() { 
	try { 
		// x等于1，此处不返回 
		return ++x; 
	} finally { 
		// 返回的结果是2 
		return ++x; 
	} 
}
```

Plain Text

8. 【强制】捕获异常与抛异常，必须是完全匹配，或者捕获异常是抛异常的父类。

说明：如果预期对方抛的是绣球，实际接到的是铅球，就会产生意外情况。

9\. 【强制】在调用RPC、二方包、或动态生成类的相关方法时，捕捉异常必须使用Throwable类来进行拦截。

说明：通过反射机制来调用方法，如果找不到方法，抛出NoSuchMethodException。什么情况会抛出NoSuchMethodError呢？二方包在类冲突时，仲裁机制可能导致引入非预期的版本使类的方法签名不匹配，或者在字节码修改框架（比如：ASM）动态创建或修改类时，修改了相应的方法签名。这些情况，即使代码编译期是正确的，但在代码运行期时，会抛出NoSuchMethodError。

10. 【推荐】方法的返回值可以为 null，不强制返回空集合，或者空对象等，必须添加注释充分 说明什么情况下会返回 null 值。

说明：本手册明确防止 NPE 是调用者的责任。即使被调用方法返回空集合或者空对象，对调用者来说，也并非高枕无忧，必须考虑到远程调用失败、序列化失败、运行时异常等场景返回  
null 的情况。

11. 【推荐】防止 NPE，是程序员的基本修养，注意 NPE 产生的场景：

1）返回类型为基本数据类型，return 包装数据类型的对象时，自动拆箱有可能产生 NPE。

反例：public int f() { return Integer 对象}， 如果为 null，自动解箱抛 NPE。

2） 数据库的查询结果可能为 null。

3） 集合里的元素即使 isNotEmpty，取出的数据元素也可能为 null。

4） 远程调用返回对象时，一律要求进行空指针判断，防止 NPE。

5） 对于 Session 中获取的数据，建议 NPE 检查，避免空指针。

6） 级联调用 obj.getA().getB().getC()；一连串调用，易产生 NPE。

正例：使用 JDK8 的 Optional 类来防止 NPE 问题。

12. 【推荐】定义时区分 unchecked / checked 异常，避免直接抛出 new RuntimeException()， 更不允许抛出 Exception 或者 Throwable，应使用有业务含义的自定义异常。推荐业界已定义 过的自定义异常，如：DAOException / ServiceException 等。

13. 【参考】对于公司外的 http/api 开放接口必须使用 "错误码" ；而应用内部推荐异常抛出； 跨应用间 RPC 调用优先考虑使用 Result 方式，封装 isSuccess()方法、"错误码"、"错误简 短信息"。

说明：关于 RPC 方法返回方式使用 Result 方式的理由：

1）使用抛异常返回方式，调用方如果没有捕获到就会产生运行时错误。

2）如果不加栈信息，只是 new 自定义异常，加入自己的理解的 error message，对于调用 端解决问题的帮助不会太多。如果加了栈信息，在频繁调用出错的情况下，数据序列化和传输 的性能损耗也是问题。

14.【参考】避免出现重复的代码（Don't Repeat Yourself），即 DRY 原则。

说明：随意复制和粘贴代码，必然会导致代码的重复，在以后需要修改时，需要修改所有的副 本，容易遗漏。必要时抽取共性方法，或者抽象公共类，甚至是组件化。

正例：一个类中有多个 public 方法，都需要进行数行相同的参数校验操作，这个时候请抽取：

private boolean checkParam(DTO dto) {...}


## (二) 日志规约

A

1. 【强制】应用中不可直接使用日志系统（Log4j、Logback）中的 API，而应依赖使用日志框架 SLF4J 中的 API，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。

```text
import org.slf4j.Logger; 
import org.slf4j.LoggerFactory;  
private static final Logger logger = LoggerFactory.getLogger(Test.class); 
```

Plain Text

说明：slf4j日志框架中，打印日志时已经对日志级别做了开关判断，无需应用侧主动判断。（对于没有日志级别判断的日志实现， 需要在应用侧主动判断）

2\. 【强制】所有日志文件至少保存15天，因为有些异常具备以“周”为频次发生的特点。

 网络运行状态、安全相关信息、系统监测、管理后台操作、用户敏感操作需要留存相关的网络日志不少于6个月。

3\. 【强制】在日志输出时，字符串变量之间的拼接使用占位符的方式。

说明：因为String字符串的拼接会使用StringBuilder的append()方式，有一定的性能损耗。使用占位符仅是替换动作，可以有效提升性能。

正例：logger.debug("Processing trade with id: {} and symbol: {}", id, symbol);

4\. 【强制】避免重复打印日志，浪费磁盘空间，务必在log4j.xml中设置additivity=false。

正例：<logger name="com.inspur.mysql.config" additivity="false">

5\. 【强制】异常信息应该包括两类信息：案发现场信息和异常堆栈信息。如果不处理，那么通过关键字throws往上抛出。

正例：logger.error(各类参数或者对象toString() + "\_" + e.getMessage(), e);

6. 【推荐】谨慎地记录日志。生产环境禁止输出 debug 日志；有选择地输出 info 日志；如果使 用 warn 来记录刚上线时的业务行为信息，一定要注意日志输出量的问题，避免把服务器磁盘 撑爆，并记得及时删除这些观察日志。

说明：大量地输出无效日志，不利于系统性能提升，也不利于快速定位错误点。记录日志时请 思考：这些日志真的有人看吗？看到这条日志你能做什么？能不能给问题排查带来好处？

7. 【推荐】可以使用 warn 日志级别来记录用户输入参数错误的情况，避免用户投诉时，无所适 从。如非必要，请不要在此场景打出 error 级别，避免频繁报警。

说明：注意日志输出的级别，error 级别只记录系统逻辑出错、异常或者重要的错误信息。

8. 【推荐】尽量用英文来描述日志错误信息，如果日志中的错误信息用英文描述不清楚的话使用 中文描述即可，否则容易产生歧义。国际化团队或海外部署的服务器由于字符集问题，强制使用全英文来注释和描述日志错误信息。


## 三、单元测试

1\. 【强制】好的单元测试必须遵守AIR原则。

说明：单元测试在线上运行时，感觉像空气（AIR）一样并不存在，但在测试质量的保障上，却是非常关键的。好的单元测试宏观上来说，具有自动化、独立性、可重复执行的特点。 ⚫ A：Automatic（自动化） ⚫ I：Independent（独立性） ⚫ R：Repeatable（可重复）

2\. 【强制】单元测试应该是全自动执行的，并且非交互式的。测试用例通常是被定期执行的，执行过程必须完全自动化才有意义。输出结果需要人工检查的测试不是一个好的单元测试。单元测试中不准使用System.out来进行人肉验证，必须使用assert来验证。

3\. 【强制】保持单元测试的独立性。为了保证单元测试稳定可靠且便于维护，单元测试用例之间决不能互相调用，也不能依赖执行的先后次序。

反例：method2需要依赖method1的执行，将执行结果作为method2的输入。

4\. 【强制】单元测试是可以重复执行的，不能受到外界环境的影响。

说明：单元测试通常会被放到持续集成中，每次有代码check in时单元测试都会被执行。如果单测对外部环境（网络、服务、中间件等）有依赖，容易导致持续集成机制的不可用。

正例：为了不受外界环境影响，要求设计代码时就把SUT的依赖改成注入，在测试时用spring 这样的DI框架注入一个本地（内存）实现或者Mock实现。

5\. 【强制】对于单元测试，要保证测试粒度足够小，有助于精确定位问题。单测粒度至多是类级别，一般是方法级别。

说明：只有测试粒度小才能在出错时尽快定位到出错位置。单测不负责检查跨类或者跨系统的交互逻辑，那是集成测试的领域。

6\. 【强制】核心业务、核心应用、核心模块的增量代码确保单元测试通过。

说明：新增代码及时补充单元测试，如果新增代码影响了原有单元测试，请及时修正。

7\. 【强制】单元测试代码必须写在如下工程目录：src/test/java，不允许写在业务代码目录下。

说明：源码编译时会跳过此目录，而单元测试框架默认是扫描此目录。

8\. 【推荐】单元测试的基本目标：语句覆盖率达到70%；核心模块的语句覆盖率和分支覆盖率都要达到100%

说明：在工程规约的应用分层中提到的DAO层，Manager层，可重用度高的Service，都应该进行单元测试。

9\. 【推荐】编写单元测试代码遵守BCDE原则，以保证被测试模块的交付质量。

•

B：Border，边界值测试，包括循环边界、特殊取值、特殊时间点、数据顺序等。

•

C：Correct，正确的输入，并得到预期的结果。

•

D：Design，与设计文档相结合，来编写单元测试。

•

E：Error，强制错误信息输入（如：非法数据、异常流程、业务允许外等），并得到预期的结果。

10\. 【推荐】对于数据库相关的查询，更新，删除等操作，不能假设数据库里的数据是存在的，或者直接操作数据库把数据插入进去，请使用程序插入或者导入数据的方式来准备数据。

反例：删除某一行数据的单元测试，在数据库中，先直接手动增加一行作为删除目标，但是这一行新增数据并不符合业务插入规则，导致测试结果异常。

11\. 【推荐】对于不可测的代码在适当的时机做必要的重构，使代码变得可测，避免为了达到测试要求而书写不规范测试代码。

12\. 【推荐】在设计评审阶段，开发人员需要和测试人员一起确定单元测试范围，单元测试最好覆盖所有测试用例。

13\. 【推荐】单元测试作为一种质量保障手段，在项目提测前完成单元测试，不建议项目发布后补充单元测试用例。

14\. 【参考】为了更方便地进行单元测试，业务代码应避免以下情况：

•

构造方法中做的事情过多。

•

存在过多的全局变量和静态方法。

•

存在过多的外部依赖。

•

存在过多的条件语句。

说明：多层条件语句建议使用卫语句、策略模式、状态模式等方式重构。

15\. 【参考】不要对单元测试存在如下误解：

•

那是测试同学干的事情。本文是开发手册，凡是本文内容都是与开发同学强相关的。

•

单元测试代码是多余的。系统的整体功能与各单元部件的测试正常与否是强相关的。

•

单元测试代码不需要维护。一年半载后，那么单元测试几乎处于废弃状态。

•

单元测试与线上故障没有辩证关系。好的单元测试能够最大限度地规避线上故障。


## 四、安全规约

A

1\. 【强制】隶属于用户个人的页面或者功能必须进行权限控制校验（IAM）。

说明：防止没有做水平权限校验就可随意访问、修改、删除别人的数据，比如查看他人的私信内容、修改他人的订单。

2\. 【强制】用户敏感数据禁止直接展示，必须对展示数据进行脱敏。

说明：中国大陆个人手机号码显示为:137\*\*\*\*0969，隐藏中间4位，防止隐私泄露。

3\. 【强制】用户输入的SQL参数严格使用参数绑定或者METADATA字段值限定，防止SQL注入，禁止字符串拼接SQL访问数据库。

4\. 【强制】用户请求传入的任何参数必须做有效性验证。

说明：Java代码用正则来验证客户端的输入，有些正则写法验证普通用户输入没有问题，但是如果攻击人员使用的是特殊构造的字符串来验证，有可能导致死循环的结果。

如果忽略参数校验，可能导致如下情况：

1）page size过大导致内存溢出

2）恶意order by导致数据库慢查询

3）任意重定向

4）SQL注入

5）反序列化注入

6）正则输入源串拒绝服务ReDoS

5\. 【强制】禁止向HTML页面输出未经安全过滤或未正确转义的用户数据。

6\. 【强制】表单、AJAX提交必须执行CSRF安全验证。

说明：CSRF(Cross-site request forgery)跨站请求伪造是一类常见编程漏洞。对于存在CSRF漏洞的应用/网站，攻击者可以事先构造好URL，只要受害者用户一访问，后台便在用户不知情的情况下对数据库中用户参数进行相应修改。

7\. 【强制】在使用平台资源，譬如短信、邮件、电话、下单、支付，必须实现正确的防重放的机制，如数量限制、疲劳度控制、验证码校验，避免被滥刷而导致资损。

说明：如注册时发送验证码到手机，如果没有限制次数和频率，那么可以利用此功能骚扰到其它用户，并造成短信平台资源浪费。

8\. 【推荐】发贴、评论、发送即时消息等用户生成内容的场景必须实现防刷、文本内容违禁词过滤等风控策略。


## 五、设计规约

A

1\. 【强制】存储方案和底层数据结构的设计获得评审一致通过，并沉淀成为文档。

说明：有缺陷的底层数据结构容易导致系统风险上升，可扩展性下降，重构成本也会因历史数据迁移和系统平滑过渡而陡然增加，所以，存储方案和数据结构需要认真地进行设计和评审，生产环境提交执行后，需要进行double check。

正例：评审内容包括存储介质选型、表结构设计能否满足技术方案、存取性能和存储空间能否满足业务发展、表或字段之间的辩证关系、字段名称、字段类型、索引等；数据结构变更（如在原有表中新增字段）也需要进行评审通过后上线。

2\. 【强制】在需求分析阶段，如果与系统交互的User超过一类并且相关的Use Case超过5个，使用用例图来表达更加清晰的结构化需求。

3\. 【强制】如果某个业务对象的状态超过3个，使用状态图来表达并且明确状态变化的各个触发条件。

说明：状态图的核心是对象状态，首先明确对象有多少种状态，然后明确两两状态之间是否存在直接转换关系，再明确触发状态转换的条件是什么。

正例：淘宝订单状态有已下单、待付款、已付款、待发货、已发货、已收货等。比如已下单与已收货这两种状态之间是不可能有直接转换关系的。

4\. 【强制】如果系统中某个功能的调用链路上的涉及对象超过3个，使用时序图来表达并且明确各调用环节的输入与输出。

说明：时序图反映了一系列对象间的交互与协作关系，清晰立体地反映系统的调用纵深链路。

5\. 【强制】如果系统中模型类超过5个，并且存在复杂的依赖关系，使用类图来表达并且明确类之间的关系。

说明：类图像建筑领域的施工图，如果搭平房，可能不需要，但如果建造蚂蚁Z空间大楼，肯定需要详细的施工图。

6\. 【强制】如果系统中超过2个对象之间存在协作关系，并且需要表示复杂的处理流程，使用活动图来表示。

说明：活动图是流程图的扩展，增加了能够体现协作关系的对象泳道，支持表示并发等。

7\. 【推荐】需求分析与系统设计在考虑主干功能的同时，需要充分评估异常流程与业务边界。

反例：用户在淘宝付款过程中，银行扣款成功，发送给用户扣款成功短信，但是支付宝入款时由于断网演练产生异常，淘宝订单页面依然显示未付款，导致用户投诉。

8\. 【推荐】类在设计与实现时要符合单一原则。

说明：单一原则最易理解却是最难实现的一条规则，随着系统演进，很多时候，忘记了类设计的初衷。

9\. 【推荐】谨慎使用继承的方式来进行扩展，优先使用聚合/组合的方式来实现。

说明：不得已使用继承的话，必须符合里氏代换原则，此原则说父类能够出现的地方子类一定能够出现，比如，“把钱交出来”，钱的子类美元、欧元、人民币等都可以出现。

10\. 【推荐】系统设计时，根据依赖倒置原则，尽量依赖抽象类与接口，有利于扩展与维护。

说明：低层次模块依赖于高层次模块的抽象，方便系统间的解耦。

11\. 【推荐】系统设计时，注意对扩展开放，对修改闭合。

说明：极端情况下，交付线上生产环境的代码都是不可修改的，同一业务域内的需求变化，通过模块或类的扩展来实现。

12\. 【推荐】系统设计阶段，共性业务或公共行为抽取出来公共模块、公共配置、公共类、公共方法等，避免出现重复代码或重复配置的情况。

说明：随着代码的重复次数不断增加，维护成本指数级上升。

13\. 【推荐】避免如下误解：敏捷开发 = 讲故事 + 编码 + 发布。

说明：敏捷开发是快速交付迭代可用的系统，省略多余的设计方案，摒弃传统的审批流程，但核心关键点上的必要设计和文档沉淀是需要的。

反例：某团队为了业务快速发展，敏捷成了产品经理催进度的借口，系统中均是勉强能运行但像面条一样的代码，可维护性和可扩展性极差，一年之后，不得不进行大规模重构，得不偿失。

14\. 【参考】系统设计主要目的是明确需求、理顺逻辑、后期维护，次要目的用于指导编码。

说明：避免为了设计而设计，系统设计文档有助于后期的系统维护和重构，所以设计结果需要进行分类归档保存。

15\. 【参考】设计的本质就是识别和表达系统难点，找到系统的变化点，并隔离变化点。

说明：世间众多设计模式目的是相同的，即隔离系统变化点。

16\. 【参考】系统架构设计的目的：

•

确定系统边界。确定系统在技术层面上的做与不做。

•

确定系统内模块之间的关系。确定模块之间的依赖关系及模块的宏观输入与输出。

•

确定指导后续设计与演化的原则。使后续的子系统或模块设计在规定的框架内继续演化。

•

确定非功能性需求。非功能性需求是指安全性、可用性、可扩展性等。


## 六、代码审计关键字修改


在代码审计过程中，bugcleaner、fortify等工具会对代码中包含account 、 password、 pass、 credent 、secret关键字的代码进行相应漏洞标注。

针对以上情况需在编码过程中对包含这些关键字（函数名、变量名）的大小写任意组合比如Password、Credential等代码进行修改，。

现制定关键字对应规则：首尾字母+中间字母数字的个数。

<table class="row-title drag-preview-element" style="min-width: 0px;"><colgroup><col><col></colgroup><tbody><tr><td><div class="child" data-type="editor-container" data-container-id="_FLn6Y1sQ" id="ones-editor-container-_FLn6Y1sQ"><div class="container-blocks"><div class="text-block" id="_WVFLA1VQ" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">关键字如下</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_YcTWiTVD" id="ones-editor-container-_YcTWiTVD"><div class="container-blocks"><div class="text-block" id="_AR8Jyj6D" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">代码中同步修改如下</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_LqLNNky8" id="ones-editor-container-_LqLNNky8"><div class="container-blocks"><div class="text-block" id="_9yMW7dXB" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">account</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_9sBTUjDT" id="ones-editor-container-_9sBTUjDT"><div class="container-blocks"><div class="text-block" id="_Y1os8tBH" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 0, 0);">a5t</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_RcVBxXQe" id="ones-editor-container-_RcVBxXQe"><div class="container-blocks"><div class="text-block" id="_7JJsVTBw" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">password&nbsp;</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_FLjDjmTb" id="ones-editor-container-_FLjDjmTb"><div class="container-blocks"><div class="text-block" id="_VjjNJ2GL" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 0, 0);">p6d</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_T7zoZepb" id="ones-editor-container-_T7zoZepb"><div class="container-blocks"><div class="text-block" id="_2E4nDAeZ" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">pass</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_AFrapgjT" id="ones-editor-container-_AFrapgjT"><div class="container-blocks"><div class="text-block" id="_Rvtb4AVu" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 0, 0);">p2s</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_QrtEdJXh" id="ones-editor-container-_QrtEdJXh"><div class="container-blocks"><div class="text-block" id="_HAph3x9U" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">credent</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_D6hPHEPL" id="ones-editor-container-_D6hPHEPL"><div class="container-blocks"><div class="text-block" id="_J49ChZe9" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 0, 0);">c5t</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_H783Gm5w" id="ones-editor-container-_H783Gm5w"><div class="container-blocks"><div class="text-block" id="_K4D8A7c1" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">secret&nbsp;</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_SBTgua7p" id="ones-editor-container-_SBTgua7p"><div class="container-blocks"><div class="text-block" id="_CJzsML8U" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 0, 0);">s4t</span></div></div></div></div></td></tr><tr><td><div class="child" data-type="editor-container" data-container-id="_9CgdXaNr" id="ones-editor-container-_9CgdXaNr"><div class="container-blocks"><div class="text-block" id="_9p6z7SAY" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text">credential</span></div></div></div></div></td><td><div class="child" data-type="editor-container" data-container-id="_UoT87At5" id="ones-editor-container-_UoT87At5"><div class="container-blocks"><div class="text-block" id="_UjiMTpqr" data-type="editor-block" data-block-type="text"><div data-type="block-content"><span class="text" style="color: rgb(255, 0, 0);">&nbsp;c8l</span></div></div></div></div></td></tr></tbody></table>



