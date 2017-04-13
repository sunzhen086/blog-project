---
title: CSS3的性能与兼容性
date: 2017-04-11 15:04:38
tags: CSS3
---
# 性能
###  translate3d进行gpu加速
```css
  /**不推荐**/
  left: 500px
  /**推荐**/
  transform: translateX(500px);
```
一个元素通过translate3d右移500px的动画流畅度会明显优于使用left属性；
原因是因为：
* CSS动画属性会触发整个页面的重排relayout、重绘repaint、重组recomposite

* Paint通常是其中最花费性能的，尽可能避免使用触发paint的CSS动画属性，这也是为什么我们推荐在CSS动画中使用webkit-transform: translateX(3em)的方案代替使用left: 3em，因为left会额外触发layout与paint，而webkit-transform只触发整个页面composite（这也是为什么推荐在CSS动画中使用webkit-transform: translateX(500px)的方案代替使用left: 500px）；

###  box-shadow和gradients(渐变)
这两个东西在css3里往往都是页面的性能杀手，计算他们尤其消耗cpu, 尤其是在一个元素同时都使用了它们，所以拥抱扁平化设计吧;

# 兼容性
### border-radius
IE9+,Firefox4+,Chrome5+,Safari5+,Opera01.5+,iOS Safari4+,Android Browser2.2+ ,Android Chrome18+

低版本的chrome:-webkit-border-radius:10px;

低版本的firefox:-moz-border-radius:10px;

IE6/7/8:引入ie-css3兼容文件,不支持除了黑色(#000)以外的其他颜色

### box-shadow
IE9.0+,Firefox4.0+,Chrome10.0+,Safari5.1+,Opera10.5+,iOS Safari5.0+,Android Browser4.0+,Android Chrome18.0+

低版本的chrome:-webkit-box-shadow:10px 10px 5px #888;

低版本的firefox:-moz-box-shadow:10px 10px 5px #888;

IE6/7/8:
* 使用filter
* 推荐：引入ie-css3兼容文件behavior:url(ie-css3.htc)

### border-image
IE9+,Firefox4+, Chrome15+,Safari7+, Opera15+, iOS Safari7+, Android Browser4.4+, Android Chrome18+

低版本的chrome:-webkit-background-size:10px 10px 5px #888; //不支持background简写

低版本的firefox:-moz-background-size:10px 10px 5px #888;

IE8 引入backgroundsize.min.htc兼容文件

### text-shadow
IE10+, Firefox3.5+, Chrome4.0+, Safari6.0+

低版本的chrome:-webkit-text-shadow:1px 1px 1px #000;

低版本的firefox:-moz-text-shadow:1px 1px 1px #000;

IE6/7/8:引入ie-css3兼容文件behavior:url(ie-css3.htc);

### transform
IE9+, Firefox3.5+, Chrome4.0+, Safari6.0+, iOS Safari8.4+, Android Browser4.4+, Android Chrome34+

兼容方法：
```css
.transform{
    -webkit-transform: x,y;
    -moz-transform: x,y;
    -ms-transform: x,y;
    -o-transform: x,y;
    transform: x,y;
}
```
IE8及以下：用IE滤镜
```css
{
    filter:fliph;//水平翻转相当于transform:rotateY(180deg)
    filter:flipv;//垂直翻转相当于transform:rotateX(180deg)
}
```
### transition

IE10+, Firefox16+, Chrome26+ ,Safari6.1+ , iOS Safari7+, Android Browser4.4+, Android Chrome25+

兼容方法：
```
p {
  -webkit-transition: all .5s ease-in-out 1s;
  -moz-transition: all .5s ease-in-out 1s;
  -o-transition: all .5s ease-in-out 1s;
  transition: all .5s ease-in-out 1s;
}
```
IE9以及更早的版本，不支持 transition 属性。

### animation
IE10+,Firefox16+, Chrome43+, Safari9+
兼容方法：
低版本的chrome:-webkit-

低版本的firefox：-moz-

IE9及以下不支持

### linear-gradient radial-gradient
IE10+, Firefox16+, Chrome26+, Safari6.1+
兼容方法：

低版本的chrome:-webkit-

低版本的firefox：-moz-

IE9及以下可使用 IE 滤镜处理:
```css
filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#000000',endColorstr='#ffffff');
```

### rgba(r,g,b,a)
IE9+, Firefox2+, Chrome4+, Safari3+, iOS Safari3.2+, Android Browser2.1+, Android Chrome18+

兼容方法：

IE6/7/8不支持使用 rgba 模式实现透明度，可使用 IE 滤镜处理
``` css
filter:progid:DXImageTransform.Microsoft.gradient(startColorstr=#7fff0000,endColorstr=#7fff0000);
```
### flex
IE11+,Firefox22+, Chrome21+, Safari6.1+

低版本的chrome:-webkit- 或者 -webkit-box-flex

低版本的firefox：-moz-box-flex:1;

IE10:-ms-flex:1;

box-flex效果类似于过渡版本和新版本的flex属性；
