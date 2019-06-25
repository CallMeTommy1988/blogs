# http 基本

## http 基本

1. 起因.
2. Request 协议
3. Response 协议
4. http的原理.
5. http的运行方式.
6. http header
7. 引用

#### 1.起因

起因就是因为想实现一下`ajax`无刷新上传。搜索了一下需要用到 `form-data`，于是又准备继续看一下相关 `form-data` & 原生`ajax`. 看`ajax`的时候又发现 **http** 不是很熟悉。于是开始了 **http** 的学习。有了这篇文章。

### 2.Request 协议

分为3块。 **报头，header,body**

##### 报头
-----

![报头](https://mdn.mozillademos.org/files/13687/HTTP_Request.png)

这张图很清楚的说明了请求的方式。

1. **Method** 可以是 GET,POST,DELETE等等. 请求的方式
2. **Path** 请求的路径.
3. **Version of the protocol** http版本。

##### header.
----

放在报头下面，用于描述和扩展这个请求。

![header](https://mdn.mozillademos.org/files/13821/HTTP_Request_Headers2.png)

> General headers，例如 Via，适用于整个报文。
> Request headers，例如 User-Agent，Accept-Type，通过进一步的定义(例如 Accept-Language)，或者给定上下文(例如 Referer)，或者进行有条件的限制 (例如 If-None) 来修改请求。
> Entity headers，例如 Content-Length，适用于请求的 body。显然，如果请求中没有任何 body，则不会发送这样的头文件。

每一个参数都对应有表达和对应个格式，需要查询的话看[这里](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)

##### body
---
用于传输内容。
比如无刷新上传的 `form-data` 需要在里面传入参数。
理论上你可以在`body`里传入参数，但是服务器是否解析就不知道了。

**POST**方式是最常用的需要在**body**里传入参数的

这样你发给服务器，就会有对应的返回. 返回值其实也类似。

### Response 协议

![Response](https://mdn.mozillademos.org/files/13823/HTTP_Response_Headers2.png)

这是一个返回。
其实和**Request**很类似

报头上返回 
> 协议 + 状态码 + 信息

其他就是

> header + body

状态码会告诉你这次请求的结果。 200 就是ok了。如果出现错误 message 会给你提示. 
对应所有的状态码的表在[这里](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)

### 4. http的原理.

其实一直都知道 http 是基于tcp。 只是也不知道过程，是否是直接发送协议就好了

在node中使用net尝试发送了一下 http协议。

```
let net = require("net");
var Host = "127.0.0.1";
var PORT = 3002;


var client = new net.Socket();
client.connect(PORT, Host, d => {
    var h = "\r\n";
    var o = "GET /en HTTP/1.1" + h
        + "Host: localhost:3002" + h
        + "Connection: keep-alive" + h + h;

    client.write(o);
    client.write(o);
    client.write(o,"UTF8", function(d) {
        console.log("cb",d);
    });
});

client.on('data', data => {
    console.log(data.toString());
});

client.on('error', data => {
    console.log("error:", data);
});

client.on('close', data => {
    console.log(data);
    console.log('close');
});
```

我自己用 `express` 搭建了一个简单的服务。
创建一个 tcp，直接发送请求。

然后服务器返回

```
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 14740
ETag: W/"3994-j7Nux319iwzdxfspPtPQXL1lOx4"
set-cookie: front.sid=s%3AtpA4_wmu9pRxdomGHj1tWMra-kkUmagL.9KHKKgDlm7U1vdXSSbQbkwVGqGk9hqMHmsTm0hF2Tc4; Path=/; Expires=Wed, 15 Aug 2018 11:58:39 GMT; HttpOnly
Date: Wed, 15 Aug 2018 10:18:39 GMT
Connection: keep-alive
```

这就是基础。


### 5. 运行方式.

上面的简单代码说明了 `http` 请求其实就是**tcp**发送协议文本罢了。
但是早期协议是短链接。 也就是说 
```
client.on('data', data => {
    console.log(data.toString());
    client.destroy();
});
```

你每一个请求都会新开一个链接，一次tcp，玩握手。 效率很低。
于是很明显的都想到了**长连接**
就如我上文中。在一个`tcp` 不断开，直接发送3次请求。 会返回三次。看似解决了这个问题。
但是呢，我换一个测试方式
```
var h = "\r\n";
var o = "GET /en HTTP/1.1" + h
    + "Host: localhost:3002" + h
    + "Connection: keep-alive" + h + h;

var o1 = "GET /en/hahaha HTTP/1.1" + h
    + "Host: localhost:3002" + h
    + "Connection: keep-alive" + h + h;

client.write(o);
client.write(o1);
```
这个时候,我先讲地址 `/en` 加断点。在断的时候，接下来的`o1` 并不能执行。 这也是一个问题。他不是并行的。 他需要一个一个执行.

接下来就是**HTTP 流水线**
这个是理论上的，因为我没有查到资料怎么自己去实现。 就是说解决了之前我说的在一个**tcp**并不是并行请求的问题。
流水线模式是可以并行的，在浏览器支持。

![i](https://mdn.mozillademos.org/files/13727/HTTP1_x_Connections.png)

这个就是图片上的解释，短连接，长连接，流水线。

### 6. http-header

**header** 字段比较多。需要的测试也比较多。

可以看这里 [HTTP Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)

有所有的字段描述，这里就不赘述了。


### 6. 引用

1. [header 解释](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)
2. [状态码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)
3. [HTTP的长连接和短连接](https://www.cnblogs.com/cswuyg/p/3653263.html)
4. [HTTP Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)

