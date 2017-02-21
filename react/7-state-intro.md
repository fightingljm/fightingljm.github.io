---
title:  State 状态值
---

- state 的作用

　　state 是 React 中组件的一个对象. React 把用户界面当做是状态机,想象它有不同的状态然后渲染这些状态,可以轻松让用户界面与数据保持一致.
　　React 中,更新组件的 state ,会导致重新渲染用户界面(不要操作 DOM ).`简单来说,就是用户界面会随着 state 变化而变化.`

- state 工作原理

　　常用的通知 React 数据变化的方法是调用 setState(data,callback) .这个方法会合并 data 到 this.state ,并重新渲染组件.渲染完成后,调用可选的 callback 回调.大部分情况不需要提供callback,因为React会负责吧界面更新到最新状态.

- 那些组件应该有 state ?

　　大部分组件的工作应该是从 props 里取数据并渲染出来.但是,有时需要对`用户输入,服务器请求或者时间变化等作出响应`,这时才需要 state .
　　组件应该尽可能的无状态化,这样能隔离 state ,把它放到最合理的地方( Redux 做的就是这个事情?),也能减少冗余并易于解释程序运作过程.
　　常用的模式就是创建多个只负责渲染数据的无状态(stateless)组件,在他们的上层创建一个有状态(stateful)组件并把它的状态通过 props 传给子级.有状态的组件封装了所有的用户交互逻辑,而这些无状态组件只负责声明式地渲染数据.

- 哪些应该作为 state ?

　　`state 应该包括那些可能被组件的事件处理器改变并触发用户界面更新的数据`.这种数据一般很小且能被JSON序列化.当创建一个状态化的组件的时候,应该保持数据的精简,然后存入 this.state .在 render() 中在根据 state 来计算需要的其他数据.因为如果在 state 里添加冗余数据或计算所得数据,经常需要手动保持数据同步.

- 哪些不应该作为 state ?

  this.state 应该仅包括能表示用户界面状态所需要的最少数据.因此,不应该包括:
    `计算所得数据`:
    `React组件`:在 render() 里使用 props 和 state 来创建它.
    `基于props的重复数据`:尽可能保持用 props 来做作为唯一的数据来源.把 props 保存到 state 中的有效的场景是需要知道它以前的值得时候,因为未来的 props 可能会变化. [第九节 :Props 组件的复用](./9-props-repeat.html)

参考文档 : [React中文文档](http://www.css88.com/react/docs/interactivity-and-dynamic-uis.html)

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
