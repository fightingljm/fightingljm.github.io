---
title: GET 链接中嵌入参数传递到服务器
---

浏览器一般发出两类请求: GET ,POST. GET 用来获取数据,POST 用来修改服务器数据。 GET 请求主体数据（ payload ）通常为空，但是依然可以用一定的技巧，通过 GET 请求往服务器传递数据，例如，之前我们介绍的`查询字符串`的形式。但是那不是唯一形式，这一节我们看看如何在`链接中嵌入参数`传递给服务器。

- [peter](http://tiger.haoduoshipin.com/)搭建的公网

**API 文档**

```
  GET /users/1  {username: 'peter'}
  GET /users/2  {username: 'billie'}

  POST Content-Type: application/json  username=peter  # 使用 axios/react 来实现
       服务端会返回 { "msg": "你好 peter ，你已经成功登陆了"}
```

### 案例：传递用户 id 到服务器

现在我想请求一个具体的用户，通常会发这样的请求

```
GET /users/12345
```

我们要解决的问题是：如何把 12345 传递给服务器。

*先给出后台代码*

index.js

```js
const express =  require('express');
const app = express();
const cors = require('cors');
app.use(cors());

app.get('/users/:id', function(req, res){
  console.log(req.params.id)
})

app.listen(3000, function(){
  console.log('running on port 3000...');
});
```

后台运行

```
$ nodemon index.js
```
启动服务器。

*前台发出请求*

使用 curl 来发出请求

```
$ curl -X GET localhost:3000/users/12345
```

这样，到后台终端 log 中，可以看到 12345 被打印出来了，表示后台以及收到了前台的参数。

GET 请求因为不涉及到 HTTP 请求主体（ body ）部分，所以传参数都是通过 url 中嵌入参数的方式实现的。

### 跨域请求的问题

这时候，我们用 react 客户端实现一个请求，请求一下 [peter](http://tiger.haoduoshipin.com/) 搭建在公网上的 API 服务器。但是，此时不管请求任何 API ，后台都会触发跨域请求的保护机制。于是请求失败，浏览器 console 中的报错信息是：

XMLHttpRequest cannot load http://tiger.haoduoshipin.com/users/1. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.
解决方法

这个问题出现，我们作为前端开发者是无能为力的，只能同时后端开发者，修改服务器配置或者代码。

最终保证，我们在前台用 curl 测试

curl -I url.com/api
注： -I 的作用是显示请求头部。

要保证 access-control-allow-origin 这一项要存在，要么设成 * ，要么就是客户端实际域名。之后，前端开发可以继续进行。
