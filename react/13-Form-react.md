---
title: Form
---

在 React 中,Form 表单的 value 输入不了了,不用担心,高手在民间

解决办法有两种呢

- 受控组件onChange,state 控制 value
- 非受控组件[Uncontrolled Components](https://facebook.github.io/react/docs/uncontrolled-components.html) defaultValue,不可设置 value

来看一个小李子, [更多示例](https://facebook.github.io/react/docs/forms.html) 详见官网

```js
import React from 'react';
import Checkbox from './Checkbox.js'
class Form extends React.Component {
  constructor() {
    super();
    this.state={
      inputValue:'',
      textarea:'',
      select:'',
      radio:'famale'
    }
  }
  handleSubmit(e){
    e.preventDefault();
    //ajax
    this.refs.form.reset()
  }
  handleChange(e){
    console.log(e.target.value);
    this.setState({inputValue:e.target.value})
  }
  handleTextarea(e){
    console.log(e.target.value);
    this.setState({textarea:e.target.value})
  }
  handleSelect(e){
    console.log(e.target.value);
    this.setState({select:e.target.value})
  }
  render(){
    return(
      <div>
        <form action='http://blog.duopingshidai.com' method='POST'
        onSubmit={this.handleSubmit.bind(this)} ref='form'>

          {/* 受控输入框 */}
          {/* <input type='text' defaultValue='123'/> */}
          <input type='text' value={this.state.inputValue}
            onChange={this.handleChange.bind(this)}/>
          <button>提交</button> {/* 默认提交 */}
          {/* <button type='button'>普通</button>
          <button type='reset'>重置</button> */}

          <br/>
          <br/>{/* 受控评论框 */}
          <textarea value={this.state.textarea}
          onChange={this.handleTextarea.bind(this)}/>

          <br/>{/* 受控下拉菜单 */}
          <select value={this.state.select} onChange={this.handleSelect.bind(this)}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>

          <br/>{/* 非受控单选按钮 */}
          <input type='radio' name='test1' value='male'/>男
          <input type='radio' name='test1' value='famale' defaultChecked/>女

          <br/>{/* 受控单选按钮 */}
          <input type='radio' name='test2' value='male'
            checked={this.state.radio==='male' ? true : false}
            onChange={(e)=>this.setState({radio:e.target.value})}/>男
          <input type='radio' name='test2' value='famale'
            checked={this.state.radio==='famale' ? true : false}
            onChange={(e)=>this.setState({radio:e.target.value})}/>女

          <br/>
          <Checkbox/>
        </form>
      </div>
    )
  }
}
export default Form;
```

- 下面是一个多选框的示例,这里我要添加一点小色彩,拿到被选中的多选框的 value
- 这里并不像想象的简单,如果单纯输出一下,那么点击选框,不管他有没有被选中就会输出一次,
  如下 JSX 语法注释里所示,所以我要开动大脑转个弯
- 下面还用到了 findIndex() 方法

```js
import React from 'react';
class Checkbox extends React.Component {
  constructor() {
    super();
    this.state={
      checkboxValue:[]//数组是存放被选中的多选框的
    }
  }
  handleChange(e){
    let cbValue = this.state.checkboxValue;//先拿到存放被选中多选框的数组
    let nowValue = e.target.value;//再拿到被点击的多选框的value
    let index = cbValue.findIndex(element=>element===nowValue)
    //findIndex()方法返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。
    if(index===-1){
      cbValue.push(nowValue)
    }else{
      cbValue.splice(index,1)
    }
    this.setState({checkboxValue:cbValue})
  }
  render(){
    console.log(this.state.checkboxValue);
    return(
      <div>
        {/* <input type='checkbox' value='apple' name='fruits' defaultChecked/>苹果 */}
        {/* <input type='checkbox' value='apple' name='fruits'
          onChange={e=>console.log(e.target.value)}/>苹果 */}
        <input type='checkbox' value='apple' name='fruits'
          onChange={this.handleChange.bind(this)}/>苹果

        <input type='checkbox' value='banana' name='fruits'
          onChange={this.handleChange.bind(this)}/>香蕉

        <input type='checkbox' value='pear' name='fruits'
          onChange={this.handleChange.bind(this)}/>梨子

      </div>
    )
  }
}
export default Checkbox;
```
