---
title:  "关于长页面滚动动画实现方法"
date:   2017-04-22 19:29:23
categories: [23]
tags: [廿三,2017,web前端]
---
# 关于长页面滚动动画实现方法

### 此方法适用于`IScroll.js`
---



- 首先我们使用到IScroll的自定义事件 *scroll*  不知道使用的请参考[IScroll文档](https://iiunknown.gitbooks.io/iscroll-5-api-cn/content/customevents.html)；由于*scroll*事件只有在`scroll-probe.js`版本中有效,API文档中是这样说的：
> scroll，内容滚动时触发，只有在scroll-probe.js版本中有效，请参考onScroll event。

- 实现原理简单描述为监控滑动Y轴的坐标来显示指定区域元素的显示，提前写好这个元素显示的动画效果，推荐使用`animation.css`动画库中的`fadeIn`相关动画,演示

---
# 代码演示
## html部分

```html
<div class="scrollWarp">
    <div>
        <!--这里的三个Demo元素都是超出可视范围外的，所以是隐藏状态-->
        <img src="img.png" class="domTest1 showIn">
        <img src="img.png" class="domTest2 showIn">
        <img src="img.png" class="domTest3 showIn">
    </div>
</div>
```

## css部分
```css
.scrollWarp{height:1008px;width:640px;position:relative;overflow:hidden}
.scrollWarp >div> img{position:absolute;width:100px;height:100px;display:none}
.scrollWarp .domTest1{left:50px;top:1300px}
.scrollWarp .domTest2{left:150px;top:1900px}
.scrollWarp .domTest3{left:350px;top:2300px}
.showIn{animation:showIn ease-in-out .8s}/*这里自己去写相关显示动画*/
```

## js部分

```javascript

var iscroll = new Iscroll(".scrollWarp",{probeType:3})//probeType需要使用 iscroll-probe.js 才能生效 probeType
iscroll.on('scroll',function(){
    //此处代码仅作演示相关  实际项目中最好封装相关判断，不然会出现一大串的判断
    //this.y < -1400 && this.y > 2000；这个判断是因为domTest1的y坐标是1300，它本身的高度为100px所以我们需要它完全显示出来之后才显示它，所以当this.y<1400才进入，this.y>2000是为了避免当前domTest2和domTest3还没有出现在可视区域的时候提前显示，这样就不会看到动画效果；后面的以此类推
    //这个代码只能播放一次，可以再优化为重复播放的，就是当元素超出可视区域的时候再隐藏起来就可以了，此处不详细展开，后续再说
    if(this.y < -1400 && this.y > 2000){
        document.getElementByClassName('domTest1').style.display = "block";
    }else if(this.y <2000 && this.y > 2400){
        document.getElementByClassName('domTest1').style.display = "block";
        document.getElementByClassName('domTest2').style.display = "block";
    }else if(this.y <2400){
        document.getElementByClassName('domTest1').style.display = "block";
        document.getElementByClassName('domTest2').style.display = "block";
        document.getElementByClassName('domTest3').style.display = "block";

    }
})

```

**以上内容就是长页面滚动动画实现方法**

`2017-04-22 by R`