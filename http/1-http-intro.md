---
title: HTTP 简介
---

Although I am a full stack developer ...

跟大多数人一样,每次在浏览器顶部输入那些 URL 网址, HTTP 冒号 双斜杠 三大不留 ,接着是一串所谓域名的东西 Oh my God , what does this all mean ?

### 什么是 HTTP ？

HTTP ，是 `超文本传输协议` --- Hypertext Transfer Protocol 的缩写。

每天，网上都有大量的图片，视频，HTML 页面等等这些东西飞来飞去，从服务器上送到我们的浏览器里，这个过程都是实用 HTTP 协议来传输数据的。所以理解 HTTP ，不管是对于做网站，还是做 Web App ，都是开发者非常重要的基本功。

### Web 客户端和服务器端

Web 内容都是存储在 Web 服务器上的，Web 服务器都是实用 HTTP 协议的，因此也被称为 HTTP 服务器。HTTP 服务器存储了各种类型的数据，如果 HTTP 的客户端发出请求的话，服务器就会返回数据给客户端，叫做响应。请求（ request )和响应（ response ）都是重要的术语，后面我们会反复用到，这里要脑子里先划一道印。

HTTP 的服务器和客户端是万维网（ World Wide Web ）的基本单元。其实我们每天都在使用 HTTP 的客户端，最常见的客户端就是浏览器。浏览一个页面的时候，浏览器会向服务器发出一个 HTTP 请求。等到服务器响应返回之后，浏览器再去处理响应数据，以美观的形式展示给用户。
