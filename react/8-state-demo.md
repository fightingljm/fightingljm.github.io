---
title:  State 小例子
---

- demo 1:

```js
//App.js
import React from 'react';
let date = ['这是第一条消息','这是第二条消息','这是第三条消息','hello world']
class App extends React.Component {
  constructor() {
    super();
    this.state = {
      text : date
    }
  }
  render(){
    // console.log(this.state.text);
    // let content = this.state.text.map( item => <p>今天的消息是:{item}</p>)
    let content = this.state.text.map( item => <div key = {Math.random()}>
      <p>今天的消息是:{item}</p><button>删除</button>
    </div>)
    // ` key = {Math.random()}`防止警告,加载最外层的标签上(这是React底层用到的,为了做底层计算的)
    return(
      <div>
        {/*{date} 因为 React 的行为,会遍历数组,以上写 content 的意思是让文本返回标签*/}
        {content}
      </div>
    )
  }
}
export default App;
```

- demo 2:选项卡

```js
//Selectbar.js
import React from 'react';

import Box1 from './Box1'
import Box2 from './Box2'
import Box3 from './Box3'

class Selectbar extends React.Component {
  constructor() {
    super();
    this.state = {
      show : 0
    }
  }
}
handleClick(num){
  this.setState({show:num})
}
render(){
  let styles = {width:'200px', border:'1px solid #ccc', backgroundColor: this.state.show===0 ? '#ccc' : this.state.show===1 ? 'red' : 'yellow'}
  //0,1,2 是给handleClick 函数传递的参数,用来修改 state
  return(
    <div>
      <button onClick={this.handleClick.bind(this,0)}>Select 1</button>
      <button onClick={this.handleClick.bind(this,1)}>Select 2</button>
      <button onClick={this.handleClick.bind(this,2)}>Select 3</button>
      <div style = {styles}>
        {
          this.state.show===0 ? <Box1/> :
          this.state.show===1 ? <Box2/> : <Box3/>
        }
      </div>
    </div>
  )
}
export default Selectbar;


//Box1.js
import React from 'react';
class Box1 extends React.Component {
  render() {
    return(
      <div>
        <p>商品名称: pro</p>
        <p>商品名称: pro</p>
        <p>商品名称: pro</p>
      </div>
    )
  }
}
export default Box1;

//Box2.js
import React from 'react'
class Box2 extends React.Component {
  render() {
    return(
      <div>
        <p>商品名称: iphone</p>
        <p>商品名称: iphone</p>
        <p>商品名称: iphone</p>
      </div>
    )
  }
}
export default Box2;

//Box3.js
import React from 'react'
class Box3 extends React.Component {
  render() {
    return(
      <div>
        <p>商品名称: Mac</p>
        <p>商品名称: Mac</p>
        <p>商品名称: Mac</p>
      </div>
    )
  }
}
export default Box3;

//同时修改 index.js 为
import React from 'react';
import ReactDOM from 'react-dom';

import Selectbar from './Selectbar';

ReactDOM.render(
  <Selectbar/> ,document.getElementById('app')
)

```

- demo 3: 计时器随机选择

```js
//Eatwhat.js
import React from 'react'
let date = ['牛奶','蛋挞','豆干','胡萝卜','青菜','','']
class Eatwhat {
  constructor() {
    super();
    this.state = {
      start : false,
      date,
      text : ''
    }
  }
  select(){
    let result = this.state.date[Math.floor(Math.random()*this.state.date.length)];
    this.setState({text:result})
  }
  handleClick(){
    // this.setState({start:!this.state.start})
    if(this.state.start){
      //clearInterval
      //false
      clearInterval(this.interval)
      this.setState({start:false})
    }else{
      //计时器
      //true
      this.interval=setInterval(()=>this.select(), 100);
      this.setState({start:true})
    }
  }
  render(){
    return(
      <div>
        <p>今天吃什么:{this.state.text}</p>
        <button onClick={this.handleClick.bind(this)}>{this.state.start ? '停止' : '开始'}</button>
      </div>
    )
  }
}
export default Eatwhat;


//同时修改 index.js 为
import React from 'react';
import ReactDOM from 'react-dom';

import Eatwhat from './Eatwhat';

ReactDOM.render(
  <Eatwhat/> ,document.getElementById('app')
)
```

- demo 3:[阮一峰transition](http://www.ruanyifeng.com/home.html)

```js
//RuanyfTrans.js
import React from 'react';
class RuanyfTrans extends React.Component {
  constructor() {
    super();
    this.state = {
      show:true
    }
  }
  handleClick(){
    this.setState({start:!this.state.show})
  }
  render(){
    //两个状态
    let style  = {
      top :this.state.show ? '35%' : '10%',
      height :this.state.show ? '30vh' : '4.5vh',
      overflow :this.state.show ? 'auto' : 'hidden'
    }
    return(
      <div className='container' style= {style}>
        <button onClick={this.handleClick.bind(this)}>{this.state.show ? '上' : '下'}</button>
        <br/>
        <p>Jinmeng Liu 的个人网站<br/><a href='https://fightingljm.github.io'>https://fightingljm.github.io</a></p>
        <hr/>
        <p className='select'> >> <a href='#'>Blog</a> / <a href='#'>Tweets</a> / <a href='#'>About</a></p>
        <hr/>
        <p>博客标题</p>
      </div>
    )
  }
}
export default RuanyfTrans;


//同时修改 index.js 为
import React from 'react';
import ReactDOM from 'react-dom';

import RuanyfTrans from './RuanyfTrans';

ReactDOM.render(
  <RuanyfTrans/> ,document.getElementById('app')
)
```
```css
/*main.css*/
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
a{
  text-decoration: none;
}
html,body{
  height: 100%;
}
body{
  position: relative;
  background-image: url('http://www.ruanyifeng.com/images_pub/pub_292.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: 50% 50%;
}
.container{
  position: absolute;
  width: 35%;
  height: 30vh;
  top: 35%;
  left: 45%;
  background-color: rgba(0,0,0,0.3);
  border-radius: 10px;
  margin-left: -15%;
  text-align: center;
  transition:all 1s ease;
}
.select{
  margin: 1em 0.8em;
  line-height: 180%;
  background-color: rgba(0,0,0,0.5);
  padding: 0.25rem;
  border-radius: 0.5em;
  text-shadow: 0 1px 1px #333;
  color: #fefefe;
}
.select a{
  color: #fefefe;
}
.select:hover{
  background-color: #fefefe;
  color: #222;
}
.select:hover a{
  color: #222;
}
.select a:hover{
  text-decoration: underline;
}

```
