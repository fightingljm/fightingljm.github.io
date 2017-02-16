---
title: 初试 webpack
---

webpack ES6语法编译成ES5

```
ls

执行一条webpack命令
./node_modules/.bin/webpack index.js build/ljm.js

以上 index.js是ES6语法 ；build/ljm.js是ES5语法
```

ES6多个js文件打捆导入导出

```
打捆，执行一条webpack命令
./node_modules/.bin/webpack index.js build/ljm.js

导出
module.exports = a;

导入
var a = require('./demo');
```
