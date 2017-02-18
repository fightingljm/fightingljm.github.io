---
title:  react 安装及简单使用
---

### 安装

首先需要webpack和node环境

```
npm install react react-dom --save
npm install --save-dev babel-preset-react
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

* * *

***

*****

- - -

---------------------------------------

### JSX 语法

它允许我们在JS里直接去写标签,他的特点如下

- 每个标签必须关闭;
- Adjacent JSX elements must be wrapped in an enclosing tag(JSX元素必需被包裹在一个完全封闭的标签内);
- 注释的写法: {/*我是注释*/}
- 我们可以在JSX语法内嵌入变量{obj},但是不能嵌入js语句;不支持if语句,但是支持三目运算符
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
    <h1>{name+'_^'}</h1>
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

- 方法二:

 ```js
 function Greeting() {
   return (<h1>Hello</h1>)
 }
 ReactDOM.render(
   <Greeting></Greeting> ,document.getElementById('app')
 )
 ```
