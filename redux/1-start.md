---
title: Redux 简介及初步 React-webpack 环境搭建
---

### Redux 简介

Redux 的创造者 Dan Abramov 说过一句话

```
"只有遇到 React 实在解决不了的问题，你才需要 Redux 。"
```

简单说，如果你的UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。

下面这些情况，都`不需要`使用 Redux。

- 用户的使用方式非常简单
- 用户之间没有协作
- 不需要与服务器大量交互，也没有使用 WebSocket
- 视图层（View）只从单一来源获取数据

下面这些情况才是 `Redux 的适用场景：多交互、多数据源`。

- 用户的使用方式复杂
- 不同身份的用户有不同的使用方式（比如普通用户和管理员）
- 多个用户之间可以协作
- 与服务器大量交互，或者使用了WebSocket
- View要从多个来源获取数据

从组件角度看，如果你的应用有以下场景，可以考虑使用 Redux。

- 某个组件的状态，需要共享
- 某个状态需要在任何地方都可以拿到
- 一个组件需要改变全局状态
- 一个组件需要改变另一个组件的状态

发生上面情况时，如果不使用 Redux 或者其他状态管理工具，不按照一定规律处理状态的读写，代码很快就会变成一团乱麻。你需要一种机制，可以在同一个地方查询状态、改变状态、传播状态的变化。

总之，不要把 Redux 当作万灵丹，如果你的应用没那么复杂，就没必要用它。另一方面，Redux 只是 Web 架构的一种解决方案，也可以选择其他方案。

*Redux 基本思想*

> 组件不管理自己的 state

用下一节讲到的三大概念来说就是

> 组件发出 `action` , `action` 出发 `reducer` , `reducer` 修改 `store` 状态树

### React-webpack 环境搭建

这里先整出一个会用到 Redux 的环境 --- 组件间通信

第一步:克隆 webpack-react 脚手架

```
$ git clone git@github.com:fightingljm/webpack-react-boilerplate.git redux-demo
$ cd redux-demo
$ ls -a
$ rm -rf .git
```

第二步:重新制作版本上传

```
$ atom .
$ npm start
```

第三步:创建两个组件

一个叫做 PostBody ，另外一个叫做 CommentBox 。我们的目的:让这两个组件间进行数据通信。

- [two components](https://github.com/fightingljm/redux-demo/commit/a25e0eab45e050f9f7d3226967227600867ced74)
- [style](https://github.com/fightingljm/redux-demo/commit/9e19777e88406a2db03ae3f3b065b3364ee4ebbb)
- [commentList](https://github.com/fightingljm/redux-demo/commit/e93fcd6508e51778494f30fca850f6fa95632de8)
- [form style and submit](https://github.com/fightingljm/redux-demo/commit/50f89bbfecd4b5b566b96427d768659eb6a5a83d)

这样基本操作就完成了,具体到我们的小目标:组件间共享评论条数.着实还差一点...
