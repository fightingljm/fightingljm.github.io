---
title: TodoMVC Demo
---

- [TodoMVC 官网实例](http://todomvc.com/examples/react/#/)
- [React 官网 An Application](https://facebook.github.io/react/)

- 在 index.html 中引入了 Bootstrap [参见 Bootstrap 官网](http://getbootstrap.com/)

```html
<link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
```

- 首先我们把 App.js 作为上半部分标题和表单的组件;
- 把 TodoList.js 作为添加的一堆列表的组件;
- 把 TodoControl.js 作为筛选完成未完成的列表的组件;

```js
//App.js
import React from 'react';
import TodoList from './components/TodoList';
import TodoControl from './components/TodoControl';

class Test extends React.Component {
  constructor() {
    super();
    this.state={
      inputValue:'',
      data:[
        {text:'aaa',completed:true,id:1},
        {text:'bbb',completed:false,id:2},
        {text:'ccc',completed:false,id:3}
      ],
      visible:'All'
    }
  }
  handleInput(e){
    this.setState({inputValue:e.target.value})
  }
  handleSubmit(e){
    e.preventDefault();
    let newItem = this.state.inputValue.trim();//去掉首尾空格
    if(newItem.length===0){
      alert('输入内容不能为空')
    }else{
      let newTodo = {
        text:newItem,
        completed:false,
        id:new Date().getTime()
      }
      this.setState({data:[...this.state.data,newTodo]})
    }
    this.setState({inputValue:''})
  }
  handleCompleted(id){
    // console.log(index);
    let index = this.state.data.findIndex(item=>item.id===id)
    this.state.data[index].completed=!this.state.data[index].completed;
    this.setState({data:this.state.data})
  }
  handleRemove(id){
    let index = this.state.data.findIndex(item=>item.id===id)
    this.state.data.splice(index,1)
    //splice() 方法返回的是被删除的元素,不是一个数组,所以不能直接给data赋值
    this.setState({data:this.state.data})
  }
  handleFilter(visible){
    this.setState({visible:visible})
  }
  render(){
    let styles = {
      root:{
        maxWidth:'270px',
        margin:'0 auto'
      },
      h:{textAlign:'center'}
    }
    let showData;
    switch(this.state.visible){
      case 'Active':showData=this.state.data.filter(item=>!item.completed);break;
      case 'Completed':showData=this.state.data.filter(item=>item.completed);break;
      default:showData=this.state.data;
    }
    return(
      <div style={styles.root}>
        <h1 style={styles.h}>TODO</h1>

        <form onSubmit={this.handleSubmit.bind(this)} className="form-inline">
          <div className="form-group">
            <input type='text' value={this.state.inputValue} onChange={this.handleInput.bind(this)} className="form-control" />
            <button className="btn btn-default">Add #{this.state.data.length+1}</button>
          </div>
        </form>

        <TodoList data={showData} handleCompleted={this.handleCompleted.bind(this)} handleRemove={this.handleRemove.bind(this)}/>

        <TodoControl handleFilter={this.handleFilter.bind(this)} visible={this.state.visible}/>
        {/* {this.state.visible} */}
      </div>
    )
  }
}
export default Test;

```


```js
// TodoList.js
import React from 'react';
class TodoList extends React.Component {
  render(){
    // console.log(this.props);
    // let list = this.props.data.map((item,index) =>
    //   <li key={Math.random()}>
    //     <input type='checkbox' className='pull-left' defaultChecked={item.completed} onChange={()=> this.props.handleCompleted(index)}></input>
    //
    //     <span style={{textDecoration: item.completed ? 'line-through' : 'none',color: item.completed ? '#ccc' : '#000'}}>{item.text}</span>
    //
    //     <span className="glyphicon glyphicon-remove-circle pull-right" aria-hidden="true" style={{color:'red'}} onClick={()=> this.props.handleRemove(index)}></span>
    //
    //   </li>)
    let list = this.props.data.map(item=>
      <li key={item.id}>
        <input type='checkbox' className='pull-left' defaultChecked={item.completed} onChange={()=> this.props.handleCompleted(item.id)}></input>

        <span style={{textDecoration: item.completed ? 'line-through' : 'none',color: item.completed ? '#ccc' : '#000'}}>{item.text}</span>

        <span className="glyphicon glyphicon-remove-circle pull-right" aria-hidden="true" style={{color:'red'}} onClick={()=> this.props.handleRemove(item.id)}></span>

      </li>)
    return(
      <ul>
        {list}
      </ul>
    )
  }
}
export default TodoList;
```

```js
// TodoControl.js
import React from 'react';
class TodoControl extends React.Component {
  constructor() {
    super();
    this.state={
      btns:['All','Active','Completed']
    }
  }
  render(){
    return(
      <div>
        {/* <button className="btn btn-primary" onClick={()=> this.props.handleFilter('All')}>All</button>
        <button className="btn btn-warning" onClick={()=> this.props.handleFilter('Active')}>Active</button>
        <button className="btn btn-success" onClick={()=> this.props.handleFilter('Completed')}>Completed</button> */}
        {this.state.btns.map(
          item=><button key={Math.random()}
            onClick={()=> this.props.handleFilter(item)}
            className={this.props.visible===item ? 'btn btn-info' : 'btn btn-default'}>{item}</button>
        )}
      </div>
    )
  }
}
export default TodoControl;

```
