---
title: 连通 API
---

前面的应用看起来功能完整了，但是实际的数据都没有被保存到数据库（数据持久化没有做）。

### 引入 `redux-thunk`

thunk 的意思是“模式转换”，有了 redux-thunk ，redux 的工作模式就会发生一定的“转换”，具体来讲就是[官网](https://github.com/gaearon/redux-thunk)上说的

> Redux Thunk middleware allows you to write action creators that return a function instead of an action

意思是：Redux-thunk 中间件允许我们的 action creator 不直接返回 action ，而是去返回一个函数。（这样做的目的，就是为了实现一异步操作）。

Redux-thunk 解决的问题是：**异步操作** 。没有 thunk 之前，一旦用户点按钮，aciton 立即被发出，reducer 立即执行，store 立即改变。这个是 redux 默认工作机制。但是这种机制在很多情况下是不能满足要求的，例如，用户点按钮之后，action 中要携带的数据需要从网络上读取的这种情况。有了 thunk 之后，即使要等很长时间也没有问题，因为 diptach 操作可以等待网络请求结束之后再去执行。

先来装包

```
$ npm install --save redux-thunk
```

这里用到的版本是 --- "redux-thunk": "^2.2.0"

到 store.js 中，

```js
import { createStore, applyMiddleware, compose } from 'redux';
```

- `applyMiddleware`: 使用中间件
- `compose`：构建

下面的代码是让我们的 redux 加载 thunk 中间件：

- [load thunk](https://github.com/fightingljm/redux-demo/commit/60bb1d8119a6777d8949f9192147f8999c2987a8)

加载 thunk 之后，我们 dispatch（分发）action 的方式就有了很大变化。对 reducer 影响不大。

下面我们来创建 actions/commentActions.js 文件，里面会放很多函数，这些函数本身不是 action ，但是它们的返回值（或者说就是最终运算的结果是 action )，我们把这一类的函数叫 Action Creator （ Action 创建器）。

> 注：没有安装 thunk 之前，也是可以创建 Action Creator ，但是使用形式差别很大。但是有了 thunk 之后，Action Creator 的功能就变得很强大了。

下面的代码，使用了 redux-thunk ，虽然没有用它来完成任何异步操作：

- [thunk works](https://github.com/fightingljm/redux-demo/commit/041be5d5e00d875e2dc6d92f9f1747b62e3bcfea)

### 后台 API

- [API 文档](https://coding.net/u/happypeter/p/redux-hello-api/git)

### 实现 axios 异步操作

- [thunk aync works](https://github.com/fightingljm/redux-demo/commit/ee0191c8248c74d83d96e365f050f7d90c5decad)

*如何从服务器加载数据到 redux 中*

- [参考](https://github.com/happypeter/hand-in-hand-react-demo/blob/master/client/complete/src/ui/DashBoard.js#L11)
