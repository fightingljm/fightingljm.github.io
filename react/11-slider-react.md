---
title:  React 轮播图
---

### index.js

```js
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import Slider from './Slider'
import './main.css';
import Img1 from '../images/1.jpg'
import Img2 from '../images/2.jpg'
import Img3 from '../images/3.jpg'
import Img4 from '../images/4.jpg'
let imgs = [Img1,Img2,Img3,Img4]
ReactDOM.render(
  <div>
    <Slider imgs={imgs}/>
  </div>
  ,document.getElementById('app')
)


//Slider.js
import React from 'react';
class Slider extends React.Component {
  constructor() {
    super();
    this.state={
      nowScroll:0 // 0 是第一张
    }
  }
  scroll(num){
    let next = this.state.nowScroll+num;
    if(next>=this.props.imgs.length){
      return this.setState({nowScroll:0})
    }//从右没有图片时到左边第一张
    if(next<0){
      return this.setState({nowScroll:this.props.imgs.length-1})
    }//从左没有图片时到右边第四张
    return this.setState({nowScroll:next})//正常轮播
  }
  // //左右的点击事件
  // handleClick(which){
  //   //this.setState({nowScroll:this.state.nowScroll+num})
  //   //先清掉定时器=>然后执行 scroll() => 最后打开定时器
  //   clearInterval(this.interval)
  //   this.scroll(which)
  //   this.goPlay()
  // }
  // //一排按钮点击事件
  // handleDotted(index){
  //   // console.log(index);
  //   //这里的思路是让点击的 index 减去现在所处位置的 nowScroll ,求得 ul 需要动的 left
  //   //同样先清掉定时器=>然后执行 scroll() => 最后打开定时器,所以有了下面 handlelick 的代码优化
  //   let n = index-this.state.nowScroll;
  //   clearInterval(this.interval)
  //   this.scroll(n)
  //   this.goPlay()
  // }
  handleClick(index){
    let n = index-this.state.nowScroll;
    clearInterval(this.interval)
    this.scroll(n)
    this.goPlay()
  }
  goPlay(){
    this.interval=setInterval(()=> this.scroll(1),3000)
  }//轮播定时器
  componentDidMount(){
    this.goPlay()
  }//组件加载完毕 自动轮播
  render(){
    let liWidth = 100/this.props.imgs.length + '%'
    let styles = {
      ul:{
        width:this.props.imgs.length*100 + '%',
        left:-this.state.nowScroll*100 + '%'
      }
    }
    return(
      <div className='slider-wrap'>{/* 视窗 */}
        <ul style = {styles.ul}>{/* 超级大盒子 */}
          {
            this.props.imgs.map(item => <li key={Math.random()}
              style={{width:liWidth,backgroundImage:`url(${item})`}}></li>)
          }
        </ul>
        {/* 左右按钮 ,以下传参只需要传入1或者-1,但因为之后有了代码的优化,
          为了左右点击和一排按钮的点击使用同一个 handleClick() 方法,传入了类似`this.state.nowScroll-1`的参数,
          这样在使用左右点击是即相当于传入了1或者-1,而使用一排按钮点击时则传入了index*/}
        <button className='arrow left' onClick={this.handleClick.bind(this,this.state.nowScroll-1)}> &lt; </button>
        <button className='arrow right' onClick={this.handleClick.bind(this,this.state.nowScroll+1)}> &gt; </button>
        {/* 一排按钮 */}
        <div className='dotted'>
          {
            this.props.imgs.map((item,index) => <span key={Math.random()}
              onClick={this.handleClick.bind(this,index)}
              style={{backgroundColor:this.state.nowScroll==index ? '#000' : '#888'}}
              ></span>)
          }
        </div>
      </div>
    )
  }
}
export default Slider;
```

### 最后导入 css 即可

```css
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.slider-wrap{
  width: 100%;
  height: 50vh;
  background-color: #ccc;
  overflow: hidden;
  position: relative;
}
.slider-wrap ul{
  list-style: none;
  height: 100%;
  position: relative;
  transition: left 1s ease;
}
.slider-wrap ul li{
  float: left;
  height: 100%;
  -webkit-background-size: cover;
  background-size: cover;
  background-position: 50% 50%;
}
.arrow{
  position: absolute;
  width: 30px;
  height: 50vh;
  background-color: rgba(0,0,0,0);
  line-height: 100%;
  vertical-align: middle;
  font-size: 24px;
  top: 0;
  border: none;
  color: #333;
  font-weight: bolder;
  outline: none;
  cursor: pointer;
}
.arrow:hover{
  background-color: rgba(0,0,0,0.5);
  color: #fff;
}
.right{
  right: 0;
}
.dotted{
  position: absolute;
  bottom: 20px;
  width: 100%;
  text-align: center;
}
.dotted span{
  display: inline-block;
  width: 13px;
  height: 13px;
  border-radius: 50%;
  margin-right: 10px;
  cursor: pointer;
}

```
