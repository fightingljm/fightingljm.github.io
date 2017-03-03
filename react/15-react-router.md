---
title: React Router
---

- [react-router-cn](https://github.com/react-guide/react-router-cn)
- [React Router 中文文档](http://react-guide.github.io/react-router-cn/index.html)
- [react-router on github](https://github.com/ReactTraining/react-router)
- [react-router-tutorial](https://github.com/reactjs/react-router-tutorial)


### Router,hashHistory,Route

首先下载 react-router

```
$ npm install react-router --save
```

使用时需要类似这样引入

```js
import { Router,Route,hashHistory,browserHistory,Redirect,IndexRoute,Link } from 'react-router'
```

>注意:用到 browserHistory 时,需要在 webpack.dev.config.js 中添加如下内容

```js
  devServer: {
    port: 8080,
    inline: true,
+   historyApiFallback:true
  },
```

React Router 是完整的 React 路由解决方案,具体示例如下

```js
//App.js
import React from 'react';
import { Router,Route,hashHistory,browserHistory,Redirect,IndexRoute } from 'react-router';

import Home from './Home'
import About from './About'
import Work from './Work'
import Me from './Me'
import PageNotFound from './PageNotFound'
import Index from './IndexRoute'
import Blog from './Blog'

class App extends React.Component {
  render(){
    return(
      <Router history={browserHistory}>
        <Route path='/' component={Home}>
          <IndexRoute component={Index}/>

          <Route path='about' component={About}>
          {/*  以上不加 / , 访问 /aa/about;如果加上 / ,访问 /about  */}
            <Route path='me' component={Me}/>
          <Route/>

          {/* 传单个组件用 component ;传多个组件用 components */}
          <Route path='work' component={Work}/>
          <Route path='work' components={{main:Work,body:PageNotFound}}/>

          {/* 路由变量 */}
          <Route path='blog/:title' component={Blog}/>

          {/* 404 页面放到最后 */}
          <Route path='404' component={PageNotFound}/>
          {/* Redirect 重定向 */}
          <Redirect from='contact' to='about'/>

          <Route path='404' component={PageNotFound}/>
          <Redirect from='*' to='404'/>
        </Route>
      </Router>
    )
  }
}

export default App;
```

- `hashHistory`

>给我们的路径添加一个 '/#/' ,防止浏览器跳转到其他界面,始终拿到的是 index.html ,不需要服务器支持

- `browserHistory`

>利用我们的服务器,实现页面跳转,不论访问任何路径,都返回 index.html ,这样路径看起来美观正常,但是需要有本地服务器支持,如果托管在 github, coding 等网站上的话用不了

- `IndexRoute`

>IndexRoute即处理页面初始进入时候的组件展示，等路由切换的时候，再根据其他路由规则进行组件的切换展示

### Link

Link是react-router提供的导航组件，可以直接使用进行路由切换

```js
//NavBar.js
import React from 'react';
import { Link } from 'react-router';
class NavBar extends React.Component {
  render(){
    return(
      <div>
        <Link to='/' activeStyle={{color:'red'}} activeClassName='cools' onlyActiveOnIndex={true}>home</Link><br/>
        <Link to='/about' activeStyle={{color:'red'}} activeClassName='cools' onlyActiveOnIndex={true}>about</Link><br/>
        <Link to='/work' activeStyle={{color:'red'}} activeClassName='cools'>work</Link><br/>
        <Link to='/about/me' activeStyle={{color:'red'}} activeClassName='cools'>me</Link>
      </div>
    )
  }
}
export default NavBar;
```

- `activeStyle` 和 `activeClassName`

>当前路由被点击处于触发显示状态的时候，我们可以使用activeStyle来给路由设置一些颜色。

```js
//Home.js
import React from 'react';
import NavBar from './NavBar';
class Home extends React.Component {
  render() {
    return (
      <div style={{border:'5px solid #000'}}>
        {this.props.children}
        {this.props.body}
        {this.props.main}
        <NavBar/>
      </div>
    )
  }
}
export default Home;
```

```js
//About.js
import React from 'react';
import NavBar from './NavBar';
class About extends React.Component {
  render() {
    return (
      <div style={{border:'15px solid red'}}>
        about<br/>
        {this.props.children}
        <NavBar/>
      </div>
    )
  }
}
export default About;
```

```js
//Work.js
import React from 'react';
class Work extends React.Component {
  render() {
    return (
      <div style={{border:'10px solid blue'}}>
        work<br/>
      </div>
    )
  }
}
export default Work;
```

```js
//Me.js
import React from 'react';
class Me extends React.Component {
  render() {
    return (
      <div style={{border:'15px solid red'}}>
        me<br/>
        <NavBar/>
      </div>
    )
  }
}
export default Me;
```

```js
//PageNotFound.js
import React from 'react';
class PageNotFound extends React.Component {
  render() {
    return (
      <div style={{fontSize:'50px'}}>
        404
      </div>
    )
  }
}
export default PageNotFound;
```

```js
//IndexRoute.js
import React from 'react';
class IndexRoute extends React.Component {
  render() {
    return (
      <div>
        我是首页
      </div>
    )
  }
}
export default IndexRoute;
```

```js
//Blog.js
import React from 'react';
class Blog extends React.Component {
  render() {
    console.log(this.props);
    return (
      <div style={{border:'10px solid #888'}}>
        你现在访问的是: /blog/{this.props.params.title} <br/>
        {
          this.props.params.title==1 ? <h1>这是第一篇文章</h1> :
          this.props.params.title==2 ? <h1>这是第二篇文章</h1> :
          <h2>你查看的文章不存在</h2>
        }
      </div>
    )
  }
}
export default Blog;
```

```css
*{
  box-sizing: border-box;
}
.cools{
  font-size: 30px;
}
```
