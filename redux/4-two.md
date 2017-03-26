---
title: 处理两类数据
---

### 第一个小目标:添加 react-router

参考[add react-router](https://github.com/fightingljm/redux-demo/commit/d2c68285f451dc9e66ad2330e5ce99cfa4ec1908)

### 第二个小目标:添加多个 store 数据,每篇文章显示自己的数据,提交改变自己组件的内容,两组件间互不影响

- 添加多个 store 数据

首先把 reducer 拿出去,store 里边只存放数据

参考 [add rootReducer](https://github.com/fightingljm/redux-demo/commit/9d1dbab542f70b179a917b5f093404255c6563dc)

- 添加另一组评论，每篇 post 显示自己的评论

通过判断 post 的 id 是否可以与路由的id相匹配

参考 [show comments for each post](https://github.com/fightingljm/redux-demo/commit/83a5ad0935a4fac7813c5d392646355c9d6734e0)

- 解决问题

即便现在可以拿到 id ,但是当 store 变化的时候没有把id传回去,换句话说,reducer没有改变store的值

参考 [ADD_COMMENT](https://github.com/fightingljm/redux-demo/commit/e9073ef8addd4a6e2c0930ca627e8f90cd506ad2)
