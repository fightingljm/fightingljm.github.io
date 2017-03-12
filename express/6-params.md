---
title: 前台向后台传递参数
---

这里我们的小目标是，前台在发 axios 的 http 请求的时候，携带一个参数(字符串)到服务器端,这里的参数我们就用 id 这个家伙

先测试一下吧,随便一个字符串 '123456' 来传递吧.养成一个良好的习惯,先瞅瞅后台

### 后台接收请求

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

> 解释一坨:`req` 是 request 请求的缩写，express 用这个变量来接收前台发过来的请求。
`res` 是 response 响应的缩写，用来响应前台发过来的请求，实际作用就是从后台往前台发送数据。

> 再来一坨:上面的 id 写成其他的单词也可以，关键点就是前面的`冒号`。这样写之后前台请求中的字符串，就可以在后台通过 `req.params.id` 这个变量来拿到了。

同样先用 `curl` 来模拟一下请求,执行下面的命令

```
curl -X GET http://localhost:3000/users/123456
```

看到后台打印出 `123456` ,That's Proved the request was successful !

接下来就整整前台吧...

src/app.js

```js
import React from 'react';
import axios from 'axios';

class App extends Component {
  handleClick(e){
    e.preventDefault();
    console.log('handleClick......');
    axios.get('http://localhost:3000/users/123456')
      .then(response => console.log(response))
  }
  render(){
    return(
      <div onClick={this.handleClick.bind(this)}>
         Click Me
      </div>
    )
  }
}

export default App;
```

> Description:上面的字符串是 123456 ,还可以替换成任意的字符串,启动之后便可以在后台拿到

看吧,这样我们就实现了前台传递了一个 '123456' 的数据给后台.There are many kinds about 前台向后台传递数据的方式,This is the simplest kind.
