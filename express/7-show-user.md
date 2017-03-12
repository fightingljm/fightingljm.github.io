---
title: 显示一个用户的信息
---

这里会用到下面几个知识点：

- MongoDB 数据库基本操作
- Mongoose 这个 JS 库的使用
- 前台向后台传递参数：涉及 axios 和 express 配合
- React 基本操作

*小目标* : 用户在浏览器中点按钮，发　axios 请求访问这个链接 --- http://localhost:3000/users/+[一个真实的用户 id]
页面上要能显示出用户名和邮箱。

### 后台增加请求一位用户信息的API

index.js

```js
const express =  require('express');
const app = express();

const cors = require('cors');
app.use(cors());

const mongoose = require('mongoose');
const User = require('./models/user');

mongoose.connect('mongodb://localhost:27017/digicity');
// 执行此行代码之前，要保证 mongodb 数据库已经运行了，而且运行在 27017 端口

var db = mongoose.connection;
db.on('error', console.log);
db.once('open', function() {
  console.log('success');
});

// 下面三行就是我们实现的一个 API
app.get('/users/:id', function(req, res){
  User.findById(req.params.id,function (err,user) {
    console.log(user);
    res.json(user)
  })
})

app.listen(3000, function(){
  console.log('running on port 3000...');
});
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

### 前台代码

我们先打开 mongo-express ，拷贝一个真实的 id 出来。然后粘贴到前台代码的 axios 请求中

App.js

```js
import React from 'react';
import axios from 'axios';

class App extends React.Component {
  constructor() {
    super();
    this.state={
      user:[]
    }
  }
  handleClick(e){
    e.preventDefault();
    console.log('handleClick......');
    axios.get('http://localhost:3000/users/58c36f80e40c622018b539eb')
      .then(respons => {
        console.log(respons);
        this.setState({user:respons.data.user})
      })
  }
  render(){
    return(
      <div>
        <div onClick={this.handleClick.bind(this)}>
          啦啦啦 Click Me
        </div>
        <div>
          username:
          {this.state.user.username}
        </div>
      </div>
    )
  }
}

export default App;
```

看吧，前台打开 index.html 文件，点一下 啦啦啦ClickMe ，就可以在页面上显示出从后台 mongodb 中取出对应 id 的用户的 username 了。
