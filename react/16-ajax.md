---
title: Ajax
---

### 发送 ajax 请求的几种方法

- 原生: XMLHttpRequest()
- jQuery: $.ajax({type:'POST',data:string,success:function(){}})
- fetch: fetch('url').then(res=>res.json()).then(json=>do()).catch(err=>alert('err'))
- axios

**原生方法**

```js
  var ajax = new XMLHttpRequest();    
  ajax.onreadystatechange = function () {
    console.log(ajax.readyState);
    if(ajax.readyState==4 && ajax.status==200){
      console.log(typeof ajax.responseText);
      let data = JSON.parse(ajax.responseText);
      console.log(data);
      document.getElementById('name').innerHTML = data.name;
    }
  }  
  ajax.open("GET","https://api.github.com/users/fightingljm",true)
  ajax.send();
```

**jQuery 方法**

```js
<script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
```

`GET`

```js
  $.ajax({
    type:'GET',//默认为 GET ,可省略不写
    url:'https://api.github.com/users/fightingljm',
    success:function (data,status) {
      console.log(data,status);
    }
  })
```

`POST`

```js
  $.ajax({
    type:'POST',
    data:{
      accesstoken:'3aae6c64-7539-44d7-8494-ff0acaa36ce1'
    },
    url:' https://cnodejs.org/api/v1/accesstoken',
    success:function (data,status) {
      // console.log(data,status);
      $('#name').html(data.loginname)
      $('#img').append(`<img src="${data.avatar_url}"/>`)
    }
  })
```

`链式操作`

```js
  $.ajax({
    type:'POST',
    data:{
      accesstoken:'3aae6c64-7539-44d7-8494-ff0acaa36ce1'
    },
    url:' https://cnodejs.org/api/v1/accesstoken'
  }).done(function (data,status) {
    // console.log(data,status);
    $('#name').html(data.loginname)
    $('#img').append(`<img src="${data.avatar_url}"/>`)
  }).fail(function () {
    alert('aaa')
  })
```

`跨域请求`

- 跨域请求 http 协议的规定就是不同源之间不能进行资源共享
- https://api.github.com:80 这一段如果有不同的都叫跨域
- 一般后台设置 Access-Control-Allow-Origin:*  就能解决

```js
  $.ajax({
    dataType:'jsonp',
    jsonp:'callback',
    //jsonp 请求只能 GET
    url:' https://api.douban.com/v2/book/1220562'
  }).done(function (data,status) {
    console.log(data,status);
  }).fail(function (xhr,error) {
    alert(error)
  })
```

**Fetch 方法**

`GET`

```js
  fetch('https://api.github.com/users/fightingljm',{method:'GET'})
    .then(response=>response.json())
    .then(json=>console.log(json))
```

`POST`

```js
  fetch('https://cnodejs.org/api/v1/accesstoken', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({
      accesstoken:'3aae6c64-7539-44d7-8494-ff0acaa36ce1'
    })
  })
    .then(res=>res.json())
    .then(json=>console.log(json))
```

拓展理解 `Promise then`

```js
  new Promise(function (resolve,reject) {
    console.log('Run!');
    setTimeout(function () {
      reject()
    },3000);
  }).then(function () {
    console.log('thing.then()...');
  }).catch(function () {
    console.log('thing.catch()...');
  })
```

**axios 方法**

[axios 的使用](https://github.com/mzabriskie/axios)

```js
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

`GET`

```js
  axios.get('https://api.github.com/users/fightingljm')
    .then(response=> console.log(response))
    .catch(error=> console.log(error))
```

`POST`

```js
  axios.post('https://cnodejs.org/api/v1/accesstoken',{
    accesstoken:'3aae6c64-7539-44d7-8494-ff0acaa36ce1'
  })
    .then(response=> console.log(response.data))
    .catch(error=> console.log(error))
```

### cnodejs

[cnodejs 官网](https://cnodejs.org/)

首先装包

```
$ npm install axios
```

`App.js`

```js
import React from 'react';
import axios from 'axios';
import $ from 'jquery';

class App extends React.Component {
  constructor() {
    super();
    this.state={
      data:[]
    }
  }
  componentDidMount(){
    //1.axios
    axios.get('https://cnodejs.org/api/v1/topics?page=1&limit=5')
      .then(res => this.setState({data:res.data.data}))

    //2.Fetch
    fetch('https://cnodejs.org/api/v1/topics')
      .then(res => res.json())
      .then(json => this.setState({data:json.data}))

    //3.原生
    var _this=this;
    var ajax = new XMLHttpRequest();
    ajax.onreadystatechange = function () {
      if(ajax.readyState==4 && ajax.status==200){
        let data = JSON.parse(ajax.responseText);
        _this.setState({data:data.data})
      }
    }
    ajax.open("GET","https://cnodejs.org/api/v1/topics",true)
    ajax.send();

    //4.jQuery
    $.ajax({
      type:'GET',
      url:'https://cnodejs.org/api/v1/topics',
      success:data => this.setState({data:data.data})
    })
  }
  render(){
    // console.log(this.state.data);
    let blog = this.state.data.map(item =>
      <div key={item.id}>
        <img src={item.author.avatar_url}/>
        <a href={`https://cnodejs.org.topics/${item.id}`} target='_blank'>{item.title}</a>
        <span>浏览量:{item.visit_count}</span>
      </div>
    )
    return(
      <div>
        {blog}
      </div>
    )
  }
}

export default App;
```
