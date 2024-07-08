
网络应用程序，分为前端和后端两个部分。当前的发展趋势，就是前端设备层出不穷（手机、平板、桌面电脑、其他专用设备......）。

因此，必须有一种统一的机制，方便不同的前端设备与后端进行通信。这导致 API 构架的流行，甚至出现 `API First` 的设计思想。[RESTful API](https://en.wikipedia.org/wiki/Representational_state_transfer)是目前比较成熟的一套互联网应用程序的 API 设计理论。


## 一、协议

API 与用户的通信协议，总是使用 `HTTPS` 协议。


## 二、域名

应该尽量将 API 部署在专用域名之下。

```
https://api.example.com
```

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。

```
https://example.org/api/
```


## 三、版本（Versioning）

应该将 API 的版本号放入 URL。

```
https://api.example.com/v1/
```

另一种做法是，将版本号放在 HTTP 头信息中，如放在 `x-api-version` 中， 但不如放入URL方便和直观。[Github](https://developer.github.com/v3/media/#request-specific-version)采用这种做法。


## 四、路径（Endpoint）

路径又称"终点"（endpoint），表示 API 的具体网址。

在 RESTful 架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词（可复数），而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。

举例来说，有一个 API 提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

```
https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/employees
```


## 五、HTTP 动词

对于资源的具体操作类型，由 HTTP 动词表示。常用的 HTTP 动词有下面前五个（括号里是对应的SQL命令），后面两个 HTTP 动词不常用。

*   GET（SELECT）：从服务器取出资源（一项或多项）。
    + 完成请求后返回状态码 `200 OK`
    + 完成请求后需要返回被请求的资源详细信息

*   POST（CREATE）：在服务器新建一个资源。
    + 创建完成后返回状态码 `201 Created`
    + 完成请求后需要返回被创建的资源详细信息

*   PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）或者创建指定身份的资源，比如创建 id 为 123 的某个资源。
    + 如果是创建了资源，则返回 `201 Created`
    + 如果是替换了资源，则返回 `200 OK`
    + 完成请求后需要返回被修改的资源详细信息。

*   PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
    + 完成请求后返回状态码 `200 OK`
    + 完成请求后需要返回被修改的资源详细信息

*   DELETE（DELETE）：从服务器删除资源。
    + 完成请求后返回状态码 `204 No Content`

*   HEAD：获取资源的元数据。

*   OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

下面是一些例子。

*   GET /zoos：列出所有动物园
*   POST /zoos：新建一个动物园
*   GET /zoos/ID：获取某个指定动物园的信息
*   PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
*   PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
*   DELETE /zoos/ID：删除某个动物园
*   GET /zoos/ID/animals：列出某个指定动物园的所有动物
*   DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物


## 六、过滤信息（Filtering）

如果记录数量很多，服务器不可能都将它们返回给用户。API 应该提供参数，过滤返回结果。

下面是一些常见的参数。

*   `?limit=10`：指定返回记录的数量
*   `?offset=10`：指定返回记录的开始位置。

*   `?current_page=2&page_size=100`：指定第几页，以及每页的记录数。
*   `?sort_by=name&order=asc`：指定返回结果按照哪个属性排序，以及排序顺序。
*   `?animal_type_id=1`：指定筛选条件

参数的设计允许存在冗余，即允许 API 路径和 URL 参数偶尔有重复。比如 GET /zoo/ID/animals 与 GET /animals?zoo\_id=ID 的含义是相同的。


## 七、状态码（Status Codes）

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

- 2xx: 请求成功

    *   200 OK - \[GET\]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
    *   201 CREATED - \[POST/PUT/PATCH\]：用户新建或修改数据成功。
    *   202 Accepted - \[\*\]：接受请求，但无法立即完成创建行为，比如其中涉及到一个需要花费若干小时才能完成的任务。返回的实体中应该包含当前状态的信息，以及指向处理状态监视器或状态预测的指针，以便客户端能够获取最新状态。
    *   204 NO CONTENT - \[DELETE\]：请求执行成功，不返回相应资源数据，如 `PATCH` ， `DELETE` 成功。

-  3xx: 重定向，重定向的新地址都需要在响应头 `Location` 中返回

    *   301 Moved Permanently : 被请求的资源已永久移动到新位置
    *   302 Found : 请求的资源现在临时从不同的 URI 响应请求
    *   303 See Other : 对应当前请求的响应可以在另一个 URI 上被找到，客户端应该使用 `GET` 方法进行请求
    *   307 Temporary Redirect : 对应当前请求的响应可以在另一个 URI 上被找到，客户端应该保持原有的请求方法进行请求

- 4xx：客户端错误
    
    *   400 INVALID REQUEST - \[POST/PUT/PATCH\]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
    *   401 Unauthorized - \[\*\]：需要验证用户身份，如果服务器就算是身份验证后也不允许客户访问资源，应该响应 `403 Forbidden`。
    *   403 Forbidden - \[\*\]: 服务器拒绝执行。
    *   404 NOT FOUND - \[\*\]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
    *   406 Not Acceptable - \[GET\]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）,但响应里会包含服务端能够给出的格式的数据，并在 `Content-Type` 中声明格式名称。
    *   410 Gone -\[GET\]： 被请求的资源已被删除，只有在确定了这种情况是永久性的时候才可以使用，否则建议使用 `404 Not Found`
    *   422 Unprocesable entity - \[POST/PUT/PATCH\] 当创建一个对象时，发生一个验证错误。

- 5xx: 服务器错误

    *   500 INTERNAL SERVER ERROR - \[\*\]：服务器发生错误，用户将无法判断发出的请求是否成功。

状态码的完全列表参见[这里](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)。


## 八、错误处理（Error handling）

如果状态码是 4xx 或 5xx，就应该向用户返回出错信息。一般来说，返回的信息中将 error 作为键名，也可以直接复用 message，出错信息作为键值即可。

```
{
    error: "Invalid API key"
    // message: "Invalid API key"
}
```

*   服务器维护中，`503` 状态码

    ```http
    HTTP/1.1 503 Service Unavailable

    {"code": "503", "message": "Service In the maintenance"}
    ```

*   发送了无法转化的请求体，`400` 状态码

    ```http
    HTTP/1.1 400 Bad Request

    {"code": "400","message": "Problems parsing JSON"}
    ```

*   服务到期（比如付费的增值服务等）， `403` 状态码

    ```http
    HTTP/1.1 403 Forbidden

    {"code": "403","message": "Service expired"}
    ```

*   因为某些原因不允许访问（比如被 ban ），`403` 状态码

    ```http
    HTTP/1.1 403 Forbidden

    {"code": "403", "message": "Account blocked"}
    ```

*   权限不够，`403` 状态码

    ```http
    HTTP/1.1 403 Forbidden

    {"code": "403", "message": "Permission denied"}
    ```

*   需要修改的资源不存在，`404` 状态码

    ```http
    HTTP/1.1 404 Not Found

    {"code": "404", "message": "Resource not found"}
    ```

*   缺少了必要的头信息，`428` 状态码

    ```http
    HTTP/1.1 428 Precondition Required

    {"code": "428", "message": "Header User-Agent is required"}
    ```

*   发送了非法的资源，`422` 状态码

    ```http
    HTTP/1.1 422 Unprocessable Entity

    {
      "code": "428",
      "message": "Validation Failed",
    }
    ```


## 九、返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范。

> *   GET /collection：返回资源对象的列表（数组）
> *   GET /collection/resource：返回单个资源对象
> *   POST /collection：返回新生成的资源对象
> *   PUT /collection/resource：返回完整的资源对象
> *   PATCH /collection/resource：返回完整的资源对象
> *   DELETE /collection/resource：返回一个空文档


## 十、Hypermedia API

RESTful API 最好做到 Hypermedia，即返回结果中提供链接，连向其他 API 方法，使得用户不查文档，也知道下一步应该做什么。

比如，当用户向 api.example.com 的根目录发出请求，会得到这样一个文档。

```json
{
  "link": {
    "rel":   "collection https://www.example.com/zoos",
    "href":  "https://api.example.com/zoos",
    "title": "List of zoos",
    "type":  "application/vnd.yourformat+json"
  }
}
```

上面代码表示，文档中有一个 link 属性，用户读取这个属性就知道下一步该调用什么 API 了。rel 表示这个 API 与当前网址的关系（collection 关系，并给出该 collection 的网址），href 表示 API 的路径，title 表示 API 的标题，type 表示返回类型。

Hypermedia API 的设计被称为[HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)。Github 的 API 就是这种设计，访问[api.github.com](https://api.github.com/)会得到一个所有可用API的网址列表。

```
{
  "current_user_url": "https://api.github.com/user",
  "authorizations_url": "https://api.github.com/authorizations",
  // ...
}
```

从上面可以看到，如果想获取当前用户的信息，应该去访问[api.github.com/user](https://api.github.com/user)，然后就得到了下面结果。

```
{
  "message": "Requires authentication",
  "documentation_url": "https://developer.github.com/v3"
}
```

上面代码表示，服务器给出了提示信息，以及文档的网址。


## 十一、其他

- API的身份认证应该使用[OAuth 2.0](https://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)框架。

- 服务器返回的数据格式，应该尽量使用 JSON，避免使用 XML。


## reference

- http://www.ruanyifeng.com/blog/2014/05/restful_api.html

