---
title: box-sizing属性的理解
date: 2017-04-13 08:51:58
tags:
---
说到 IE 的 bug，一个臭名昭著的例子是它对于“盒模型”的错误解释：在 IE5.x 以及 Quirks 模式的 IE6/7 中，将 border 与 padding 都包含在 width 之内。这为前端工程师的工作平添了不少麻烦，几户每个需要定义尺寸的 box 都要思量一下：是否触发了“盒模型 bug”？

同时，由于另一撮浏览器对标准的遵从，我们在精确定义一个在有限空间内显示的 box 时，也需要计算一下：留给它的空间只有那么大，刨去 border 和 padding，我们该把它的 width 写成多少呢？

这种情况在 CSS3 时代有了改善，得益于这个叫做 box-sizing 的属性：

box-sizing属性可以为三个值之一：content-box（default），border-box，padding-box。

content-box，border和padding不计算入width之内

padding-box，padding计算入width内

border-box，border和padding计算入width之内，其实就是怪异模式了~
