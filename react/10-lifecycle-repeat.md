---
title:  React 生命周期
---

[Vue 生命周期图示](http://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram)

### 初始化,首次挂载

- constructor      (initial state)
- componentWillMount      (will mount)
- render      (render)
- componentDidMount      (did mount)

### 更新阶段(state props 发生变化时触发)

state 发生变化时

- shouldComponentUpdate(必须有返回值(有两个参数 nextProps 和 nextState))     (should update)
- componentWillUpdate(有两个参数 nextProps 和 nextState)     (will update)
- render      (render)
- componentDidUpdate(有两个参数 prevProps 和 prevState)     (did update)

props 发生变化时

- componentWillReceiveProps(有一个参数 nextProps)     (Test will receive props=== Object {childNum: 2})
- componentWillUpdate(有两个参数 nextProps 和 nextState)      (Test will update=== Object {childNum: 2} null)

### 销毁

- componentWillUnmount      (我要被销毁了)

### 三个过程整合示例

```js
//App.js
import React from 'react';
import Test from './Test';
class App extends React.Component {
  constructor() {
    super();
    this.state = {
      num:1,
      show:true
    }
    console.log('initial state');
  }
  componentWillMount(){
    console.log('will mount');
  }
  componentWillReceiveProps(nextProps)(
    console.log('will receive props===',nextProps);
  )
  shouldComponentUpdate(nextProps,nestState){
    console.log('should update===',nextProps,nextState);
    return true
  }
  componentWillUpdate(nextProps,nextState){
    console.log('will update===',nextProps,nextState);
  }
  componentDidUpdate(prevProps,prevState){
    console.log('did update===',prevProps,prevState);
  }
  render(){
    console.log('render');//挂载
    return(
      <div>
        数值:{this.state.num}
        <button onClick={()=>this.setState({num:this.state.num+1})}>click</button>
        <button onClick={()=>this.setState({show:false})}>销毁 Test</button>
        {this.state.show ? <Test childNum={this.state.num}/> : null}
      </div>
    )
  }
  componentDidMount(){
    console.log('did mount');
  }
}
export default App;

//Test.js
import React from 'react';
export default class Test extends React.Component {
  componentWillReceiveProps(nextProps){
    console.log('Test will receive props===',nextProps);
  }
  componentWillUpdate(nextProps,nextState){
    console.log('Test will update===',nextProps,nextState);
  }
  componentWillUnmount(){
    console.log('我要被毁灭了');
  }
  render(){
    return(
      <div>
        我是test组件:{this.props.childNum}
      </div>
    )
  }
}

```
