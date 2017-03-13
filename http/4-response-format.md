---
title: HTTP 响应格式
---

这里就来揭开 HTTP 响应格式的秘密 ...

 HTTP 响应的基本格式:

- 状态行
- 响应头部
- 响应主体数据

### 状态行

对于请求有请求行，响应的第一行也很特别，叫做状态行 （ status line ） ，基本格式如下

```
HTTP[版本号] [状态码] [状态信息]
```

For example

```
HTTP/1.1 200 OK
```

`状态码` 是个什么东西...?

- `20x` 的状态码都代表某种成功状态。最常见的 200 ，它的意义，就正如它后面跟的 状态信息 一样，代表一切 OK 。
- `30x` 的状态码，意味着资源已经被移动到其他地方了，但是响应中给出了应该跳转到哪里去找到这个资源。这个行为的 术语就叫做 redirect （重定向）。
- `40x` 的代码也都是代表一种客户端请求错误 。一个最常见的状态码 404 ，它的意 义也跟它后面紧跟的状态信息所说的 一样：Page Not Found （页面未找到）。
- `50x` 的状态吗也很常见。返回的如果是这一系列的状态码，就意味着 服务器端在处理请求的时候出错 。50x 出现，对于开发者，一般意味着服务器端代码出了错误。

### 响应头部 Headers

跟请求一样，响应也有自己的 Headers 。它的基本格式是

```
[ header 名]: [ header 值]
```

For example

```
Server: nginx/1.4.6 (Ubuntu)
Date: Fri, 09 Dec 2016 09:23:59 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
```

这些信息基本上都是用来描述后面要介绍的响应主体数据的，例如， `Date` 响应数据返回的时间， `Content-Type` 响应数据的格式（ html ）...

### 响应主体

响应主体，response body ，也可以叫做 payload 。

For example

```html
<!DOCTYPE html>
<html>
<head>
  <title>haoqicat</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  ...
</head>
<body>
...

</body>
</html>
```
