---
title: 加载 CSS 及 img
---

**添加CSS样式**

方法一:

```js
//例如 Logo.js
import React from 'react';
class Logo extends React.Component {
  render(){
    return(
      <div>
           <img src="https://fightingljm.github.io/images/book-cover.jpg" alt="" style={{width:'100px',border:'2px solid #ccc',borderRadius:'50%'}}/>
      </div>
    )
  }
}
export default Logo;

或者另一种方式
import React from 'react';
class Logo extends React.Component {
  render(){
    let style = {width:'100px',border:'2px solid #ccc',borderRadius:'50%'}
    return(
      <div>
           <img src="https://fightingljm.github.io/images/book-cover.jpg" alt="" style={style}/>
      </div>
    )
  }
}
export default Logo;
```

方法二:
```js
//例如 Signin.js
import React from 'react';
class Signin extends React.Component {
  getStyle(){
    return{
      float : 'left'
    }
  }
  render(){
    let color = 0;
    let styles = {
      leftBtn : {
        background:color ? 'red' : 'yellow'
      },
      rightBtn : {
        background:'blue'
      }
    }
    return(
      <div style={this.getStyle()}>
        <input style={styles.leftBtn} type="button" value="登录"/>
        <input style={styles.rightBtn} type="button" value="注册"/>
      </div>
    )
  }
}
export default Signin;
```

方法三:

**外链样式**

首先要装包

```
$ npm install --save-dev style-loader css-loader
```

没有包会报错:You may need an appropriate loader to handle this file type.

然后修改 webpack.config.js

```js
// webpack.config.js
module.exports = {
  entry:'./src/index.js',
  output:{
    path:'build',
    filename:'bundel.js'
  },
  devtool:'eval',//报错到源代码
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,//排除node_modules文件夹
        use: "babel-loader"
      },
+     {
+       test: /\.css$/,
+       use: ['style-loader','css-loader']
+     }
    ]
  }
}
```

接下来就可以演示一个简单的导出
先看 main.css
```css
/*main.css*/
*{
  margin: 0;
  padding: 0;
}
body{
  background-color: #ccc;
}
```
再看导入到 index.js 时,直接导入文件就可以

```js
  import React from 'react';
  import ReactDOM from 'react-dom';

  import App from './App';
+ import './main.css'  //没有装包会报错:You may need an appropriate loader to handle this file type.
  ReactDOM.render(
    <App/> ,document.getElementById('app')
  )
```
这样就完成了css文件的外链


**本地图片加载**

同样,首先装包
```
$ npm install --save-dev file-loader
```

修改webpack.config.js

```js
module.exports = {
  entry:'./src/index.js',
  output:{
    path:'build',
    filename:'bundel.js',
+    publicPath:'build/'
  },
  devtool:'eval',//报错到源代码
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,//排除node_modules文件夹
        use: "babel-loader"
      },//js文件用babel加载
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      },//css文件的加载,解析文件样式
+     {
+       test: /\.(jpe?g|png)$/,
+       use: 'file-loader'
+     }//图片加载
    ]
  }
}
```
