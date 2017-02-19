---
title:  react 安装及简单使用
---

### 安装

首先需要webpack和node环境

```
$ npm install react react-dom --save
$ npm install --save-dev babel-preset-react
```

### 使用

修改`.babelrc`配置文件为

```
{
  "presets": ["env","react"]
}
```

在 `index.js` 里编写简单的 react ,首先导入

```js
import React from 'react';
import ReactDOM from 'react-dom';
```

### React 的最基本方法

`ReactDOM.render` 是 React 的最基本方法,用于将模板转为 HTML 语言,并插入指定的 DOM 节点;可以执行下面代码查看

```js
console.log(ReactDOM.render)
```

结果为

```js
function (nextElement, container, callback) {
    return ReactMount._renderSubtreeIntoContainer(null, nextElement, container, callback);
  }
```

### JSX 语法

它允许我们在JS里直接去写标签,他的特点如下

- 每个标签必须关闭;
- Adjacent JSX elements must be wrapped in an enclosing tag(JSX元素必需被包裹在一个完全封闭的标签内);
- 注释的写法: {/*我是注释*/}
- 我们可以在JSX语法内嵌入变量{obj},但是不能嵌入js语句,比如 `console.log();`会报错 ;不支持if语句,但是支持三目运算符
- class 写为 className , tabindex 写为 tabIndex , for 写为 htmlFor
- JSX语法会被编译,通过React.createElement()方法

例:

```js
let name = 'ljm';
let age = 4;
let male = 1;
let obj = {name:'ljm'};
function add() {
  return 6+8;
}
let a =
  <div>
    <h1>hello react</h1><br/>
    {/*我是注释*/}
    <h1>{name+'^_^'}</h1>
    <h1>年龄:{age*5}</h1>
    <h1 className={male ? 'aaa' : 'bbb'}>性别:{male ? '男' : '女'}</h1>
    {/*<style>
    .aaa{color: #ccc;}
    .bbb{color: pink;}
    </style>*/}
    {/*在index.html里添加以上style的样式,当性别为男时这一行会显示灰色,性别为女时会显示粉色*/}
    <h1>{obj.name}</h1>
    <h1>{add()}</h1>
  </div>;
//以上注释的形式是JSX语法
ReactDOM.render(
  a ,document.getElementById('app')
)
```

### component 组件(3种方式)

  `首字母要大写`

- 方法一: var Hello = React.createClass({})  es5的写法,过时不用

 ```js
console.log(React);//可以打印出React下的方法,其中有一个createClass以下会用到
 let Greeting = React.createClass({
   render: function () {
     return <h1>Hello</h1>
   }
 })
 ReactDOM.render(
   <Greeting></Greeting> ,document.getElementById('app')//自定义的标签不会被渲染,会去找返回值
 )
 ```

- 方法二:返回一段Dom结构的组合,也不太常用

 ```js
 function Greeting() {
   return (<h1>Hello</h1>)
 }
 ReactDOM.render(
   <Greeting></Greeting> ,document.getElementById('app')
 )
 ```

 - 方法三:

 ```js
 class Hello extends React.Component {
  render(){
    return(
      <div>
         <h1>hello react</h1><br/>
         <h2>略略略</h2>
      </div>
    )
  }
}
ReactDOM.render(
  <Hello></Hello> ,document.getElementById('app')
)
```

也可以用导入导出的方式:

```js
//Header.js
import React from 'react';
class Header extends React.Component {
  render(){
    return(
      <div>
         我是头部
      </div>
    )
  }
}
export default Header;

//index.js
import React from 'react';
import ReactDOM from 'react-dom';

import Header from './Header'
ReactDOM.render(
  <Header></Header> ,document.getElementById('app')
)
```

**组件化嵌入式开发**

用组件化实现一个网页的头部 Banner 和 Footer:

```js
//Header.js
import React from 'react';
class Header extends React.Component {
  render(){
    return(
      <div>
         我是头部
      </div>
    )
  }
}
export default Header;

//Banner.js
import React from 'react';
class Banner extends React.Component {
  render(){
    return(
      <div>
         我是Banner
      </div>
    )
  }
}
export default Banner;

//Footer.js
import React from 'react';
class Footer extends React.Component {
  render(){
    return(
      <div>
         我是Footer
      </div>
    )
  }
}
export default Footer;

//index.js
import React from 'react';
import ReactDOM from 'react-dom';

import Header from './Header';
import Banner from './Banner';
import Footer from './Footer';
ReactDOM.render(
  <div>
    <Header/>
    <Banner/>
    <Footer/>
  </div>
  ,document.getElementById('app')
)
```

简化出口: Header.js Banner.js Footer.js,都没有变化,新建一个App.js,让所有文件都从这一个出口出去

```js
//新建 App.js
import React from 'react';

import Header from './Header';
import Banner from './Banner';
import Footer from './Footer';
class App extends React.Component {
  render(){
    return(
      <div>
        <Header/>
        <Banner/>
        <Footer/>
      </div>
    )
  }
}
export default App;


//修改 index.js 为
import React from 'react';
import ReactDOM from 'react-dom';

import App from './App';
ReactDOM.render(
  <App/>,document.getElementById('app')
)
```

在头部假定一个 logo 一个登录注册按钮

```js
//新建 Logo.js
import React from 'react';

class Logo extends React.Component {
  render(){
    return(
      <div>
           <img src="https://fightingljm.github.io/images/book-cover.jpg" alt=""/>
      </div>
    )
  }
}
export default Logo;


//新建 Signin.js
import React from 'react';

class Signin extends React.Component {
  render(){
    return(
      <div>
        <input type="button" value="登录"/>
        <input type="button" value="注册"/>
      </div>
    )
  }
}
export default Signin;


//修改 Header.js 为
import React from 'react';

import Logo from './Logo';
import Signin from './Signin';
class Header extends React.Component {
  render(){
    return(
      <div>
        <Logo/>
        <Signin/>
         我是头部
      </div>
    )
  }
}
export default Header;
```

**添加CSS样式**

方法一:

```js
//例如 Logo.js
import React from 'react';
class Logo extends React.Component {
  render(){
    return(
      <div>
           <img src="https://fightingljm.github.io/images/book-cover.jpg" alt="" style={{width:'100px',border:'2px solid #ccc',borderRadius:'50%'}}/>
      </div>
    )
  }
}
export default Logo;

或者另一种方式
import React from 'react';
class Logo extends React.Component {
  render(){
    let style = {width:'100px',border:'2px solid #ccc',borderRadius:'50%'}
    return(
      <div>
           <img src="https://fightingljm.github.io/images/book-cover.jpg" alt="" style={style}/>
      </div>
    )
  }
}
export default Logo;
```

方法二:
```js
//例如 Signin.js
import React from 'react';
class Signin extends React.Component {
  getStyle(){
    return{
      float : 'left'
    }
  }
  render(){
    let color = 0;
    let styles = {
      leftBtn : {
        background:color ? 'red' : 'yellow'
      },
      rightBtn : {
        background:'blue'
      }
    }
    return(
      <div style={this.getStyle()}>
        <input style={styles.leftBtn} type="button" value="登录"/>
        <input style={styles.rightBtn} type="button" value="注册"/>
      </div>
    )
  }
}
export default Signin;
```

方法三:

**外链样式**

首先要装包

```
$ npm install --save-dev style-loader css-loader
```

没有包会报错:You may need an appropriate loader to handle this file type.

然后修改 webpack.config.js

```js
// webpack.config.js
module.exports = {
  entry:'./src/index.js',
  output:{
    path:'build',
    filename:'bundel.js'
  },
  devtool:'eval',//报错到源代码
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,//排除node_modules文件夹
        use: "babel-loader"
      },
+     {
+       test: /\.css$/,
+       use: ['style-loader','css-loader']
+     }
    ]
  }
}
```

接下来就可以演示一个简单的导出
先看 main.css
```css
/*main.css*/
*{
  margin: 0;
  padding: 0;
}
body{
  background-color: #ccc;
}
```
再看导入到 index.js 时,直接导入文件就可以

```js
  import React from 'react';
  import ReactDOM from 'react-dom';

  import App from './App';
+ import './main.css'  //没有装包会报错:You may need an appropriate loader to handle this file type.
  ReactDOM.render(
    <App/> ,document.getElementById('app')
  )
```
这样就完成了css文件的外链



**本地图片加载**

同样,首先装包
```
$ npm install --save-dev file-loader
```
修改webpack.config.js
```js
module.exports = {
  entry:'./src/index.js',
  output:{
    path:'build',
    filename:'bundel.js',
+    publicPath:'build/'
  },
  devtool:'eval',//报错到源代码
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,//排除node_modules文件夹
        use: "babel-loader"
      },//js文件用babel加载
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      },//css文件的加载,解析文件样式
+     {
+       test: /\.(jpe?g|png)$/,
+       use: 'file-loader'
+     }//图片加载
    ]
  }
}
```
### resolve解析

resolve下常用的是extension和alias字段的配置：

extension 不用在require或是import的时候加文件后缀
```js
// webpack.config.js
module.exports = {
  entry:'./src/index.js',
  output:{
    path:'build',
    filename:'bundel.js',
    publicPath:'build/'
  },
  devtool:'eval',//报错到源代码
+ resolve: {
+   extensions: [".js", ".jsx",".css",".jpg"],
+ },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,//排除node_modules文件夹
        use: "babel-loader"
      },//js文件用babel加载
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      },//css文件的加载,解析文件样式
      {
        test: /\.(jpe?g|png)$/,
        use: 'file-loader'
      }//图片加载
    ]
  }
}
```
```js
- var component = require('./component.js');
+ var component = require('./component');
```
