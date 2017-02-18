---
title: ES6 编译成 ES5
---

ES6语法需要使用babel编译器，到[bable官网](http://babeljs.cn/docs/setup/)。

- 第一步：点击webpack;
- 第二步：installation;
```
npm install --save-dev babel-loader
```
- 第三步：Usage;
```js
module.exports = {
  entry:'./index.js',
  output:{
    path:'build',
    filename:'bundel.js'
  },
  devtool:'eval',//报错到源代码
  //以下是新添加的代码
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,//排除node_modules文件夹
        loader: "babel-loader"
      }
    ]
  }

}
```

> 注意：至今最新版本要把loaders变为rules；把loader变为use；

- 第四步：创建`.babelrc`配置文件，并执行以下命令使配置文件生效
```
npm install babel-preset-env --save-dev
```
- 第五步：在`.babelrc`添加内容
```
{
  "presets": ["env"]
}
```

### ES6语法入门
