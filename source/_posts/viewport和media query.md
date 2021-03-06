---
title: viewport和media query
date: 2017-04-11 15:44:00
tags:
- 响应式
- CSS
---
# viewport
### width=device-width:
你可以定义viewport的宽度.如果你不使用width=device-width,在移动端上你的页面延伸会超过视窗布局的宽度(width=980px),如果你使用了width=device-width,你的页面将会显示为合适的移动端宽度(width=320px),我们可以使用meta标记:
```html
<meta name="viewport" content="width=device-width">
```
我们看见很多网站都建议把content属性的值设置为width=device-width。这相当于告诉浏览器将页面宽度假设为设备宽度。不幸的是，只有当设备是纵向时假设才是正确的。当我们把设备旋转成横向时，device-width还是和纵向的一样（比如，320px），这意味着，即使我们把页面设计成适应了480px横向设备，它还是会返回320px的效果。
如果我们的页面在纵向和横向设备中样式相同，那么我们就可以用width=device-width就足够了，需要注意的是这个是唯一告诉android设备使用设备宽度的方法。

### initial-scale=1.0,maximum-scale=1.0
 在大多数手机上,默认的缩放在手机浏览器上可能会触发"zoom".为了阻止用户缩放,你可以设置initial-scale=1.0,下面是移动视窗的完整写法:
 ```html
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 ```
# Media Queries
```css
@media screen and (max-width: 600px) { /*当屏幕尺寸小于600px时，应用下面的CSS样式*/
  .class {
    background: #ccc;
  }
}
```
要注意的是由于网页会根据屏幕宽度调整布局，所以不能使用绝对宽度的布局，也不能使用具有绝对宽度的元素。这一条非常重要，否则会出现横向滚动条。 
