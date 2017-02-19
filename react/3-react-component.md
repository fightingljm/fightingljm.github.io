---
title: component 组件
---

### component 组件(3种方式)

  `首字母要大写`

- 方法一: var Hello = React.createClass({})  es5的写法,过时不用

 ```js
console.log(React);//可以打印出React下的方法,其中有一个createClass以下会用到
 let Greeting = React.createClass({
   render: function () {
     return <h1>Hello</h1>
   }
 })
 ReactDOM.render(
   <Greeting></Greeting> ,document.getElementById('app')//自定义的标签不会被渲染,会去找返回值
 )
 ```

- 方法二:返回一段Dom结构的组合,也不太常用

 ```js
 function Greeting() {
   return (<h1>Hello</h1>)
 }
 ReactDOM.render(
   <Greeting></Greeting> ,document.getElementById('app')
 )
 ```

 - 方法三:

 ```js
 class Hello extends React.Component {
  render(){
    return(
      <div>
         <h1>hello react</h1><br/>
         <h2>略略略</h2>
      </div>
    )
  }
}
ReactDOM.render(
  <Hello></Hello> ,document.getElementById('app')
)
```

也可以用导入导出的方式:

```js
//Header.js
import React from 'react';
class Header extends React.Component {
  render(){
    return(
      <div>
         我是头部
      </div>
    )
  }
}
export default Header;

//index.js
import React from 'react';
import ReactDOM from 'react-dom';

import Header from './Header'
ReactDOM.render(
  <Header></Header> ,document.getElementById('app')
)
```
