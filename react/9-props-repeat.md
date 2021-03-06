---
title:  Props 组件的复用
---

- props 的作用

　组件中的props是一种父级向子级传递数据的方式.

- props 的特点

```
1 只能从父组件传给子组件;
2 子组件通过 {this.props.[name]} 获取 props 的值;
3 子组件设置默认属性
  Btn.defaultProps = {
    title:'Default',
    col: '#333',
    bgc: '#fff',
    bdc: '#ccc'
  };
4 子组件设置属性格式验证
  Btn.propTypes = {
    title:React.PropTypes.[string],
    col: React.PropTypes.[string],
    bgc: React.PropTypes.[string],
    bdc: React.PropTypes.[string],
    add: React.PropTypes.func.isRequired
  };
```
React.PropTypes.[更多类型参考](https://facebook.github.io/react/docs/typechecking-with-proptypes.html)

- 复合组件

```js
var Avatar = React.createClass({
  render: function() {
    return (
      <div>
        <ProfilePic username={this.props.username} />
        <ProfileLink username={this.props.username} />
      </div>
    );
  }
});

var ProfilePic = React.createClass({
  render: function() {
    return (
      <img src={'http://graph.facebook.com/' + this.props.username + '/picture'} />
    );
  }
});

var ProfileLink = React.createClass({
  render: function() {
    return (
      <a href={'http://www.facebook.com/' + this.props.username}>
        {this.props.username}
      </a>
    );
  }
});

React.render(
  <Avatar username="pwh" />,
  document.getElementById('example')
);

// 从属关系:
//   Avatar拥有ProfilePic和ProfileLink的实例,拥有者就是给其它自口岸设置props的那个组件..更正式的说,如果　
// 组件Y在render()方法创建了组件X,那么Y就拥有X.
//   React 里，数据通过上面介绍过的 props 从拥有者流向归属者
```

[以上是 React 的官网案例](http://www.css88.com/react/docs/multiple-components.html)

- demo 1:简单的 Btn 复用

```js
//Btn.js
import React from 'react'
import './main.css'

class Btn extends React.Component {
  render(){
    let styles = {
      color:this.props.col,
      backgroundColor:this.props.bgc,
      borderColor:this.props.bdc
    }
    console.log(this.props);
    return (
      <button style={styles} className='btn'>{this.props.title}</button>
    )
  }
}
//当我们未传入时,用以下方式设置默认的Btn
Btn.defaultProps={
  title:'Default',
  col:'#333',
  bgc:'#fff',
  bdc:'#ccc'
}
export default Btn;
//当我们传入错误的,以下方式会报一条警告
Btn.propTypes={
  title:React.PropTypes.string,
  col:React.PropTypes.string,
  bgc:React.PropTypes.string,
  bdc:React.PropTypes.string
}

//App.js
import React from 'react';
import Btn from './Btn';
class App extends React.Component {
  render(){
    return(
      <div>
        <Btn/>
        <Btn title='Primary' col='#fff' bgc='#337ab7' bdc='#2e6da4'/>
        <Btn title='Success' col='#fff' bgc='#5cb85c' bdc='#4cae4c'/>
        <Btn title='Info' col='#fff' bgc='#5bc0de' bdc='#46b8da'/>
        <Btn title='Warning' col='#fff' bgc='#f0ad4e' bdc='#eea236'/>
        <Btn title='Danger' col='#fff' bgc='#d9534f' bdc='#d43f3a'/>
        <Btn title='Danger' col='#fff' bgc={d9534f} bdc='#d43f3a'/>
        {/*以上写法会报一条警告,因为 Btn.js 里边的 `add: React.PropTypes.func.isRequired`*/}
      </div>
    )
  }
}
export default App;

```
```css
/*main.css*/
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.btn{
  display: inline-block;
  padding: 6px 12px;
  margin: 10px 5px;
  font-size: 14px;
  font-weight: 400;
  line-height: 1.42857143;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-image: none;
  border: 1px solid transparent;
  border-radius: 4px;
  font-family: inherit;
}
```

*通过 Btn 的例子描述一下 props 父级向子级传递函数的方式*

```js
//Btn.js
import React from 'react';
import './main.css'
class Btn extends React.Component {
  handleClick(){
    this.props.add(this.props.count)
  }
  //handleClick 是点击事件绑定的方法,这个方法用来执行父组件传递过来的 add 方法
  render(){
    let styles = {
      color: this.props.col,
      backgroundColor: this.props.bgc,
      borderColor: this.props.bdc
    }
    return(
      <div>
        <p>我是子组件的 button</p>
        <button style={styles} className='btn' onClick={this.handleClick.bind(this)}>{this.props.title}</button>
      </div>
    )
    //以上 button 和 handleClick 等价于
    //<button style={styles} className='btn' onClick= {()=>this.props.add(this.props.count)}> {this.props.title} </button>
  }
}
Btn.propType = {
  title:'Default',
  col: '#333',
  bgc: '#fff',
  bdc: '#ccc'
}
Btn.propTypes = {
  title:React.PropTypes.string,
  col: React.PropTypes.string,
  bgc: React.PropTypes.string,
  bdc: React.PropTypes.string,
  add: React.PropTypes.func.isRequired
}
export default Btn;


//App.js
import React from 'react';
import Btn from './Btn';

class App extends React.Component {
  constructor() {
    super();
    this.state={
      num:0
    }
  }
  addNum(val){
    this.setState({num:this.state.num+val})
  }
  render(){
    <div>
      <Btn bgc='#f0ad4e'/>
      {/*以上写法会报一条警告,因为 Btn.js 里边的 `add: React.PropTypes.func.isRequired`*/}

      数字是: {this.state.num}<br/>
      <Btn bgc='#5bc0de' add={this.addNum.bind(this)}/>
      {/*父组件给子组件传递一个 add 方法,这个 add 方法来自父组件的 addNum 方法*/}

      <Btn bgc='#f0ad4e' add={this.addNum.bind(this)} count={5} title='+5'/>
      <Btn bgc='#f0ad4e' add={this.addNum.bind(this)} count={-10} title='-10'/>
    </div>
  }
}
```

- demo 2:简单的 Card 复用

```js
//Card.js
import React from 'react';
import './main.css'
class Card extends React.Component {
  render(){
    console.log(this.props);
    return(
      <div className='card'>
        <div className='card-index'>{this.props.index}</div>
        <div className='card-desc'>
          <h3>{this.props.title}</h3>
          <p>{this.props.date}</p>
        </div>
      </div>
    )
  }
}
Card.defaultProps={
  index:'Default',
  title:'这是默认标题',
  date:'这是默认日期'
}
export default Card;

//App.js
import React from 'react';
import Card from './Card';
let styles = [
  {index:1,title:'标题一',date:'2017.1.12'},
  {index:2,title:'标题二',date:'2017.1.13'},
  {},
  {index:4,title:'标题四',date:'2017.1.15'}
]
class App extends React.Component {
  constructor() {
    super();
    this.state = {
      styles
    }
  }
  render(){
    //以下注释里是复杂的方法.
    //把样式放到数组里,用 map() 方法遍历数组更简便
    return(
      <div>
      {/*<Card index='' title='' date=''/>
      <Card index='' title='' date=''/>
      <Card index='' title='' date=''/>
      <Card index='' title='' date=''/>*/}
        {
          this.state.styles.map(item => <card title={item.title} index={item.index} date={item.date} key={Math.random()}/>)
        }
      </div>
    )
  }
}
export default App;
//以上 `title={item.title} index={item.index} date={item.date}` 等价于 `{...item}`
```

```css
/*main.css*/
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.card{
  width: 100%;
  max-width: 760px;
  margin: 10px auto;
  box-shadow:rgba(0, 0, 0, 0.2) 0px 1px 6px, rgba(0, 0, 0, 0.2) 0px 1px 4px;
  height: 80px;
}
.card-index{
  float: left;
  width: 90px;
  height: 80px;
  text-align: center;
  line-height: 80px;
  color: #fff;
  background-color: #00bcd4;
  font-size: 22px;
}
.card-desc{
  float: left;
  height: 80px;
  padding: 10px 0 0 10px;
  color: #777;
}
.card-desc p{
  opacity: 0.7;
}
```

- demo 3:新建组件中套用N个标签和新组件

```js
//当 Children 里引入 Son ,Son 里套用 p 标签和 Test 新组件时,应该这样write

//Children.js
import React from 'react';
import Son from './Son'
import Test from './Test'
class Children extends React.Component {
  render(){
    return(
      <div>
        Children
        <Son>
          <p>123</p>
          <p>123</p>
          <Test/>
          <Test/>
        </Son>
      </div>
    )
  }
}
export default Children;

//Test.js
import React from 'react';
class Test extends React.Component {
  getValue(){
    return this.refs.input.value
  }
  handleClick(){
    alert('aaa')
  }
  render(){
    return(
      <div>
        我是测试组件
        <input type='text' defaultValue='111' ref='input'/>
        <button onClick={this.handleClick.bind(this)}>aa</button>
      </div>
    )
  }
}
export default Test;

//Son.js
import React from 'react';
class Son extends React.Component {
  constructor() {
    super();
    this.state={

    }
  }
  render(){
    console.log(this.props.children);
    return(
      <div>
        Son
        {this.props.children}
        {/* 这里是重点 */}
      </div>
    )
  }
}
export default Son;
```

### React 中 `state` 和 `props` 的区别

**props和state都是用于描述component状态的，并且这个状态应该是与显示相关的**

`State`  [第七节 :State 状态值](./7-state-intro.html)

  如果 component 的某些状态需要被改变，并且会影响到 component 的 render ，那么这些状态就应该用 state 表示。
例如：一个购物车的 component ，会根据用户在购物车中添加的产品和产品数量，显示不同的价格，那么“总价”这个状态，就应该用 state 表示。

`Props`

  如果 component 的某些状态由外部所决定，并且会影响到 component 的 render ，那么这些状态就应该用 props 表示。
例如：一个下拉菜单的 component ，有哪些菜单项，是由这个 component 的使用者和使用场景决定的，那么“菜单项”这个状态，就应该用 props 表示，并且由外部传入。


[深入理解 React](http://www.css88.com/react/docs/thinking-in-react.html)
