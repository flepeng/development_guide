# RESTful API 设计指南



经过『学生成长档案』、『新高考选课』以及『陕西教育资源平台』等项目前后端分离实践，积累了一定的 RESRful API 设计经验，遂将之整理为本文档。

- 本文档主要目的是为设计接口时提出建议，使大家不必重复造 HTTP 协议已经完成的轮子，其次规避之前在接口设计中所遇到的错误和缺陷，以及借鉴前人优秀的设计和经验;
- 文档中若有错误或者未提及部分，烦请指出以便更正。





##1. API 域名、前缀

一般地，数字校园项目 API 地址前缀为：`该项目名（上下文）+ /api`。如：

```
  http://192.168.0.216/xspj/api/grades  (学生评价项目获取年级列表);
  http://192.168.12.217/xgk/api/periods (新高考选科获取所有届)；
```

## 2. API 版本

因当前 API 设计还未涉及太多版本问题， 暂时将 API 版本置于请求 header 头内，字段为" x-api-version",版本为 1，如下图：

![Snip20160307_1](app/client/src/img/Snip20160307_1.png)￼

服务器端应该检测 API 请求版本，若 Request headers 没有 `x-api-version` 应该拒绝请求，版本错误应进行相应处理。

ps: 使用HTTP Header时，优先使用合适的标准头属性。用X-作为前缀自定义一个头属性，例如: `X-Result-Fields`。

## 3. 路径

路径又称"终点"（endpoint），表示API的具体网址，路由信息。

在 RESTful 架构中，每个路径代表一种资源（resource），路径指向的应该是唯一的资源对象。所以路径中不能有动词，只能有名词。动作应该通过 http 动词（get,delete,put,post等）来表达。一般来说，数据库中的表都是同种记录的"集合"(collection)，故API中的名词也应该使用复数。

RESTful 接口永远不要直接下行图片，图片文件等内容可走单独的 download 接口，通过 URL 去 CDN 服务器或者自己的文件服务器下载到本地。

- 使用短的、完整的并且是大家都知道的单词。如果某个部分中有一个破折号或是一个特殊的字符的话，这个词就有可能太长；
- 路径只能是名词，且应该是复数；
- 尽可能清晰的表达资源之间的关系；
- **使用中划线`-`，不使用`_`**；
- 参数列表应该被 encode 过。

如下路径设计所示：

|http verb|endpoint|description|
|---|---|---|
|GET| /posts| 获取所有的文章列表|
|POST| /posts | 创建一篇新的文章|
|GET| /posts/12 | 获取 id 为 12 的文章|
|GET| /posts/1,2,3 | 获取 id 为 1,2,3的文章|
|PUT| /posts/12 | 更新 id 为 12 的文章|
|DELETE| /posts/12 | 删除 id 为 12 的文章|
|GET| /posts/12/comments| 获取 id 为 12 的文章的所有评论|
|GET| /posts/12/comments/5| 获取 id 为 12 的 文章中 id 为 5 的评论|
|POST|/posts/12/comments | 为 id 为12 的文章创建一条心得评论 |
|PATCH|/posts/12/comments/5| 部分更新 id 为 12 的文章中 id 为 5 的评论|
|DELETE| /posts/12/comments/5 | 删除 id 为 12 的 文章中 id 为 5 的评论|



## 4. 过滤信息

|http verb|endpoint| description|
|---|---|---|
|GET|/posts?q=xxx| 搜索含有 xxx 的文章信息|
|GET|/posts?state=open| 获取所有状态为 open 的文章|
|GET|/posts?keyword=xxx| 获取所有关键词包含XXX的文章|
|GET|/posts?sort=-priority| 所有文章按照优先级倒序|
|GET|/posts?title=demo&author=pm| 获取名字为 demo  作者为 pm 的文章列表|
|GET|/posts?sort=votes&order=asc|获取所有文章按投票数升序排序|
|GET|/posts?page=10&per_page=100|获取第10页最多100篇文章|Get


## 5. HTTP 请求方法

关于方法语义的说明：

- `GET` 用于从服务器获取某个资源的信息
  + 完成请求后返回状态码 `200 OK`
  + 完成请求后需要返回被请求的资源详细信息
- `POST` 用于创建资源
  + 创建完成后返回状态码 `201 Created`
  + 完成请求后需要返回被创建的资源详细信息
- `PUT` 用于完整的替换资源或者创建指定身份的资源，比如创建 id 为 123 的某个资源
  + 如果是创建了资源，则返回 `201 Created`
  + 如果是替换了资源，则返回 `200 OK`
  + 完成请求后需要返回被修改的资源详细信息
- `DELETE` 用于删除某个资源
  + 完成请求后返回状态码 `204 No Content`
- `PATCH` 用于局部更新资源
  + 完成请求后返回状态码 `200 OK`
  + 完成请求后需要返回被修改的资源详细信息
- `OPTIONS` 用于获取资源支持的所有 HTTP 方法
- `HEAD` 用于只获取请求某个资源返回的头信息

关于方法参数的说明：

* `GET`,`DELETE`,`HEAD` 方法，参数风格为标准的 `GET` 风格的参数，如 `url?a=1&b=2`；
* `POST`,`PUT`,`PATCH`,`OPTION` 方法
  - 默认情况下请求实体会被视作标准 json 字符串进行处理，当然依旧推荐设置头信息的`Content-Type` 为 `application/json`；
  - 在一些特殊的接口中，此时允许`Content-Type` 为 `application/x-www-form-urlencoded` 或者 `multipart/form-data`,此时请求实体会被视作标准 `POST` 风格的参数进行处理。




## 6. 状态码

-  **请求成功**

  * 200 **OK** : 请求执行成功并返回相应数据，如 `GET` 成功
  * 201 **Created** : 对象创建成功并返回相应资源数据，如 `POST` 成功；创建完成后响应头中应该携带头标 `Location` ，指向新建资源的地址
  * 202 **Accepted** : 接受请求，但无法立即完成创建行为，比如其中涉及到一个需要花费若干小时才能完成的任务。返回的实体中应该包含当前状态的信息，以及指向处理状态监视器或状态预测的指针，以便客户端能够获取最新状态。
  * 204 **No Content** : 请求执行成功，不返回相应资源数据，如 `PATCH` ， `DELETE` 成功

-  **重定向**

  **重定向的新地址都需要在响应头 `Location` 中返回**

  * 301 **Moved Permanently** : 被请求的资源已永久移动到新位置
  * 302 **Found** : 请求的资源现在临时从不同的 URI 响应请求
  * 303 **See Other** : 对应当前请求的响应可以在另一个 URI 上被找到，客户端应该使用 `GET` 方法进行请求
  * 307 **Temporary Redirect** : 对应当前请求的响应可以在另一个 URI 上被找到，客户端应该保持原有的请求方法进行请求

- **条件请求**

  * 304 **Not Modified** : 资源自从上次请求后没有再次发生变化，主要使用场景在于实现数据缓存
  * 409 **Conflict** : 请求操作和资源的当前状态存在冲突。主要使用场景在于实现并发控制
  * 412 **Precondition Failed** : 服务器在验证在请求的头字段中给出先决条件时，没能满足其中的一个或多个。主要使用场景在于实现并发控制

- **客户端错误**

  * 400 **Bad Request** : 请求体包含语法错误
  * 401 **Unauthorized** : 需要验证用户身份，如果服务器就算是身份验证后也不允许客户访问资源，应该响应 `403 Forbidden`
  * 403 **Forbidden** : 服务器拒绝执行
  * 404 **Not Found** : 找不到目标资源
  * 405 **Method Not Allowed** : 不允许执行目标方法，响应中应该带有 `Allow` 头，内容为对该资源有效的 HTTP 方法
  * 406 **Not Acceptable** : 服务器不支持客户端请求的内容格式，但响应里会包含服务端能够给出的格式的数据，并在 `Content-Type` 中声明格式名称
  * 410 **Gone** : 被请求的资源已被删除，只有在确定了这种情况是永久性的时候才可以使用，否则建议使用 `404 Not Found`
  * 413 **Payload Too Large** : `POST` 或者 `PUT` 请求的消息实体过大
  * 415 **Unsupported Media Type** : 服务器不支持请求中提交的数据的格式
  * 422 **Unprocessable Entity** : 请求格式正确，但是由于含有语义错误，无法响应
  * 428 **Precondition Required** : 要求先决条件，如果想要请求能成功必须满足一些预设的条件

- **服务端错误**

  * 500 **Internal Server Error** : 服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。
  * 501 **Not Implemented** : 服务器不支持当前请求所需要的某个功能。
  * 502 **Bad Gateway** : 作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。
  * 503 **Service Unavailable** : 由于临时的服务器维护或者过载，服务器当前无法处理请求。这个状况是临时的，并且将在一段时间以后恢复。如果能够预计延迟时间，那么响应中可以包含一个 `Retry-After` 头用以标明这个延迟时间（内容可以为数字，单位为秒；或者是一个 [HTTP 协议指定的时间格式](http://tools.ietf.org/html/rfc2616#section-3.3)）。如果没有给出这个 `Retry-After` 信息，那么客户端应当以处理 500 响应的方式处理它。

  `501` 与 `405` 的区别是：`405` 是表示服务端不允许客户端这么做，`501` 是表示客户端或许可以这么做，但服务端还没有实现这个功能



## 7. 数据设计

使用JSON格式传输数据，JSON 是一种可以跨平台高扩展的轻量级的数据交换格式。易于人阅读和编写，同时也易于机器解析和生成。在 http 请求头和响应头申明 Content-Type。返回的数据结构应该做到尽可能简单，不要过于包装。响应状态应该包含在响应头中！


**Request**

```
Accept: application/json
Content-Type: application/json;charset=UTF-8
```

**Response**

```
Content-Type: application/json;charset=UTF-8

```

**错误的做法**

```
{
    "status": 200,
    "data": {
        "trade_id": 1234,
        "trade_name": "Bala bala"
    }
}
```

**正确的做法**

```
Response Headers:
    Status: 200
Response Body:
    {
        "trade_id": 1234,
        "trade_name": "Bala bala"
    }
```


**JSON 属性定义限制建议**

1. 属性名应该是具有定义语义的有意义的名称；

2. 首字母不能使用大写(大小写友好)；

3. **使用下划线**；

4.  属性和字符串值必须使用双引号`””`；

5. 数组类型应该是复数属性名。其它属性名都应该是单数。


6. 当JSON对象作为Map(映射)使用时，属性的名称命名规则并不适用，如：


  ```
{
    "id":"123",
    "status": {
        "组长":1,
        "主任":2,
        "校长":3
    }
}
  ```

**JSON 属性值准则**

- 属性值必须是Unicode 的 booleans（布尔）, 数字(numbers), 字符串(strings), 对象(objects), 数组(arrays), 或 null；
- 接口遵循“输入宽容，输出严格”原则，输出的数据结构中空字段的值一律为 `null`, 是 `null` 不是字符串 `"null"`；
- 如果一个属性是可选的或者包含空值或null值，考虑从JSON中去掉该属性，除非它的存在有很强的语义原因；
- 日期属性值推荐使用 RFC3339 建议的格式：

  ```
  {
    "lastUpdate": "2007-11-06T16:34:41.000Z"
  }
  ```
- 时间间隔属性值推荐使用 ISO 8601 建议的格式

  ```
  {
    // 三年, 6个月, 4天, 12小时,
    // 三十分钟, 5秒
    "duration": "P3Y6M4DT12H30M5S"
  }
  ```
- 分页返回的数据格式：

  ```
    // /posts?page=10&per_page=10 请求第10页的10条数据

    {
      "totalItems" : 100, // 总共有100条
      "data": [] // 返回请求的10条
    }
  ```
## 8. 错误处理


在调用接口的过程中，可能出现下列几种错误情况：

* 服务器维护中，`503` 状态码

    ```http
    HTTP/1.1 503 Service Unavailable
    Retry-After: 3600
    Content-Length: 41

    {"message": "Service In the maintenance"}
    ```

* 发送了无法转化的请求体，`400` 状态码

    ```http
    HTTP/1.1 400 Bad Request
    Content-Length: 35

    {"message": "Problems parsing JSON"}
    ```

* 服务到期（比如付费的增值服务等）， `403` 状态码

    ```http
    HTTP/1.1 403 Forbidden
    Content-Length: 29

    {"message": "Service expired"}
    ```

* 因为某些原因不允许访问（比如被 ban ），`403` 状态码

    ```http
    HTTP/1.1 403 Forbidden
    Content-Length: 29

    {"message": "Account blocked"}
    ```

* 权限不够，`403` 状态码

    ```http
    HTTP/1.1 403 Forbidden
    Content-Length: 31

    {"message": "Permission denied"}
    ```

* 需要修改的资源不存在， `404` 状态码

    ```http
    HTTP/1.1 404 Not Found
    Content-Length: 32

    {"message": "Resource not found"}
    ```

* 缺少了必要的头信息，`428` 状态码

    ```http
    HTTP/1.1 428 Precondition Required
    Content-Length: 35

    {"message": "Header User-Agent is required"}
    ```

* 发送了非法的资源，`422` 状态码

    ```http
    HTTP/1.1 422 Unprocessable Entity
    Content-Length: 149

    {
      "message": "Validation Failed",
      "errors": [
        {
          "resource": "Issue",
          "field": "title",
          "code": "required"
        }
      ]
    }
    ```

所有的 `error` 哈希表都有 `resource`, `field`, `code` 字段，以便于定位错误，`code` 字段则用于表示错误类型：

* `invalid`: 某个字段的值非法，接口文档中会提供相应的信息
* `required`: 缺失某个必须的字段
* `not_exist`: 说明某个字段的值代表的资源不存在
* `already_exist`: 发送的资源中的某个字段的值和服务器中已有的某个资源冲突，常见于某些值全局唯一的字段，比如 @ 用的用户名（这个错误我有纠结，因为其实有 409 状态码可以表示，但是在修改某个资源时，很一般显然请求中不止是一种错误，如果是 409 的话，多种错误的场景就不合适了）

## 9. 身份验证以及角色分配


所有 API 接口 除了文档特定支出的都需要通过身份验证。目前我们采用 JSON Web Token 来验证。其支持通过登录接口使用账号密码获取，在请求接口时使用`Authorization: Bearer #{token}` 头标或者 `token` 参数的值的方式进行验证。
在数校系统中，登录 datacenter 之后。打开 '/datacenter/open/autologin.do' 可以查看到该 token。如下图所示：

![](app/client/src/img/14574224673430.jpg)￼

在前后端分离中，项目前端无法获取用户角色信息，故目前如下设计获取角色信息以及 token:

1. 当登录 datacenter 之后，浏览器一旦加载完该项目前端页面之后，前端主动发起 `/who` 请求，如下所示：


  ![](app/client/src/img/14574226567374.jpg)￼


2. 从 `/who`请求拿到用户角色信息之后和 token；

  ![](app/client/src/img/14574226836656.jpg)￼

3. 再将 token 信息注入之后的所有 API 请求的 header 头里，通过服务器端身份验证，合法请求到数据。

  ![](app/client/src/img/14574228052610.jpg)￼



不同于传统的数校项目，前后端分离项目中都是通过此种方法获得用户角色信息和类型从而进行权限限制，让不同角色的人看见不同的菜单。注意，在设计 `/who` 返回用户类型`type`时，应该加入**角色分配管理员** 判断，并且关闭应用自身的菜单，前端将角色分配管理员页面 `/datacenter/role/showRolePage.do?appId=` 以 iframe 所有菜单让前端进行控制展示。




## 10.其他资料

- API 文档生成工具：

  [apiDoc](http://apidocjs.com/)

  api.js 撰写 api 接口生成API 文档，例如 ：
  - [学生成长档案文档](http://192.168.0.219/lzx/doc/xsczda/)
  - [新高考选科文档](http://192.168.0.219/lzx/doc/xgkxk/)
  - [企业号通知公告](http://192.168.0.219/lzx/doc/weixin/notice/)
  - 等等


- API 测试工具：

  [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)(需翻墙)





## 11.参考：


- [HTTP API 设计指南](https://github.com/ZhangBohan/http-api-design-ZH_CN)
- [HTTP 接口设计指北](https://github.com/bolasblack/http-api-guide)
- [JSON风格指南](https://github.com/darcyliu/google-styleguide/blob/master/JSONStyleGuide.md)
- [Github API V3](https://developer.github.com/v3/)
- [JSON API 规范](https://github.com/lmtdit/jsonapi)
- [来自HeroKu的HTTP API 设计指南](http://get.ftqq.com/343.get)
- [RESTful API 设计参考文献列表](https://github.com/aisuhua/restful-api-design-references)
- [RESTful最佳实践](http://arccode.net/2015/02/26/RESTful%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5/)
- [URL的设计](http://article.yeeyan.org/view/213582/200363/)
- [json-server](https://github.com/typicode/json-server)

## 12. 更多资料

* [What's the difference between ISO 8601 and RFC 3339 Date Formats?](http://stackoverflow.com/questions/522251/whats-the-difference-between-iso-8601-and-rfc-3339-date-formats)
* [JSON风格指南 - Google 风格指南（中文版）](https://github.com/darcyliu/google-styleguide/blob/master/JSONStyleGuide.md#%E5%B1%9E%E6%80%A7%E5%80%BC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
* [RFC 7231 中对请求方法的定义](http://tools.ietf.org/html/rfc7231#section-4.3)
* [RFC 5789](http://tools.ietf.org/html/rfc5789) - PATCH 方法的定义
* [维基百科](http://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE#.E8.AF.B7.E6.B1.82.E6.96.B9.E6.B3.95)
* [四种常见的 POST 提交数据方式](https://imququ.com/post/four-ways-to-post-data-in-http.html)
* [RFC 里的状态码列表](http://tools.ietf.org/html/rfc7231#page-49)
* [RFC 4918](http://tools.ietf.org/html/rfc4918) - 422 状态码的定义
* [RFC 6585](http://tools.ietf.org/html/rfc6585) - 新增的四个 HTTP 状态码，[中文版](http://www.oschina.net/news/28660/new-http-status-codes)
* [维基百科上的《 HTTP 状态码》词条](http://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81)
* [Do I need to use http redirect code 302 or 307? - Stack Overflow](http://stackoverflow.com/questions/2467664/do-i-need-to-use-http-redirect-code-302-or-307)
* [400 vs 422 response to POST of data](http://stackoverflow.com/questions/16133923/400-vs-422-response-to-post-of-data)
* [RFC 7232](http://tools.ietf.org/html/rfc7232)
* [HTTP 缓存 - Google Developers](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn)
* [RFC 2616 中缓存过期时间的算法](http://tools.ietf.org/html/rfc2616#section-13.2.3), [MDN 版](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching_FAQ), [中文版](http://blog.csdn.net/woxueliuyun/article/details/41077671)
* [HTTP 协议中 Vary 的一些研究](https://www.imququ.com/post/vary-header-in-http.html)
* [Cache Control 與 ETag](https://blog.othree.net/log/2012/12/22/cache-control-and-etag/)
* [Httpbis Status Pages](https://tools.ietf.org/wg/httpbis/)
* [所有在 IANA 注册的消息头和相关标准的列表](http://www.iana.org/assignments/message-headers/message-headers.xhtml)
- **author: huhb**
- **date: 20160405**

