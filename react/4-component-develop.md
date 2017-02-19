---
title: 组件化嵌入式开发
---

### 组件化嵌入式开发

用组件化实现一个网页的头部 Banner 和 Footer:

```js
//Header.js
import React from 'react';
class Header extends React.Component {
  render(){
    return(
      <div>
         我是头部
      </div>
    )
  }
}
export default Header;

//Banner.js
import React from 'react';
class Banner extends React.Component {
  render(){
    return(
      <div>
         我是Banner
      </div>
    )
  }
}
export default Banner;

//Footer.js
import React from 'react';
class Footer extends React.Component {
  render(){
    return(
      <div>
         我是Footer
      </div>
    )
  }
}
export default Footer;

//index.js
import React from 'react';
import ReactDOM from 'react-dom';

import Header from './Header';
import Banner from './Banner';
import Footer from './Footer';
ReactDOM.render(
  <div>
    <Header/>
    <Banner/>
    <Footer/>
  </div>
  ,document.getElementById('app')
)
```

简化出口: Header.js Banner.js Footer.js,都没有变化,新建一个App.js,让所有文件都从这一个出口出去

```js
//新建 App.js
import React from 'react';

import Header from './Header';
import Banner from './Banner';
import Footer from './Footer';
class App extends React.Component {
  render(){
    return(
      <div>
        <Header/>
        <Banner/>
        <Footer/>
      </div>
    )
  }
}
export default App;


//修改 index.js 为
import React from 'react';
import ReactDOM from 'react-dom';

import App from './App';
ReactDOM.render(
  <App/>,document.getElementById('app')
)
```

在头部假定一个 logo 一个登录注册按钮

```js
//新建 Logo.js
import React from 'react';

class Logo extends React.Component {
  render(){
    return(
      <div>
           <img src="https://fightingljm.github.io/images/book-cover.jpg" alt=""/>
      </div>
    )
  }
}
export default Logo;


//新建 Signin.js
import React from 'react';

class Signin extends React.Component {
  render(){
    return(
      <div>
        <input type="button" value="登录"/>
        <input type="button" value="注册"/>
      </div>
    )
  }
}
export default Signin;


//修改 Header.js 为
import React from 'react';

import Logo from './Logo';
import Signin from './Signin';
class Header extends React.Component {
  render(){
    return(
      <div>
        <Logo/>
        <Signin/>
         我是头部
      </div>
    )
  }
}
export default Header;
```
