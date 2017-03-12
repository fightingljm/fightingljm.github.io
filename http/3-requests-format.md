---
title: HTTP 请求格式
---

上一节了解了一个请求和响应的流程。这里就来揭开 HTTP 请求格式的秘密 ...

任何一个 HTTP 请求都可能是由这三部分组成:

- 请求行 request line --- 肯定有
- 请求头 Headers --- 肯定有
- 负载数据 payload --- 可能有(post 有,一般 get 是没有的)

### 请求行

第一行的内容被叫做请求行 `Request Line` ，具体形式如下

```
GET/POST [url] HTTP/[version]   GET / HTTP/1.1
```

这一行就是以HTTP 方法（ HTTP Method ）打头，一般是 GET 或者 POST .
- 当我们用 GET 发请求的时候，一般我们就是想要从服务器上 GET （拿到）一些内容，而不是想去修改服务器数据。
- POST 正好就是用来修改服务器上的数据的。
- 那么到底是 GET 还是修改资源，就要看后面 URL 这一项是怎么指定了。
上面例子中，请求的 URL 是 `/` 。最后就是跟 `HTTP` 字样，再跟上到底是使用的哪个版本的 HTTP 协议，目前一般都是 HTTP 1.1 。

### 请求的头部 Headers

`请求行`下面的内容就是`请求的头部`了。英文叫做 Headers ，注意是复数。也就是这一项下面可以有多个 header 。

```
[header 名]：[header 值]
```

For example :献上之前的三个 header

```
> Host: haoqicat.com
> User-Agent: curl/7.43.0
> Accept: */*
```

他们都是以冒号隔开的键值对

> 解释一波:
第一个，Host 代表被请求的主机，也就是 haoqicat.com
第二个，User-Agent 代表用户使用的客户端，我们这里用的是 curl
第三个，Accept 后面指明客户端可以接受的返回资源的类型，* 代表所有类型都接受

其他的 header 还有很多，可以参考 [我的博客之HTTP头部详解](http://www.cnblogs.com/ljm-fight/p/6539373.html)

### 负载数据 payload

header 之下，一个 request 中还可能包含负载数据（ payload ）。这一项，请求中不一定会包含，就像咱们上 一节的 request 中就没有这一项。GET 请求都是不带负载数据的，POST 请求带负载数据。这个挺好理解，POST 方法的请求 都是要改动服务器数据的，当然要在请求中携带数据过去。

比如，我们页面上有一个表单 form ，我们填写几项数据，然后一点提交，这个就会发出一个 POST 请求，而我们填写的数据， 就会作为 payload 成为请求的一部分。
