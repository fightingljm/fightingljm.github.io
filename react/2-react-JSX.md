---
title:  JSX 语法
---

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
