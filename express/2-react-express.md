---
title: React Express
---

仅仅有了 API ，功能还不完全。需要有前端来调用 API ，完成整个小功能。

> 注意:前端和后端是完全独立的两个项目，通过 API 来作为桥梁

所以

```
$ cd Desktop
$ mkdir express-react-demo
$ cd express-react-demo

$ mkdir express-backend
$ mkdir react-frontend
```

### 前端项目准备

先定个小目标

- 页面最终显示出来自己的用户名，例如 fightingljm
- 要用到 react 的 state ，constructor ，生命周期
- 用 ES6/Babel/Webpack

> 啦啦啦:创建 React 应用的脚手架项目可以推荐

[点我](https://github.com/facebookincubator/create-react-app)看脚手架

不过这里我们自己走一遍

```
$ cd react-frontend
```

src/index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';


class App extends Component {
  constructor(){
    super();
    this.state = {
      username: ''
    };
  }
  componentWillMount() {
    this.setState({username: 'fightingljm'});
  }
  //用生命周期函数 componentWillMount 对 username 进行了覆盖,此乃妙计
  //后面结合 axios 向后台请求数据的代码，就会比较容易看出作用了
  render(){
    return(
      <div>
        {this.state.username}
      </div>
    )
  }
}

ReactDOM.render(<App/>,document.getElementById('app'));
```

package.json

```json
{
  "name": "react-frontend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "./node_modules/.bin/webpack && http-server ."
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "babel-core": "^6.23.1",
    "babel-loader": "^6.4.0",
    "babel-preset-env": "^1.2.1",
    "babel-preset-react": "^6.23.0",
    "react": "^15.4.2",
    "react-dom": "^15.4.2",
    "webpack": "^2.2.1"
  }
}
```

webpack.config.js

```js
var path = require('path');

module.exports = {
  entry: path.resolve(__dirname, 'src/index.js'),
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use: 'babel-loader'
      }
    ]
  }
};
```

.babelrc

```
{
  "presets": ["env", "react"]
}
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div id="app"></div>
  <script src="./build/bundle.js" charset="utf-8"></script>
</body>
</html>
```

目标完成 `^_^` 。下一步:

### 安装 http-server

前端代码最好也跑在一个 http 服务器之上,最简单的方法是:

```
$ npm install -g http-server
```

这样我们就拥有一个简单的系统工具叫 [http-server](https://github.com/indexzero/http-server)

```
$ cd react-frontend
$ http-server .
```

> 注意:要生成 /build/bundel.js 才可以执行代码,这里我把两条命令绑到了一块儿,如下所示

```json
"scripts": {
  "build": "./node_modules/.bin/webpack && http-server ."
}
```

这里我们只需要执行

```
$ npm run build
```

下面来看如何发请求到服务器端

开发一个功能好的做法是, 先修改后端代码,也就是实现 API => 然后用 curl 这样的命令来测试以下 API ,发现妥了再进行下一步 => write yourself front-end code

来来来,一起见证奇迹

### 实现 API

```
$ cd express-backend
$ atom .
```

index.js

```js
const express = require('express');
const app = express();
app.get('/username',function (req,res) {
  res.send({username:'fightingljm'})
})
app.listen(3000,function () {
  console.log('running on port 3000...');
})
```

### 用 curl 来测试 API

curl 是一个安装在系统上的命令，可以用来发 http 请求，最适合用来测试 API

当然测试之前要保证后端代码是跑起来的,否则会报以下错误

```
Failed to connect to localhost port 3000: Connection refused
```
即 连接 localhost:3000 失败,连接被拒绝 . 错误原因是 : 后端没有启动

所以我们要先执行 `npm run build` ,这样才能保证在任何位置都可以请求到

```
$ curl -X GET 'http://localhost:3000/username'
```

如果后端没有问题，应该可以看到下面的输出

```
{username: 'fightingljm'}
```

### 安装 axios ,发送 http 请求

现在我们来到前端代码中引入 axios . 按照 [axios 官网](https://github.com/mzabriskie/axios)的说法，它是一个 http client （ http 的客户端），换句话说，它是专门用来发 http 请求的。

axios 是常用的发 http 请求的工具（现在一般不提发 ajax 请求这个说法了）。

首先来进行装包：

```
$ npm install --save axios
```

把 axios 安装到 react-frontend 这个项目中。

装包之后，就可以到 src/index.js 中去使用了，代码如下

```js
import axios from 'axios';
```

我们当前的请求不希望是通过按钮来触发，而是希望，页面加载的时候，自动发出 http 请求，向服务要数据，所以，代码非常适合写到生命周期函数中：

```js
componentWillMount() {
  axios.get('http://localhost:3000/username').then(function(response){
      return console.log(response);
    }
  )
}
```

问题随之而来,当我们在浏览器中打开时,会发现如下错误

```
XMLHttpRequest cannot load http://localhost:3000/. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.
```

XMLHttpRequest 是发 HTTP 请求的底层机制，是浏览器自带功能。上面的意思是

>无法加载后台 http://localhost:3000 . 被请求的资源中没有设置 Access-Control-Allow-Origin 头部。源头设置为 Null ，所以不允许 访问。

`Access-Control-Allow-Origin` 字面意思：允许来源访问控制。服务器上的默认是不允许其他网址（或者网址相同，但是端口号不同）的网站请求资源的，如果需要开通权限，就需要设置这个选项。

那么，如何开通服务器上的这个资源访问权限呢？就是要在 **服务器** 上做下面的设置

```
Access-Control-Allow-Origin: *
```

有了这个设置，所有的第三方网站都可以访问服务器上的资源了。

### 跨域请求的解决方案

解决方案采用： https://github.com/expressjs/cors

cors (Cross Origin Resource Share --跨域请求资源分享) ，安装了这个包就可以完成 `Access-Control-Allow-Origin: *`这个设置了。

首先我们来瞅瞅在没有装包之前, Access-Control-Allow-Origin 的设置

```
$ curl -I http://localhost:3000/
```

curl 的 -I 选项用来专门拿到服务器返回的 header 。命令返回的信息，就是服务器端被请求资源的的 header 。

```
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 11
ETag: W/"b-sQqNsWTgdUEFt6mb5y4/5Q"
Date: Thu, 08 Dec 2016 01:51:44 GMT
Connection: keep-alive
```

上面是返回的结果,很明显是没有 Access-Control-Allow-Origin 这一项的。即为 null ,所以不能实现跨域资源共享.

下面我们就来安装 cors 包解决这个问题,[点我](https://www.npmjs.com/package/cors) 可以看到装包命令如下：

```
$ npm install --save cors
```

>注意：这个包要安装到后台代码中。

然后按照文档，在后台代码 index.js 添加下面两行代码：



```js
const express = require('express');
const app = express();

+ const cors = require('cors')
+ app.use(cors());

app.get('/username',function (req,res) {
  res.send({username:'fightingljm'})
})
app.listen(3000,function () {
  console.log('running on port 3000...');
})
```

保证代码是运行状态,再用 `curl -I http://localhost:3000/username` 看看输出：

```
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: text/html; charset=utf-8
Content-Length: 11
ETag: W/"b-sQqNsWTgdUEFt6mb5y4/5Q"
Date: Thu, 08 Dec 2016 02:17:23 GMT
Connection: keep-alive
```

快看,是 `Access-Control-Allow-Origin: *` 。

浏览器中，刷新一下，可以看到后台返回的 response 数据了。错误没有了。同时我们发现在后台输出的 object 下的 data 下的 username 正是存储了我们的用户名.

那下一步就让我们在浏览器把他整出来...

### 调整接口数据格式

修改前台代码，调整 componentWillMount ，如下：

```
componentWillMount() {
  axios.get('http://localhost:3000/username').then(function(response){
      return console.log(response.data.username);
  })
}
```

这样，前台浏览器的 console 中，就可以看到返回的数据 `fightingljm`

进一步靠近我们的目标,在浏览器显示,下面进一步调整 componentWillMount 如下：

```
componentWillMount() {
  axios.get('http://localhost:3000/username').then((response) => {
      this.setState({username: response.data.username});
  })
}
```

>注意:以上一定要写成箭头函数,否则 this 找不到

至此，前台页面上成功显示出了，后台的数据。这样，一个前后端分离架构，通过 API 通信的应用，我们就完成了。
