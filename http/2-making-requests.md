---
title: 发起 HTTP 请求
---

这里来唠唠发起 HTTP 请求之后，表面那些事（用浏览器发起请求），和底层那些事（用 curl 工具发起请求）。这些我们观察到的现象就会是后续我们进行深入的 HTTP 知识讨论的起点。

### 资源 和 URL

HTTP 故事的开始是浏览器发出请求。这里的请求是服务器上的资源，英文叫 Resource 。

对应的每一个资源，都有一个 URL ，也就是统一资源定位地址，指向这个资源。不过资源分两种：

- 一种是静态资源，也就是各种文件了，最常见的就是静态 HTML ，但是也可以是 PDF ，json 文件等等。
- 另外一种，就是动态 资源，也就是 URL 指向的地方不是一个文件，而是一段代码的入口，服务器经过运算后，才返回运算结果给客户端。

所以， 我们有 `http://haoqicat.com/peter.txt` ，这个 URL 就是指向一个静态资源的。如果是 `http://haoqicat.com/username` 这个可能就是指向动态资源的，后台对应的可能就是一个 API 。

### 浏览器的 HTTP 请求

发起一个 HTTP 请求很容易。比如你说你想用浏览器访问 haoqicat.com 。你所需要做的仅仅是启动浏览器然后在地址栏输入 http://haoqicat.com ，然后你就可以在页面中看到好奇猫网站的首页了。

那么底层发生了什么呢？首先，我们的浏览器作为 **HTTP Client** ，发出了一个请求给服务器

```
GET http://haoqicat.com/
```

上面的 GET 是 HTTP 方法（或者叫 HTTP 动词），这个后面我们会详细介绍的。

haoqicat.com 的服务器收到请求后，给出响应。响应中带有各种数据，html/css/图片 等等，返回到浏览器。浏览器会解析这些文件格式，所以可以最终展示一个美观的网页给用户。

### 使用 curl 发起请求

因为浏览器给我们展示的是处理过的服务器响应，我们看不到服务器发给我们的响应的本来面目。怎么样才能看到原始的 HTTP 响应数据呢？

为此，我们可以使用一个 HTTP 工具，就像用浏览器的时候我们输入网址一样，我们可以用 HTTP 工具发送一个请求到 haoqicat.com。我们的 HTTP 工具，curl ，不会处理响应数据，这样能让我们看到原始的请求和响应数据，大概长这样

```
$ curl -X GET "http://haoqicat.com" -v
* Rebuilt URL to: http://haoqicat.com/
*   Trying 182.92.203.18...
* Connected to haoqicat.com (182.92.203.18) port 80 (#0)
> GET / HTTP/1.1
> Host: haoqicat.com
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Server: nginx/1.4.6 (Ubuntu)
< Date: Fri, 09 Dec 2016 09:23:59 GMT
< Content-Type: text/html; charset=utf-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Vary: Accept-Encoding
<
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
* Connection #0 to host haoqicat.com left intact
```

> 解释来一坨: `$ curl -X GET "http://haoqicat.com" -v`
使用 curl 工具，向 haoqicat.com 发起了一个 HTTP 请求。请求方法为 GET （关于 HTTP 方法之后详细的介绍）。如果看一下 `man curl` 也就是查看 curl 的手册，可以看到 `-X` 选项后面是专门用来指定 HTTP 方法的。最后的 `-v` 用来显示详情。所以我们才能看到比较详尽的后续输出内容。

再来看输出内容的前三行

```
* Rebuilt URL to: http://haoqicat.com/
*   Trying 182.92.203.18...
* Connected to haoqicat.com (182.92.203.18) port 80 (#0)
```

> 再来解释来一坨:
上面的输出表示请求正在被发出，首先找到了 haoqicat.com 对应的 IP 地址：`182.92.203.18` 。然后去连接 这个 IP 对应的服务器的 `80` 端口。端口 也是一个重要的术语，一个服务器好比一座大厦，可以有多个大门， 一个端口就是一道门。`80` 端口是 HTTP 协议默认走的端口。

### 请求头部

瞅一瞅右尖括号打头的这几行

```
> GET / HTTP/1.1
> Host: haoqicat.com
> User-Agent: curl/7.43.0
> Accept: */*
```

这个便是那个神秘的请求的头部（ header ）。关于请求的组成和结构 ...之后在唠

### 响应

再往下瞅瞅左尖括号打头的这几行是，响应的头部：

```
< HTTP/1.1 200 OK
< Server: nginx/1.4.6 (Ubuntu)
< Date: Fri, 09 Dec 2016 09:23:59 GMT
< Content-Type: text/html; charset=utf-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Vary: Accept-Encoding
```

这个里面藏着的秘密也是放到以后再说 ...

最后的这些都是响应的主体内容：

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

这里就没有什么秘密辣


上面我们大致了解了一个 HTTP 请求发出之后,底层都进行了那些 HTTP 层面的数据传递. 重点我们其实是看到两个东西 ：**请求** 和 **响应** ,同时发现他们两个其实还都是有一定的固定格式的. 之后我们就来解密这个 HTTP 基础知识的重点部分。
