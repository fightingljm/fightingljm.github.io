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
