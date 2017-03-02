---
title: TodoMVC Demo
---

### TodoMVC

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
      data:[],
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
  componentWillMount(){
    if(localStorage.todos){
      this.setState({data:JSON.parse(window.localStorage.getItem('todos') || '[]')})
    }
  }//localStorage HTML5 本地存储 以及 JSON 格式会在本节末尾详细介绍
  render(){
    localStorage.setItem('todos',JSON.stringify(this.state.data))
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

### localStorage

这里一定要好好扯扯这个 `localStorage`

首先来说,他是对 cookie 的优化,那么 cookie 是什么呢

- cookie 和 localStorage 一样是用来缓存信息的,以避免在 url 上面传递参数,但是他回带来一些问题

```
① cookie大小限制在4k左右，不适合存业务数据
② cookie每次随HTTP事务一起发送，浪费带宽
```

- localStorage 方便了用户在客户端存储数据,并且不会随着HTTP传输

```
① localstorage大小限制在500万字符左右，各个浏览器不一致
② localstorage在隐私模式下不可读取
③ localstorage本质是在读写文件，数据多的话会比较卡（firefox会一次性将数据导入内存，想想就觉得吓人啊）
④ localstorage不能被爬虫爬取，不要用它完全取代URL传参
```

基本了解了 localStorage 相比 cookie 的优点,那现在就来关注以下使用 localStorage 的正确姿势

基础知识

```
localstorage存储对象分为两种：

① sessionStorage： session即会话的意思，在这里的session是指用户浏览某个网站时，从进入网站到关闭网站这个时间段，session对象的有效期就只有这么长。

② localStorage： 将数据保存在客户端硬件设备上，不管它是什么，意思就是下次打开计算机时候数据还在。

两者区别就是一个作为临时保存，一个长期保存。
```

我们用 localStorage.setItem() 和 localStorage.getItem() 方法来设值和取值,如上 Todomvc 代码所示

### JSON

还是看代码体会一下就好了

```js
  let arr = {name:'liu',age:22};
  let json = JSON.stringify(arr);
  console.log(json)//{"name":"liu","age":22}

  let jsonObj = JSON.parse(json)
  console.log(jsonObj)//Object age: 22 name: "liu" __proto__: Object
  console.log(jsonObj.name)//liu
```
