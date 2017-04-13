---
title: CSS实现水平、垂直居中的8中方法
date: 2017-04-07 17:09:30
tags: CSS
---
# 脱离文档流的居中
### margin:auto法
我们经常用margin:0 auto来实现水平居中，而一直认为margin:auto不能实现垂直居中……实际上，实现垂直居中仅需要声明元素高度和下面的CSS:
```css
div{
      width: 400px;
      height: 400px;
      position: relative;
      border: 1px solid #465468;
 }
img{
      position: absolute;
      margin: auto;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
 }
```
### 负margin法
```css
.container{
      width: 500px;
      height: 400px;
      border: 2px solid #379;
      position: relative;
 }
.inner{
      width: 480px;
      height: 380px;
      background-color: #746;
      position: absolute;
      top: 50%;
      left: 50%;
      margin-top: -190px; /*height的一半*/
      margin-left: -240px; /*width的一半*/
 }
```
# 未脱离文档流元素的居中
### table-cell法
```css
div{
    width: 300px;
    height: 300px;
    border: 3px solid #555;
    display: table-cell;
    vertical-align: middle;
    text-align: center;
}
img{
    vertical-align: middle;
}
```
