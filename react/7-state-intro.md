---
title:  State 状态值
---

组件的内部数据会保存到 State 值之中。

- 定义state

```js
constructor(){
  super();
  this.state = {
    num : 1
  }
}
```

- 修改state

```js
this.setState({})
```

*认识 state 之前先搭建环境并了解一点题外知识*

搭建环境和上一节的一样拷贝过来,src文件中只要保留 App.js 和 index.js

```js
//App.js
import React from 'react';
class App extends React.Component {
  aaa(){
    console.log('aaa');
  }
  render(){
    return(
      <div>
        <button onClick={this.aaa}>输出</button>
        {/*这里的 onClick 事件的写法*/}
      </div>
    )
  }
}
export default App;

//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
ReactDOM.render(
  <App/> ,document.getElementById('app')
)
```

`接下来要真正运用 state`

```js
//App.js
import React from 'react';

class App extends React.Component {
  //定义 State
  constructor(){
    super();
    this.state = {
      num : 1,
      show : false
    }
  }
  //修改 State
  handleAdd(){
    console.log(this);//在未给他绑定 this 之前,这里会输出 null;绑定之后会输出 App {}
    this.setState({num : this.state.num+1})
  }
  handleCut(){
    this.setState({num : this.state.num-1})
  }
  handleFan(){
    this.setState({show : !this.state.show})
  }
  render(){
    return(
      <div>
        数字是:{this.state.num}<br/>
        <button onClick={this.handleAdd.bind(this)}>+1</button>
        <button onClick={this.handleCut.bind(this)}>-1</button>
        <button onClick={this.handleFan.bind(this)}>{this.state.show ? '隐藏' :'显示'}</button>
        {/*<p>你现在显示吗 {this.state.show ? '显示' : '不显示'}</p>*/}
        <p style = {{display:this.state.show ? 'block' : 'none'}}>显示或隐藏</p>
      </div>
    )
    //通过点击事件调用修改 State 值的方法,并给这个方法绑定this到 App 这个类上(原因:如果不绑定,在这个方法里执行 `console.log(this);` 语句会输出 null ,如上所示;绑定到 App 这个类就可以找到 state 并修改它)
  }
}
export default App;
```
