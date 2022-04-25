---
title: css实现的3D效果
date: 2022-04-25 15:54:35
tags:
---

一个使用`transform-style: preserve-3d;`的动画效果，来自叶总的授课，挺有意思，这里记录一下
<!-- more -->

html代码如下，全部没有用到js，只用css就能实现

```javascript
<body>
  <div class="card">
    <div class="img-box">
      <img src="./imgs/0.jpeg" alt="" />
      <img src="./imgs/1.jpeg" alt="" />
    </div>
    <div class="detail">
      <span>索隆</span>
    </div>
  </div>
</body>
```

```css
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: aquamarine;
}
.card {
  position: relative;
  width: 300px;
  height: 400px;
  background-color: #ffffff;
  box-shadow: 10px 20px 40px rgba(0, 0, 0, 0.25);
  transform: perspective(1000px);
  transform-style: preserve-3d;
  transition: 1s;
}

.card:hover {
  transform: translateX(-50%);
}

.card .img-box {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  z-index: 2;
  transition: 1s;
  transform-origin: right;
  background-color: #000000;
}

.card:hover .img-box {
  transform: rotateY(180deg);
}

.card .img-box img {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transform-style: preserve-3d;
  backface-visibility: hidden;
}

.card .img-box img:nth-child(2) {
  transform: rotateY(180deg);
}

.card .detail {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card .detail span {
  color: rgb(41, 250, 180);
  font-size: 2em;
  font-weight: 500;
  transition: 0.5;
}

.card .detail span:hover {
  letter-spacing: 4px;
  text-shadow: 1px 1px 0 rgba(41, 250, 180, 0.2),
    2px 2px 0 rgba(41, 250, 180, 0.2), 3px 3px 0 rgba(41, 250, 180, 0.2),
    4px 4px 0 #ccc, 5px 5px 0 #ccc, 6px 6px 0 #ccc;
}

```