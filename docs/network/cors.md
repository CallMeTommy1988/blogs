# 关于cors

#### base

其实之前就有专门去看过跨域，不过当时使用的很急，简单的看了下就开始使用了，这次看**http**,就来个系统一点的。

首先什么是跨域。就是访问不属于你这个源的网页(在前端).
源的定义是什么？

> 协议 + 域名(ip) + 端口

如果这三个完全一样，那就是同源，你不需要跨域，只要有一个不一样，那就属于跨域

跨域这件事是属于浏览器范畴，并不属于**http**请求，**http**本身是不阻止这些信息。浏览器处于安全考虑会阻止你访问不属于一个源的资源，会直接阻止你发起请求或者你发起后再阻止。

但是难免会有一些情况需要跨域，比如你要提供`api`，所以需要在浏览器允许的规则下，实现跨域的操作.

---
### 大致方法
---

 - http cors(简单请求)
 - http cors(预检请求)
 - jsonp
 - document.domain + iframe
 - location.hash + iframe
 - window.name + iframe
 - postMessage
 - webSocket
 - 代理
 - nginx

接下来会一个一个测试

---
### http cors(简单请求)
---

简单请求的概念是区别于不简单的请求。
首先请求方式只能是这三种
>   GET
>   HEAD
>   POST
>     
>   Content-Type 的值仅限于下列三者之一：
>
>    text/plain
>    multipart/form-data
>    application/x-www-form-urlencoded
>
> 请求中的任意XMLHttpRequestUpload 对象均没有注册任何事件监听器；XMLHttpRequestUpload 对象可以使用XMLHttpRequest.upload 属性访问。
>
> 请求中没有使用 ReadableStream 对象。

这是最基本的条件.

其他的条件是请求方需要再**header**中加入 **origin**.
然而服务端也得允许
```
res.setHeader("Access-Control-Allow-Origin", "*");
```
很快可以简单测试一下.
服务端
```
app.get("/", function (req, res) {
    res.setHeader("Access-Control-Allow-Origin", "http://localhost:5003");

    //add header
    var id = req.query.id * 1;
    req.session.code = id;
    console.log(req.session.code);

    res.send("cors 01 success! " + req.session.code);
});

app.listen(5001, () => {
    console.log(`01 started service`);
});
```

客户端
```
function ajaxClick() {
    $.ajax({
        url: "http://localhost:5001?id=110",
        type: "GET",
        dataType: "text",
        xhrFields: {
            withCredentials: true
        },
        success: function (data, textStatus) {
            alert(data);
        }
    });
}
```

于是就可以了。如果不加会报错。
> Failed to load http://localhost:5001/?id=110: The 'Access-Control-Allow-Origin' header has a value 'http://localhost:5003' that is not equal to the supplied origin. Origin 'http://localhost:5002' is therefore not allowed access.

这就是最简单也最方便的跨域方式. 只要你满足条件就能跨域
如果你需要设置多个
```
req.headers.origin 做个判断就好了.
res.setHeader("Access-Control-Allow-Origin", "http://localhost:5003");
```
**关于跨域带cookie**
之前在做单点登录的时候，因为ajax跨域不能带 **cookie**. 所以很麻烦。
当我看到这个属性
```
withCredentials: true
```
于是我简单试了一下.

```
app.get("/", function (req, res) {
    res.setHeader("Access-Control-Allow-Origin", req.headers.origin);
    res.setHeader("Access-Control-Allow-Credentials", true);

    //add header
    var id = req.query.id * 1;
    req.session.code = id;
    console.log("/ :", req.session.code);

    res.send("cors 01 success! " + req.session.code);
});


app.get("/s", function (req, res) {
    res.setHeader("Access-Control-Allow-Origin", req.headers.origin);
    res.setHeader("Access-Control-Allow-Credentials", true);

    let code = req.session.code;
    console.log("s :", code);
    res.send("s :" + code);
});
```

加入了 `Access-Control-Allow-Credentials`
`ajax` 也加入相同的 **header**
结果是可以访问session. 简直不要太舒服.
不过不知道有没有安全性问题，目前正在查这方面的问题。
因为这样的话，很多单点登录的解决方案完全不需要使用认证那一套，但是我看其他网站还是使用认证通知那一套。 所以还是有所顾虑。
> IE10+ 才支持这个协议.

<br/>

### http cors(预检请求).

之前不太知道这种请求方式,但是见过。
之前再看请求的时候,发现总是发送了两个请求，一个 **options**,然后再是一个真实请求。
还以为有问题。

其实预检请求相比简单请求就是多了一个请求。
预先发送一个请求，来与服务器确认权限。确认后再继续发送真实请求。


> 使用了下面任一 HTTP 方法：
        1. PUT
        2. DELETE
        3. CONNECT
        4. OPTIONS
        5. TRACE
        6. PATCH
        
> 人为设置了对 CORS 安全的首部字段集合之外的其他首部字段。该集合为：
        1. Accept
        2. Accept-Language
        3. Content-Language
        4. Content-Type (but note the additional requirements below)
        5. DPR
        6. Downlink
        7. Save-Data
        8. Viewport-Width
        9. Width
        
 >   Content-Type 的值不属于下列之一:
        1. application/x-www-form-urlencoded
        2. multipart/form-data
        3. text/plain
        
> 请求中的XMLHttpRequestUpload 对象注册了任意多个事件监听器。

> 请求中使用了ReadableStream对象。

这就是协议，其实可以简单说不属于简单请求的就是预检请求. 只要你属于以上原则。

于是自己做了一个简单测试

**服务端**
```
app.use(function (req, res, next) {
    res.header("Access-Control-Allow-Origin", req.headers.origin);
    res.header("Access-Control-Allow-Methods", "OPTIONS");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept, Authorization, Access-Control-Allow-Credentials");
    res.header("Access-Control-Allow-Credentials", "true");
    if (req.method == "OPTIONS") {
        res.send(200);
    }
    else {
        next();
    }
});

app.get("/", function (req, res) {
    res.send("跨域成功");
});
```

**客户端**
```
$.ajax({
    url: "http://localhost:5003",
    type: "GET",
    dataType: "text",
    contentType: "text/plain",
    headers: {
        "X-PINGOTHER": "pingpong"
    },
    success: function (d) {
        alert(d);
    }
});
```

于是跨域成功.

**预检请求** 的好处是，能够自己进行验证。当我自定义**header**以后，就可以自定义去验证。也就可以不光检查来源，还可以做一些加密验证. 只是因为 `options` 会多一次请求.

**简单请求** 最简单，但是他不能自定义`header`,他只能使用来源来做检查。 如果需要非常高安全性的,检查还是比较少。

至于怎么使用就需要看具体情况了。如果只需要简单请求，就不要自定义`header`等。


### jsonp 跨域

原理无非是 `script` 标签可以直接跨域的。
```
<script src="http://localhost:5003/jsonp"></script>
```
也就是这样你输入一个地址。 返回一段`js`,直接调用约定好的方法.
所以这样写便好
**前端**
```
<script>
    function test(str) {
        alert(str);
    }

</script>
<script src="http://localhost:5003/jsonp"></script>
```
**服务端**
```
app.get("/jsonp", function (req, res) {
    res.type('text/javascript');
    res.send('test("这是来世5003的数据")');
});
```

这便是 `jsonp` 的原理.
但是 `jsonp` 的问题是。
1. 不支持POST,所以你传传简单东西没问题。你传个复杂`json` 真的很麻烦。
2. 如果访问出错，就是`fail` 你没法去捕获。出了什么问题？404？ 还是500？ 你无法捕获
3. 然后就是需要返回js. 这个再服务端也不方便。
4. 安全性，这点有点恐怖，如果你接口存在漏洞，因为最多就数据存在点问题。但是`jsonp`出现问题，他说不定可以改变`js`. 你的其他网站也炸了。 这就是问题。

但是他依然有他的应用场景。
1. 他支持老版本浏览器。
2. 一些服务，比如天气，比如股票。你本身需要自带样式，脚本。毕竟可以直接执行

还是可以用一波的，毕竟什么都不用设置。直接跨域。

### document.domain + iframe
### location.hash + iframe

这两种方案对于我来说实在是没有什么应用场景。暂时略过.

### postMessage
其实还是基于**iframe**,但是实用性大很多。
很简单一个页面发，一个页面收.
发的页面
```
<iframe src="http://localhost:9022/b.html" id="child"></iframe>
<script>
var data = "from page a message";
window.frames[0].postMessage(data, 'http://localhost:9022/');
</script>
```

收的页面
```
window.addEventListener('message', function(ev) {
    // if (ev.source !== window.parent) {return;}
    var data = ev.data;
    console.info('message from parent:', data);
}, false);
```

如果有使用iframe的场景，**postMessage**是一个很好的用法。只是安全性问题
因为可能会导致xxs攻击。

1. 谨慎使用**postMessage**
2. souce严格检查，别写*****
3. 严格检查**data**

### webscoket

这个只要服务器支持协议，就没问题。

### 代理，nginx

> 通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域登录。

其实这种没怎么用过。有机会学学nginx

### 引用

- [浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)
- [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
- [HTTP访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
- [window.postMessage
](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)






