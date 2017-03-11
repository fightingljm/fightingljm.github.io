---
title: 打通全栈
---

所谓全栈,有下面几个技术层:

- 最贴近用户的 --- React
- HTTP 请求 --- axios
- 后台 --- express
- 最底层，海量数据 --- Mongodb

在[这里](https://fightingljm.github.io/express/2-react-express.html)我们打通了三层技术

[还有这里](https://fightingljm.github.io/express/4-mongoose.html)我们get了 MongoDB

啦啦啦,这里我们就来综合使用以上两部分知识来`打通全栈`,有了一个小目标,下面就分几个具体任务来完成

### 使用 Mongoose 来查询所有用户

首先，保证 mongodb 处于运行状态，然后，通过　mongo-express 查看一下， mongodb 中是否有多个用户。

[Mongoose 的 API 文档](http://mongoosejs.com/docs/api.html)超级好,只是还不适合我这个菜鸟,不过没关系,还有[这个](http://haoqicat.com/react-express-api/5-rest-api)

到后台代码的 index.js 文件中，把 db.once 部分的修改成下面这样：

```js
db.once('open', function() {
  User.find().exec(function(err, users) {
    console.log(users);
  });
});
```

这样，我们到　express-backend 文件夹中，命令行运行，可以看到如下输出结果

```
$ nodemon index.js
running on port 3000...
[ { _id: 584b62b830a2a2cbf4c4c3f6,
    username: 'ljm',
    email: 'ljm@ljm.com' },
  { _id: 584bb045ff8f0f1c7ba4fe24,
    username: 'fightingljm',
    email: 'fightingljm@fightingljm.com',
    __v: 0 } ]
```

可以看到，终端中可以打印出所有数据文档。证明我们的　mongoose 的　find() 接口 使用正确。

> 注意:这里 find() 接口是一个`异步函数`，所以它的返回值　`users只能在回调函数` 中使用。`.exec` 字面意思就是`执行`，我们把回调函数传给它做参数。所以不能写成这样---"let users = User.find();conole.log(users)"

About 同步和异步,It's a very big subject !

### 用 API 来返回　JSON

上面数据虽然拿到，但是如果想提供给客户端使用还需要做到：

- 1 把它要封装成一个　API
- 2 数据格式转换为　JSON

先来做第一步，代码做出如下调整：

```js
db.once('open', function() {
  console.log('success');
});

app.get('/users', function(req, res){
  User.find().exec(function(err, users) {
    console.log(users);
  });
})
```

上面把　User.find() 代码封装到了一个　API ( Web API ) 。这样， 触发条件就变了。只有当客户端发出　`GET /users` 请求时，User.find() 代码才会被执行。

之后要用　`curl` 来模拟一下客户端请求,来测试以下能不能收到反馈信息

```
$ curl -X GET http://localhost:3000/users
```

执行命令发现 curl 请求不到任何返回信息，因为　`console.log(users)` 只会把信息打印到后台终端。curl 请求不到信息，未来浏览器也就请求不到。所以要把这一行 改为

```js
// res.send() 可以数据返回给客户端，但是我们要的是　json ，所以用下面接口
res.json()
```

即...

```js
app.get('/users', function(req, res){
  User.find().exec(function(err, users) {
    res.json({users});
  });
})
```

改完之后再用　curl 请求一下，前台就能读到下面的数据

```
$ curl -X GET http://localhost:3000/users
{"users":[{"_id":"584b62b830a2a2cbf4c4c3f6","username":"billie66","email":"billie@billie66.com"},{"_id":"584b760498d7b520b68a05cd","username":"pppaaa"},{"_id":"584bb045ff8f0f1c7ba4fe24","username":"inCode","email":"inCode@incode.com","__v":0}]}
```

这样，后台代码就大功告成辣。

**来来来,瞅瞅完整的后台代码**

express-backend 文件夹中，有下面的文件：

index.js

```js
  const express = require('express');
  const app = express();

  //解决跨域请求
  const cors = require('cors')
  app.use(cors());

  //使用 Mongoose 连接 JS 和 MongoDB
  const mongoose = require('mongoose');
  mongoose.connect('mongodb://localhost:27017/express-react-demo');
  // 执行此行代码之前，要保证 mongodb 数据库已经运行了，而且运行在 27017 端口
  const User = require('./models/user')

  let db = mongoose.connection;
  db.on('error', console.log);
  db.once('open', function() {
    console.log('success!')
  });

  // 下面三行就是我们实现的一个 API //GET /users
  app.get('/users',function (req,res) {
    // res.send({username:'fightingljm'});
    User.find().exec(function(err, users) {
      // console.log(users);
      res.json({users});
    })
  })

  app.listen(3000,function () {
    console.log('running on port 3000...');
  })

```

models/user.js

```js
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

const UserSchema = new Schema(
  {
    username: { type: String },
    email: { type: String }
  }
);
module.exports = mongoose.model('User', UserSchema);
// `User` 会自动对应数据库中的　users 这个集合
// 如果这里是　Apple 那么就会对应　apples 集合
// 如果这里是　Person 那么就会对应　people 集合
```

package.json

```json
{
  "name": "express-backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "nodemon index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.1",
    "express": "^4.14.0",
    "mongoose": "^4.7.2"
  }
}
```

### 前台书写　axios 请求

到前台项目 `react-frontend` 中，修改生命周期函数如下：

```js
componentWillMount() {
  axios.get('http://localhost:3000/users')
    .then(response => console.log(response))
}
```

这样，就可以看出　response 中的数据结构了，我们想要的数据可以这样拿到

```js
constructor(){
  super();
  this.state = {
    users: []
  };
}
componentWillMount() {
  axios.get('http://localhost:3000/users')
    .then((response) => this.setState({users: response.data.users}))
}
```

在　render 函数中

```js
render(){
  return(
    <div>
      {this.state.users}
    </div>
  )
}
```

打开浏览器发现可恶的 error

```
 Uncaught (in promise) Error: Objects are not valid as a React child (found: object with keys {_id, username, email}). If you meant to render a collection of children, use an array instead or wrap the object using createFragment(object) from the React add-ons. Check the render method of `App`.(…)
```

报错信息的大致意思是：对象不是被允许的　React 子元素。

小 case ......使用 map 展开数组就好辣,let me try

render 函数做如下调整：

```js
render(){
  const userList = this.state.users.map((user, index) => {
    return (
      <div key={index}>
        username: {user.username}
      </div>
    )
  })
  return(
    <div>
      { userList }
    </div>
  )
}
```

看到了吧，页面中就可以显示出所有用户的用户名的列表了。

Give you 瞅瞅 my whole 前台代码

src/index.js

```js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import axios from 'axios';

class App extends Component {
  constructor(){
    super();
    this.state = {
      users: []
    };
  }
  componentWillMount() {
    axios.get('http://localhost:3000/users')
      .then((response) => {
      this.setState({users: response.data.users});
    })
  }
  render(){
    const userList = this.state.users.map((user, i) => {
      return (
        <div key={i}>
           username:
          {user}
        </div>
      )
    })
    return(
      <div>
        { userList }
      </div>
    )
  }
}

ReactDOM.render(<App/>,document.getElementById('app'));
```

webpack.config.js 如下：

```js
module.exports = {
  entry:'./src/index.js',
  output:{
    path:'build',
    filename:'bundel.js',
    publicPath:'build/'
  },
  devtool:'eval',//报错到源代码
  resolve: {
    extensions: [".js", ".jsx",".css",".jpg"],
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,//排除node_modules文件夹
        use: "babel-loader"
      },//js文件用babel加载
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      },//css文件的加载,解析文件样式
      {
        test: /\.(jpe?g|png)$/,
        use: 'file-loader'
      }//图片加载
    ]
  }
}
```

.babelrc 如下

```
{
  "presets": ["env", "react"]
}
```

index.html 如下：

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
  "repository": {
    "type": "git",
    "url": "git+https://github.com/fightingljm/1610-demo.git"
  },
  "keywords": [
    "react"
  ],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/fightingljm/1610-demo/issues"
  },
  "homepage": "https://github.com/fightingljm/1610-demo#readme",
  "devDependencies": {
    "babel-core": "^6.23.1",
    "babel-loader": "^6.3.2",
    "babel-preset-env": "^1.1.8",
    "babel-preset-react": "^6.23.0",
    "css-loader": "^0.26.1",
    "file-loader": "^0.10.0",
    "style-loader": "^0.13.1",
    "webpack": "^2.2.1",
    "webpack-dev-server": "^2.4.1"
  },
  "dependencies": {
    "axios": "^0.15.3",
    "lodash": "^4.17.4",
    "react": "^15.4.2",
    "react-addons-css-transition-group": "^15.4.2",
    "react-dom": "^15.4.2",
    "react-router": "^3.0.2"
  }
}
```
