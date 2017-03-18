---
title: 环境搭建,路由配置, UI 库和 axios
---

### 第一步:搭建环境

- 首先给出简单脚手架 [webpack-react-boilerplate](https://github.com/fightingljm/webpack-react-boilerplate)

- 克隆仓库 => 修改仓库名 => 删除 .git => 初始化新的仓库 => 在远端创建同名仓库并上传 --- (执行以下命令)

```
$ git clone git@github.com:fightingljm/webpack-react-boilerplate.git tiger-liu
$ rm -rf .git
$ git init
...
$ git push ...
```

### 第二步:配置路由

- 首先装包

```
$ npm i --save react-router@3.0.2
```

> 注意:这里用的是 react-router 3.0.2 的版本

- 一大波创建文件正在逼近...

```
$ touch index.js
$ touch app.js
$ touch routes.js
$ mkdir components/Header.js
$ mkdir components/Home.js
```

- 来瞅瞅配置路由的过程中文件发生的具体修改

index.js

```diff
  import React from 'react';
  import { render } from 'react-dom';
+ import Routers from './routes.js';
  import './main.css'

  render(<Routers />, document.getElementById('root'));
```

app.js

```diff
  import React from 'react';
+ import Header from './components/Header.js';

  class App extends React.Component {
  render(){
    return(
      <div>
+       <Header/>
      </div>
    )
  }
  }

  export default App;
```

routes.js

```js
  import React, { PropTypes } from 'react';
  import { Router,Route,hashHistory,IndexRoute } from 'react-router';

  import App from './App.js'
  import Home from './components/Home.js'

  export default function () {
    return(
      <Router history={hashHistory}>
        <Route path='/' component={App}>
          <IndexRoute component={Home}/>
        </Route>
      </Router>
    )
  }
```

components/Header.js

```js
import React from 'react';

class Header extends React.Component {
  render(){
    return(
      <div>
        header
      </div>
    )
  }
}

export default Header;
```

components/Home.js

```js
import React from 'react';

class Home extends React.Component {
  render(){
    return(
      <div>
        home
      </div>
    )
  }
}

export default Home;
```

### 第三步:导入 UI 库

- 首先装包,这里用 [material-ui](http://www.material-ui.com/#/)

```
$ npm install --save material-ui
$ npm install --save react-tap-event-plugin@2.0.1
```

- 之后就该是 UI 库的导入及使用,这里我们暂时用到 `App Bar` , `Dialog` ,`Text Field` 这三个大块

来瞅瞅具体的代码变化吧

index.js

```diff
  ...
  import Routers from './routes.js';
+ import injectTapEventPlugin from 'react-tap-event-plugin';
+ injectTapEventPlugin();
  ...
```

app.js

```diff
  import React from 'react';
+ import MuiThemeProvider from 'material-ui/styles/MuiThemeProvider';
  import Header from './components/Header.js';

  class App extends React.Component {
    render(){
      return(
-       <div>
+       <MuiThemeProvider>
          <Header/>
+       </MuiThemeProvider>
-       </div>
      )
    }
  }

  export default App;
```

components/Home.js

```diff
  import React from 'react';

+ import AppBar from 'material-ui/AppBar';
+ import Dialog from 'material-ui/Dialog';
+ import TextField from 'material-ui/TextField';

+ import IconButton from 'material-ui/IconButton';
+ import ActionHome from 'material-ui/svg-icons/action/home';
+ import FlatButton from 'material-ui/FlatButton';
+ import RaisedButton from 'material-ui/RaisedButton';

  class Header extends React.Component{
+   constructor(){
+     super();
+     this.state={
+       open: false,
+       action:'signin'
+     }
+   }
+   handleClose(){
+     this.setState({open: false})
+   }
+   handleOpen(){
+     this.setState({open: true})
+   }
+   handleUsername(event,username){
+     console.log(username);
+   }
+   handlePassword(event,password){
+     console.log(password);
+   }

    render(){
+     const actions = [
+       <FlatButton
+         label="Cancel"
+         primary={true}
+         onTouchTap={this.handleClose.bind(this)}
+       />,
+       <FlatButton
+         label="Submit"
+         primary={true}
+         keyboardFocused={true}
+         onTouchTap={this.handleClose.bind(this)}
+       />,
+     ]
      return(
        <div>
-         home
+         <AppBar
+           title="Tiger_Liu"
+           iconElementLeft={<IconButton><ActionHome /></IconButton>}
+           onRightIconButtonTouchTap={this.handleOpen.bind(this)}
+           iconElementRight={<FlatButton label='登录/注册' />}
+         />
+         <Dialog
+           title="用户表单"
+           actions={actions}
+           open={this.state.open}
+           onRequestClose={this.handleClose.bind(this)}>

+           <FlatButton label="登录" primary={this.state.action=='signin' ? true : false} onTouchTap={()=>this.setState({action:'signin'})}/>
+           <FlatButton label="注册" primary={this.state.action=='signup' ? true : false} onTouchTap={()=>this.setState({action:'signup'})}/>

+           <br />
+           <TextField hintText="Username" floatingLabelText="username" onChange={this.handleUsername.bind(this)}/><br/>
+           <TextField hintText="Password" floatingLabelText="Password" type="password" onChange={this.handlePassword.bind(this)}/><br/><br />
+         </Dialog>
        </div>
      )
    }
  }

  export default Header;
```

routes.js 和 components/Home.js 暂时没有发生改变

### 第四步: axios 配置路由

- 首先装包

```
$ npm i --save axios
```

之后当然是导入了,在这之前要明确小目标,拿到[ shopAPI 文档](https://coding.net/u/newming/p/shopapi/git)

再推荐 [axios readme](https://github.com/mzabriskie/axios)

再再推荐 [钮老师 tiger](https://github.com/happypeter/tiger)

呃(⊙o⊙)…应该万事俱备,只欠改代码辣~

components/Header.js

```diff
import React from 'react';
+import axios from 'axios';

import AppBar from 'material-ui/AppBar';
import Dialog from 'material-ui/Dialog';
import TextField from 'material-ui/TextField';

import IconButton from 'material-ui/IconButton';
import ActionHome from 'material-ui/svg-icons/action/home';
import FlatButton from 'material-ui/FlatButton';
import RaisedButton from 'material-ui/RaisedButton';

class Header extends React.Component{
  constructor(){
    super();
    this.state={
      open: false,
      action:'signin',
+     username:'',
+     password:'',
+     welcome:'登录/注册'
    }
  }
  handleClose(){}
  ...
+ handleUsername(event,username){
-   console.log(username);
+   this.setState({username:username.trim()})
  }
+ handlePassword(event,password){
-   console.log(password);
+   this.setState({password:password.trim()})
  }
  handleSubmit(){
+   let data = {username:this.state.username,password:this.state.password}
+   // console.log(data);
+   axios.post(`http://api.duopingshidai.com/user/${this.state.action}`,data)
+     .then(res => {
+       console.log(res);
+       this.setState({open:false})
+       if(this.state.action=='signin'&&res.data.msg=='登陆成功'){
+         this.setState({welcome:`你好,${res.data.user}!欢迎登录!`})
+       }
+     })
+     .catch(err => {
+       if(err.response){
+         alert(err.response.data.msg);
+       }else{
+         console.log('Error',err);
+       }
+     })
  }
  render(){
    const actions = [
      <FlatButton
        ...
      />,
      <FlatButton
        ...
-       onTouchTap={this.handleClose.bind(this)}
+       onTouchTap={this.handleSubmit.bind(this)}
      />,
    ]
    return(
      <div className='header-wrap'>
        <div className='app-bar'>
          <AppBar
            ...
-           iconElementRight={<FlatButton label='登录/注册' />}
+           iconElementRight={<FlatButton label={this.state.welcome} />}
          />
        </div>
        <Dialog
          title="用户表单"
          actions={actions}
          open={this.state.open}
          onRequestClose={this.handleClose.bind(this)}>

          <FlatButton label="登录" primary={this.state.action=='signin' ? true : false} onTouchTap={()=>this.setState({action:'signin'})}/>
          <FlatButton label="注册" primary={this.state.action=='signup' ? true : false} onTouchTap={()=>this.setState({action:'signup'})}/>

          <br />
          <TextField hintText="Username" floatingLabelText="username" onChange={this.handleUsername.bind(this)}/><br/>
          <TextField hintText="Password" floatingLabelText="Password" type="password" onChange={this.handlePassword.bind(this)}/><br/><br />
        </Dialog>
      </div>
    )
  }
}

export default Header;
```
