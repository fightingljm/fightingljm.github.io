---
title: 上手 Express 框架
---

上一部分讲到的 React 是一个前端框架 ，前端框架是运行在浏览器环境下的，负责 UI（ User Interface 用户界面）

但是,如果只有 UI ，那么用户要看的数据从哪里来？用户需要保存的数据 如何进行运算之后保存到数据库中？这部分的功能就需要后端代码来完成

这就需要介绍 Express 一个后端（ back-end ）框架

这里普及一下 `框架` `库` `工具` 的概念

- `工具` ：就是完成特定的一个小功能的软件，比如 Babel

- `库` ： 英文叫 lib ，我们每天 import 的东西，都是库。库是把一系列相关工具，组织到一起。
  例如,lodash ,react 。库里面的东西虽然多，但是都是干一类工作的

- `框架` ：英文是 framework ，是把很多类功能的工具和库集合到一起，目的是完成整个项目。
  例如 , RubyOnRails, Express, React (这里指的是 React + friends，纯粹的 React 官方的说法就是一个 lib )

当下实现后台服务，最流行的方式就是使用 Nodejs , Express 就是 Nodejs 的 一个框架，而且是 Nodejs 各种后台框架中最为通用，最为流行的一个，没有之一。所以学习 Nodejs 最佳途径就是从 Express 入手

- [Express 官网](http://www.expressjs.com.cn/)

- `API` （ Application Program Interface ）是 **应用开发接口** ，简称接口 。而 Express 就是用来制作后台接口的，或者说叫制作后台 API 的。

那么之后，我们整个项目的架构，就是用 Express 来制作后台 API , 这些 API 的使用者就是前台 React 代码。

看过 Express 官网我们可以知道第一步需要装包

```
$ mkdir express-react-demo
$ cd express-react-demo

$ mkdir express-backend
$ mkdir react-frontend

$ cd express-backend
$ atom .
$ npm init -y
$ npm install express --save
```

普及一个常见装包错误

```
$ mkdir express
$ cd express
$ npm init -y
$ npm install --save express
```

以上则会抛出以下错误(拒绝把一个叫做 express 的包安装到它同名的包之下)

```
Refusing to install package with name "express" under a package also called "express".
```

我们的前台代码，因为有 Babel 的支持，可以全部采用 ES6 来写。后台代码，我们会让它直接运行在 Nodejs 之上，不用 Babel.

我们到 [这个](Node.green) 网站上，可以看到新版的 Nodejs (7.0 版本以上)对于 ES6 的支持已经到了99% 。所以，不用 Babel 我们也可以直接使用 ES6 语法，但是唯一要注意的就是不能用 import （ 也就是说 nodejs 是不支持 ES6 模块语法的），我们的后台代码暂时需要用 require 来替代 import 。require 用的是 commonjs 模块语法， 这个是 Nodejs 原生支持的。

说了这么多,现在该主角出场了,创建我们的 index.js

```js
const express = require('express');
const app = express();

//以下四行是一个完整的 API
app.get('./title',function (req,res) {
  console.log('my title');//命令行执行
  res.send('my title')//浏览器可以看到
})

app.get('./content',function (req,res) {
  console.log('my content');
  res.send('my content')
})

app.listen(3000,function () {
  console.log('running on port 3000...');
})
```

之后就该是运行了

```
$ node index.js
```

由于我们需要监听,这里只需安装一个 [nodemon](https://github.com/remy/nodemon) 的包就可以了

```
$ npm install -g nodemon
$ nodemon index.js
```
