---
title: ES6 简介及快速入门
---

ECMAScript简称就是ES,你可以把它看成是一套标准,JavaScript就是实施了这套标准的一门语言 现在主流浏览器使用的是ECMAScript5。

ECMAScript 6.0（以下简称ES6）是JavaScript语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。在项目中80%的时间用到的ES6语法只占其20%，所以我们暂时先集中精力把这20%学好，那就差不多够用了，剩下的可以看书或是查文档，现学现用。

### Let + Const 块级作用域和常量

let和const的出现让 JS 有了块级作用域，还可以像强类型语言一样定义常量。由于之前没有块级作用域以及 var 关键字所带来的变量提升，经常给我们的开发带来一些莫名其妙的问题。

下面看两个简单的demo理解。

```js
// demo 1
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}

// demo 2
const PI = 3.1415;
console.log(PI); // 3.1415

PI = 3;
console.log(PI); // TypeError: "PI" is read-only

if (true) {
  var a = "a"; // 期望a是某一个值
}
console.log(a);
if(true){
  let name = 'zfpx';
}
console.log(name);// ReferenceError: name is not defined

// 嵌套循环不会相互影响
for (let i = 0; i < 3; i++) {
  console.log("out", i);
  for (let i = 0; i < 2; i++) {
    console.log("in", i);
  }
}
//结果 out 0 in 0 in 1 out 1 in 0 in 1 out 2 in 0 in 1

//重复定义会报错
if(true){
  let a = 1;
  let a = 2; //Identifier 'a' has already been declared
}

//不存在变量的提升
console.log(i)
let i=10;
//结果 i is not defined

// 闭包新写法
// 以前
;(function () {

})();

//现在
{
}
```

### 解构赋值

解构意思就是分解一个东西的结构,可以用一种类似数组的方式定义N个变量，可以将一个数组中的值按照规则赋值过去。

```js
var [name,age] = ['zfpx',8];
console.log(name,age); // zfpx 8

var [x,y]=getVal(),//函数返回值的解构
    [name,,age]=['zf','male','secrect'];//数组解构

function getVal() {
    return [ 1, 2 ];
}

console.log('x:'+x+', y:'+y);//输出：x:1, y:2
console.log('name:'+name+', age:'+age);//输出： name:zf, age:secrect
```

数组、对象和字符串的解构赋值示例：

```js
'use strict';
// 数组的解构赋值
let [foo, [[bar], baz]] = [1, [[2], 3]];
console.log(foo); // 1
console.log(bar); // 2
console.log(baz); // 3

// 对象的解构赋值
var { foo, bar } = { foo: "aaa", bar: "bbb" };
console.log(foo);   // "aaa"
console.log(bar );  // "bbb"

// 字符串的解构赋值
const [a, b, c, d, e] = 'hello';
console.log(a + b + c + e); // 'hello'
```

### Arrows 箭头函数

- 箭头函数简化了函数的的定义方式，一般以 "=>" 操作符左边为输入的参数，而右边则是进行的操作以及返回的值Inputs=>outputs。
- 箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this，所以也就不能用作构造函数，从而避免了this指向的问题。

返回值为一个对象时要用小括号括起来;
多条语句时用大括号括起来,返回值写return,返回值为一个对象时,如果这个对象的对象名和参数名一样时写一个就可以;

请看下面的例子。

```js
//demo 1
//以前
let [a,b]=[1,2];
function add(a,b) {
  console.log(a+b);
}
add(a,b);
//现在
let add = (a,b) => console.log(a+b);


//demo 2
var a =(name,age)=>({name,age:age+8})
console.log(a('ljm',10));

var b =(name,age)=>{
  console.log('1');
  alert('2');
  return{name,age:age+8}
}
console.log(b('ljm',10));
```

### Template Strings 字符串模板

字符串模板相对简单易懂些。ES6中允许使用反引号来创建字符串，此种方法创建的字符串里面可以包含由美元符号加花括号包裹的变量${vraible}。如果你使用过像C#等后端强类型语言的话，对此功能应该不会陌生。

```js
//demo1
//产生一个随机数
var num = Math.random();
//将这个数字输出到console
console.log(`your num is ${num}`);


//demo2
let name = 'guoyongfeng';
let age = 18;
console.log(`${name} was ${age}`)


//demo3
let name = 'ljm';
let age = 18;
console.log("姓名是"+name+"的年龄是"+age+"岁");
console.log(`姓名是${name}的年龄是${age}岁`);
```

### Default + Rest + Spread

**函数的扩展**

```js
let a=3;
let obj={
  a,
  say(){
    console.log('我是es6允许的语法');
  }
}
console.log(obj);
```

**Default 默认参数值**

现在可以在定义函数的时候指定参数的默认值了，而不用像以前那样通过逻辑或操作符来达到目的了。

```js
function sayHello(name){
    //传统的指定默认参数的方式
    var name=name||'dude';
    console.log('Hello '+name);
}
//运用ES6的默认参数
function sayHello2(name='dude'){
    console.log(`Hello ${name}`);
}
sayHello();//输出：Hello dude
sayHello('zf');//输出：Hello zf
sayHello2();//输出：Hello dude
sayHello2('zf');//输出：Hello zf
```

**Rest 剩余参数**
不定参数是在函数中使用命名参数同时接收不定数量的未命名参数。这只是一种语法糖，在以前的JavaScript代码中我们可以通过 `arguments` 变量来达到这一目的。

不定参数的格式是三个句点后跟代表所有不定参数的变量名。比如下面这个例子中，…x代表了所有传入add函数的参数。

一个简单示例：

```js
// rest
function restFunc(a, ...rest) {
  console.log(a)
  console.log(rest)
}
restFunc(1);
restFunc(1, 2, 3, 4);
```

- `reduce()` 数组的方法
  官方概述：reduce() 方法接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始缩减，最终为一个值。
  其实reduce接收的就是一个回调函数，去调用数组里的每一项，直到数组结束。

再看一个：

```js
//将所有参数相加的函数
function add(...x){
    return x.reduce((m,n)=>m+n);
}
//传递任意个数的参数
console.log(add(1,2,3));//输出：6
console.log(add(1,2,3,4,5));//输出：15
```

以上回顾了数组的reduce()方法,接下来再回忆几个.

- `map()` 方法返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组;

数组的map()方法,遍历数组,有返回值

```js
let arr = [4,5,6];
let newArr = arr.map(function (currentValue, index, array) {
  console.log(currentValue, index, array);
  return currentValue+10;
})
console.log(newArr);
```

- `forEach()` 方法对数组的每个元素执行一次提供的函数,没有返回值

```js
//demo1
var numbers = [1, 2, 3, 4];
numbers.forEach(function(item, index, array) {
  console.log(item + "\t" + index + "\t" + array);
});
var array = [1, 2, 3];
//传统写法
array.forEach(function(v, i, a) {
    console.log(v);
});
//ES6
array.forEach(v = > console.log(v));
// 箭头函数,输入参数如果多于一个要用()包起来，函数体如果有多条语句需要用{}包起来

//demo2
let arr = [4,5,6];
let newArr = arr.forEach(function (currentValue, index, array) {
   console.log(currentValue, index, array);
 })
 console.log(newArr);//undefined   因为forEach方法没有返回值

//demo3
let arr = [4,5,6];
arr.forEach(item=>console.log(item+10))
```

- `filter()` 过滤

```js
let arr = [4,5,9];
let results = arr.filter(function (item, index, array) {
  console.log(item, index, array);
  return item>8;
})
//var results = arr.filter(item=>item>8)//简写成箭头函数
console.log('results======',results);
```

**Spread 扩展操作符**

- 官方说法:扩展操作符则是另一种形式的语法糖，它允许传递数组或者类数组直接做为函数的参数而不用通过apply。
- 扩展运算符（spread）是三个点（...）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列;

```js
var people=['zf','John','Sherlock'];

function sayHello(people1,people2,people3){
    console.log(`Hello ${people1},${people2},${people3}`);
}
//但是我们将一个数组以拓展参数的形式传递，它能很好地映射到每个单独的参数
sayHello(...people);//输出：Hello zf,John,Sherlock

//而在以前，如果需要传递数组当参数，我们需要使用函数的apply方法
sayHello.apply(null,people);//输出：Hello zf,John,Sherlock
```

### Class, extends, super 类的支持

回想之前，如果我们需要模拟一个js的类，一般会采用构造函数加原型的方式。

```js
//构造函数的简单继承,通过构造函数，定义并生成新对象
function Point(x,y){
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
}//原型,函数有prototype属性
var point = new Point(6,3);//point为Point的实例化,继承了Point的所有属性和方法
console.log(point.toString());//(6,3)

//原型链
function Point(x,y) {
  this.x = x;
  this.y = y;
}
Point.prototype.toString = function () {
  return `(${this.x},${this.y})`;
};
function Hello() {
  this.toString = function () {
    return 'hello say'
  }
}
Hello.prototype = new Point(6,8)
var p = new Hello();//这里p继承了Point和Hello的所有属性和方法
console.log(p.x);
console.log(p.y);
console.log(p.toString());//hello say
//先从Hello里找,找不到再从Hello的原型找,找不到再从p找,找不到再从p的原型找
```

ES6中添加了对类的支持，引入了class关键字（其实class在JavaScript中一直是保留字，目的就是考虑到可能在以后的新版本中会用到，现在终于派上用场了）。

JS本身就是面向对象的，ES6中提供的类实际上只是JS原型模式的包装。现在提供原生的class支持后，对象的创建，继承更加直观了，并且父类方法的调用，实例化，静态方法和构造函数等概念都更加形象化。

下面代码展示了类在ES6中的使用。

```js
//类的定义
class Animal {
    //ES6中新型构造器
    constructor(name) {
        this.name = name;
    }
    //实例方法
    sayName() {
        console.log('My name is '+this.name);
    }
}
//类的继承
class Programmer extends Animal {
  constructor(name) {
      //直接调用父类构造器进行初始化
    super(name);
  }
  program() {
    console.log("I'm coding...");
  }
}

//测试我们的类
var animal=new Animal('dummy'),
zf=new Programmer('zf');

animal.sayName();//输出 ‘My name is dummy’
zf.sayName();//输出 ‘My name is zf’
zf.program();//输出 ‘I'm coding...’
//定义“类”的方法的时候，前面不需要加上function这个关键字，直接把函数定义放进去了就可以了。另外，方法之间不需要逗号分隔，加了会报错。
```

再看一个例子:

```js
//class类,类里边只能写一个一个的方法,不能有单独的语句
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    console.log('aaa');
  }
  //constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。
  //一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。
  //类里边只能去写一个一个的方法,方法与方法之间不能用逗号。
  //一般方法都写在constructor外边,不然函数会被自动执行;
  //如上,被实例化的Point不被调用也会执行'console.log('aaa');'语句
  toString() {
    return `(${this.x},${this.y})`
  }
}
var p = new Point(6,8);
console.log(p.x,p.y);
console.log(p.toString());

//继承
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    console.log('aaa');
  }
  toString() {
    return `(${this.x},${this.y})`
    // return 'Point toString'
  }
}
class Hello extends Point {
  constructor(x,y,c){
    super(x,y);
    this.c=c;
  }
  say(){
    return this.x;
  }
}
var p = new Hello(1,2,3);
console.log(p.toString());
console.log(p.say());
console.log(p.c);
```

看一个关于继承的问题:

```js
//继承问题
class Father {
  render() {
    throw new Error('子类必须实现')
  }
  _rander() {
    return(`
      <ul>
        ${this.rander()}
      </ul>
    `)//this指向实例
    //选中this一堆,ctrl alt+w,输入ul,this一部分将被ul标签包裹
  }
}
class Son extends Father {
  rander() {
    return(`
      <li>1</li>
      <li>2</li>
      <li>3</li>
    `)//外边的()只是起到包裹代码块儿的作用
  }
}
document.getElementById('app').innerHTML = new Son()._rander();
//调用Son的_rander方法时找到了继承的Father的_rander方法,这个方法返回的this指向实例,也就是指向Son里的rander方法;
//如果Son里没有rander方法,则会找到Father的rander方法,抛出错误.
```

### Modules 模块

在ES6标准中，JavaScript原生支持module了。这种将JS代码分割成不同功能的小块进行模块化的概念是在一些三方规范中流行起来的，比如CommonJS和AMD模式。

将不同功能的代码分别写在不同文件中，各模块只需导出公共接口部分，然后通过模块的导入的方式可以在其他地方使用。

不过，还是有很多细节的地方需要注意，我们看例子：

简单使用方式：

```js
// point.js
export default class Point {
   constructor (x, y) {
     public x = x;
     public y = y;
   }
 }


// myapp.js
//这里可以看出，尽管声明了引用的模块，还是可以通过指定需要的部分进行导入
import Point from "point";

var origin = new Point(0, 0);
console.log(origin);
```

```js
//demo.js
let a = 10;
let b = 20;
function aa(x) {
  console.log('aaaa');
  return x*8;
}
class Father {
  render() {
    throw new Error('子类必须实现')
  }
  _rander() {
    return(`
      <ul>
        ${this.rander()}
      </ul>
    `)//this指向实例
    //快捷键:选中this一堆,ctrl alt+w,输入ul,this一部分将被ul标签包裹
  }
}
export {a,aa,Father};//命名导出
export default b;//默认导出



//index.js
import {a,aa,Father} from './demo';
import demo from './demo';
console.log(a);
console.log(aa(6));
console.log(demo);
class Son extends Father {
  rander() {
    return(`
      <li>1</li>
      <li>2</li>
      <li>3</li>
    `)//外边的()起到包裹代码块儿的作用
  }
}
document.getElementById('app').innerHTML = new Son()._rander();
```

*export*

```js
// demo1：简单使用
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

// 等价于
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```

```js
// demo2：还可以这样
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

```js
// demo3：需要注意的是export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

// 报错
function f() {}
export f;

// 正确
export function f() {};
```

我们再来看一下export的默认输出：

```js
export default function () {
  console.log('foo');
}
```

为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到export default命令，为模块指定默认输出。这样其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。

需要注意的是，这时import命令后面，不使用大括号。

最后需要强调的是：ES6模块加载的机制，与CommonJS模块完全不同。CommonJS模块输出的是一个值的拷贝，而ES6模块输出的是值的引用。

- CommonJS模块输出的是被输出值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。

- ES6模块的运行机制与CommonJS不一样，它遇到模块加载命令import时，不会去执行模块，而是只生成一个动态的只读引用。等到真的需要用到时，再到模块里面去取值，换句话说，ES6的输入有点像Unix系统的”符号连接“，原始值变了，import输入的值也会跟着变。因此，ES6模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

*import*

```js
// 1
import $ from 'jquery';

// 2
import {firstName, lastName, year} from './profile';

// 3
import React, { Component, PropTypes } from 'react';

// 4
import * as React from 'react';
```
