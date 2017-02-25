---
title:  React 轮播图
---

- demo 1:index.js

```

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

```
