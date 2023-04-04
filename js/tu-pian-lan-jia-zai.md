# 图片懒加载

* 兼容 seajs 模块

```
define(function(require, exports, module) {

    //函数节流
    function throttle(fn, interval) {
        var interval = interval ? interval : 100;
        var canRun = true;
        return function() {
            var _this = this;
            if (!canRun) return;
            canRun = false;
            setTimeout(function() {
                fn.apply(_this, arguments);
                canRun = true;
            }, interval)
        }
    }

    //获取元素距离滚动元素的距离
    function offsetTop(element) {
        var top = element.offsetTop;
        var parent = element.offsetParent;

        while (parent != null) {
            top += parent.offsetTop;
            parent = parent.offsetParent;
        }

        return top;
    }

    var imgArr = document.querySelectorAll('[data-src]');

    fn();

    function fn() {
        var scroll_top = document.body.scrollTop;
        var win_h = window.innerHeight;

        for (var i = 0; i < imgArr.length; i++) {
            if ((scroll_top + win_h) >= offsetTop(imgArr[i])) {
                var src = imgArr[i].getAttribute('data-src');
                if (src && /.jpg|.png|.jpeg|.gif/g.test(src)) {
                    imgArr[i].src = src;
                    imgArr[i].style.opacity = '1';
                }
            }
        }

    }

    document.addEventListener('scroll', throttle(fn), false);

});
```

* Intersection Observer

```
var images = document.querySelectorAll('[data-src]');
var config = {
                  rootMargin: '50px 0px',   //图像在Y轴上的50像素内开始下载
                  threshold: 0.01          // 阈值范围 0.0 - 1.0
             }

function preloadImage(){
  …
}

function onIntersection(entries) {
    entries.forEach(entry => {
        if (entry.intersectionRatio > 0) {
            observer.unobserve(entry.target);
            preloadImage(entry.target);
        }
    });
}

//api兼容检测
if (!('IntersectionObserver' in window)) {
    Array.from(images).forEach(image => preloadImage(image));
} else { 
    var observer = new IntersectionObserver(onIntersection, config );
    images.forEach(image => {
        observer.observe(image);
    });
}
```
