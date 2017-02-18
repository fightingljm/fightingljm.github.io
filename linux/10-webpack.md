---
title: 初试 webpack 和 Nodejs语法
---

### nodejs 模块

nodejs 的模块分为 3 类，核心模块，第三方模块，以及自定义的模块

```
- 通过 module.exports 导出模块，require 导入模块。

1 导入核心模块
var fs = require('fs')
2 导入第三方模块
var $ = require('jquery')
3 导入自定义的模块
var test = require('./test')

1 导出模块
module.exports.test = 'test';
```

### webpack 打包

我们的前端开发会用到 es6 的模块，需要用 webpack 来打包我们的代码。

```
1 创建项目文件夹
mkdir webpack-demo && cd webpack-demo
2 初始化项目
npm init -y
3 安装 webpack
npm i -D webpack
4 编写我们的 npm 脚本，使用项目内的 webpack
"scripts": {
  "build": "./node_modules/.bin/webpack"
}
5 增加 webpack 的配置文件 webpack.config.js
module.exports = {
  entry: './index.js',
  output: {
    filename: 'bundle.js'
  }
}
```

### Nodejs语法

多个js文件的打捆导入导出

```
打捆，执行一条webpack命令
./node_modules/.bin/webpack index.js build/bundel.js -p --watch -d --progress
```

- ./node_modules/.bin/webpack  ：webpack的命令，找到webpack工具
- index.js  ：入口文件，即需要被打包的文件
- build/bundel.js  ：出口文件，即打包之后生成的文件
- -p  ：参数,压缩代码
- --watch  ：参数,自动监听
- -d  ：参数,对源代码有定位功能，报错到源代码，方便错误查找
- --progress  ：参数,显示构建进度
- --display-error-details ：参数，这个很有用，显示打包过程中的出错信息

刚才我们看到，在运行webpack命令的时候，后面带了一串参数，这里我们可以统一在webpack.config.js文件里面维护

```js
/* webpack.config.js */
module.exports = {
  entry:'./index.js',
  output:{
    path:'build',
    filename:'bundel.js'
  },
  devtool:'eval'
}
```

我们在配置中新增了devtool字段，

```
导出
module.exports = a;

导入
var a = require('./demo');
```

### webpack 功能简介

Webpack 是一个专门为摩登 JS 应用开发而生的`模块打捆器`（ Module Bundler ）。安装 Webpack 就用 `npm install -g webpack` 就可以。 理解 Webpack 的功能，首先要理解它的四大概念。

- 概念一：**Entry** 入口文件和 概念二：**Output** 出口文件

Webpack 既然是个打捆器，也就是说可以把很多 JS 文件打捆成一个 JS 文件，了解打捆，主要涉及到 Entry 和 Output 两个概念。

Entry 文件就是整个项目的入口文件，所有其他的 JS 模块文件都作为这个文件的儿孙导入进来(这里可以使用 ES6 模块格式，或者其他几种常用模块格式也可以)。相对于 Output 出口文件，Entry 入口文件是整个 Webpack 打包过程的被操作对象，出口文件 Output 用来指明出口文件的名字，习惯上叫做 bundle.js 。

webpack.config.js 中会写成这样：

```js
module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'
  }
};
```

- 概念三：**Loader** 加载器

Webpack 默认只认纯粹的 JS 文件，那对于 ES6 文件，React 文件，CSS 文件，等等其他格式，Webpack 就无能为力了吗？NO，Webpack 是超级灵活的，可以通过添加各种 loader 来把其他格式的文件，先翻译成 JS ，然后进行打包处理。

例如：

webpack.config.js

```js
const config = {
  ...
  module: {
    rules: [
      {test: /\.(js|jsx)$/, use: 'babel-loader'}
    ]
  }
};
```

- 概念四：**Plugins** 插件

Webpack 还可以通过添加各种 Plugins 来丰富自己的功能。其他开发环境下能做的一些操作，例如文件压缩，使用 html 模板，等等各种功能，Webpack 这里也一样可以做到，只需要添加合适的插件进来即可。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm

const config = {
  ...
  plugins: [
    new webpack.optimize.UglifyJsPlugin(), // webpack 内置有一些 plugin
    new HtmlWebpackPlugin({template: './src/index.html'}) // 另外一些可以通过装包实现
  ]
};

module.exports = config;
```

**Webpack** 有何独特优势？

这里只说两点：

第一，Webpack 的 Code Splitting 代码分割功能。

第二，Webpack 是 React 开发的事实标准。做 React 开发的人，大家基本都用 Webpack 。

### Webpack-Babel 编译 ES6

ES6 的各种语法浏览器并不是完全支持，所以如果我们要在项目中使用 ES6 还是需要编译的。那这个编译器就是 `Babel` 。虽然单独使用 Babel 也可以编译 ES6 ，但是本节中我们用 Webpack 中添加 Babel Loader 的形式来进行 ES6 的编译。案例代码会非常简单，作为我们实际使用 Webpack 的开场。

**装包**

首先新建一个项目，并通过 npm init 把它初始化为一个 nodejs 项目

```
mkdir webpack-es6
cd webpack-es6
npm init -y
```

最后一个命令执行完毕，package.json 文件就生成了，可以开始装包了

```
npm i --save babel-loader babel-core babel-preset-es2015 babel-preset-stage-0
```

上面

添加 .babelrc 文件：

```
{
  "presets": ["es2015", "stage-0"]
}
```

上面，安装 babel-preset-es2015 这个包以及在 .babelrc 中写入 `es2015` 使得 Babel 启动了 ES6( 也就是 ES2015 )的编译支持。但是 stage-0 （ ES7 的第0阶段提案功能）的包和 .babelrc 中的设置也是必要的，不然实际使用中也会出现某些语法编译不过去的错误。
stage-0 也需要专门来安装一个包，即上面的 `npm i --save babel-preset-stage-0`

> 注意：我的系统上 webpack 已经用 `npm i -g webpack` 全局安装过了

接下来写 webpack.config.js 文件

```js
var path = require('path');

module.exports = {
  entry: path.resolve(__dirname, 'src/index.js'),
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel'
      }
    ]
  }
};
```

`__dirname` 指当前文件

要被编译的 src/index.js 文件如下：

```js
class Bar {
  doStuff() {
    console.log('stuff');
  }
}

var b = new Bar();
b.doStuff();
```

为了展示浏览器中的运行效果，来添加一个 index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <script src="./dist/bundle.js"></script>
</body>
</html>
```

**编译执行**

现在就来运行命令进行编译

```
cd webpack-es6
webpack
```

执行 webpack 命令的时候，默认就会去加载当前位置的 webpack.config.js 中的配置内容， 于是 Webpack 就可以找到配置文件中指明的 Entry （入口文件），通过 babel-loader 来把入口文件 中的 ES6 的内容编译成 ES5 ，并且输出到出口文件 Output ，也就是 dist 文件夹之内的 bundle.js 文件。

要看执行效果，就用 Chrome 浏览器打开 index.html 文件，同时打开 chrome 开发者工具，这样就可以终端中 输出了

```
stuff
```

证明代码执行成功了。

也可以手动打开 dist/bundle.js 看一下，就会发现本来写到 index.js 中的 ES6 语法，现在都已经被编译成 了 ES5 语法了。

### Webpack 打捆 ES6 模块

Webpack 可以支持的模块格式不局限于 **ES6** 模块 ，但是由于我们写 React 项目主要用 ES6 来写，所以这一集就来演示一下用 Webpack 打捆 ES6 模块。

模块的命名导出和默认导出

我们这一集的重点并不是讲解模块的基础知识。但是还是顺便提一下 ES6 模块可以支持命名导出和默认导出两种形式。

src/index.js 如下：

```js
import  i from './a';
import { j, k } from './a';

console.log(i);
console.log(j);
console.log(k);
```

src/a.js 如下：

```js
const i = 1;
const j = 2;
const k = 3;

export { j, k };//命名导出

export default i;//默认导出
```

webpack.config.js 跟上集中一样。

**编译和运行**

执行 webpack 命令，可以把 index.js 和 a.js 打捆成一个 bundle.js 文件，浏览器中运行，chrome console 中会打印出

```
1
2
3
```

运行成功了。

### Webpack 编译 React

使用 Webpack-Babel 环境编译 React 项目，因为 Babel 本来就是有能力编译 React 的，所以配置稍微改改，编译就能成功了。

写一个简单的 React 组件

先来装包：

npm install --save react react-dom
上面 react 这个包用来提供 React 的核心功能，react-dom 来帮助我们把 React 组件显示（术语也叫”渲染“ 英文叫 render ）到浏览器之上。

把上一集的 a.js 改名为 App.js（ React 的组件文件名字要大写），内容改成下面这些：

import React, { Component } from 'react';

class App extends Component {
  render(){
    return(
      <div>
        App
      </div>
    )
  }
}

export default App;
然后 index.js 改为

import App from './App';
import ReactDOM from 'react-dom';
import React from 'react';


ReactDOM.render(<App />, document.getElementById('app'));
编译一下

运行

cd webpack-es6
webpack
进行项目编译，但是报错了：

ERROR in ./src/index.js
Module build failed: SyntaxError: Unexpected token (5:16)

```
  3 |
  4 |
> 5 | ReactDOM.render(<App />, document.getElementById('app'));
    |                 ^
  6 |
```

上面 Unexpected token 意思是”未识别的符号“，你会说奇怪呀下面箭头指向的 <App> 没问题呀， 就是 React 的标准写法呀？对，代码没问题，但是编译都通过不了，那肯定是编译环境有问题呗。

安装

到 Babel 的官网，首页上就能看到除了能编译 ES6 ，Babel 还可以编译 JSX ，也就是可以编译 React 的，但是跟编译 ES6 一样，编译 React 也是要设置 Preset 的。

第一步，装包

npm i --save babel-preset-react
第二步，修改 .babelrc 文件，为

{
  "presets": ["es2015", "stage-0", "react"]
}
使用 server

Download the React DevTools and use an HTTP server (instead of a file: URL) for a better development experience: https://fb.me/react-devtools
下一集来介绍 webpack-dev-server
