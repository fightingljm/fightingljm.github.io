---
title: React refs
---


下载 jquery 包

```
$ npm i --save jquery
```

```js
//App.js
import React from 'react';
import $ from 'jquery';
import Test from './Test';
class App extends React.Component {
  componentDidMount(){
    //用 jquery 方法或者原生 js 拿到和改变 id 为 test 的 div
    console.log(document.getElementById('test'));

    let oDiv = document.querySelector('#test')
    oDiv.style.color='blue'

    $('#test').css({color:'red'}).append('<h1>hello</h1>')

    //用 refs 拿到 ref 为 test 和 aaa 的标签或者组件
    console.log(this.refs.test);
    console.log(this.refs.aaa);

    //用 trigger 方法 使点击 btn1 时触发 btn2 的点击事件
    $('#btn1').click(function () {
      $('#btn2').trigger('click')
    })
    $('#btn2').click(function () {
      alert('我被电击了')
    })

    //用 refs 拿到组件 ref 为 aaa 里的 getValue() 方法
    console.log(this.refs.aaa.getValue());
  }
  render(){
    return(
      <div>
        <div id='test'>aaa</div>
        <div ref='test'>bbb</div>
        <button id='btn1'>1</button>
        <button id='btn2'>2</button>
        <button id='btn3' onClick={()=>this.refs.aaa.handleClick()}>3</button>
        {/* #btn3 用 refs 拿到组件 ref 为 aaa 里的 handleClick() 方法 */}
        <Test ref='aaa'/>
      </div>
    )
  }
}
export default App;


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
        <button onClick={this.handleClick.bind(this)}>aaa</button>
      </div>
    )
  }
}
```
