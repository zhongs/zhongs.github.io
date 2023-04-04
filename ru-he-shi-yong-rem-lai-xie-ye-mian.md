# 如何使用rem来写页面

1.方式一

* 页面先引入[lib-flexible](https://github.com/amfe/lib-flexible/blob/2.0/index.js) （它把手机屏幕宽度除以了10，比如iphone5 宽度640；除以10是64，此时页面根标签fontSize的大小就是64px）
* 根据设计图的宽度，定义一个baseWidth基本宽度，用来换算rem；如果设计图宽640px，则baseWidth = 64，设计图宽750px，baseWidth = 75
* 换算rem：测量的设计宽度/baseWidth = rem;

2.方式二（我见的这个方式，还未理解）

* 先对页面做设置

```
((doc, win) => {
  const docEl = doc.documentElement,
    resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
    recalc = () => {
      let clientWidth = docEl.clientWidth;
      if (!clientWidth) return;
      docEl.style.fontSize = 16 * (clientWidth / 320) + 'px';
    };
  if (!doc.addEventListener) return;
  win.addEventListener(resizeEvt, recalc, false);
  doc.addEventListener('DOMContentLoaded', recalc, false);
  //当dom加载完成时，或者 屏幕垂直、水平方向有改变进行html的根元素计算
})(document, window);
```

* baseWidth = 1rem/20 ;
* rem = 测量的设计宽度 \* baseWidth

3.方式三

* 设置根标签字体大小（10.8 是设计图宽度除以100 以后的值）

```
(function(){
    var html = document.documentElement;
    var htmlW = html.clientWidth || window.innerWidth;
    html.style.fontSize = htmlW / 10.8 + "px";
})()
```

* rem = 测量的设计宽度 / 100
