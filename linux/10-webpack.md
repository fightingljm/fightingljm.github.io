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
