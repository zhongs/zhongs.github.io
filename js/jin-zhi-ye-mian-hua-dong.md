# 禁止页面滑动

* 禁止页面滑动

```
function eventDf(e){
    e.preventDefault();
}

//手指移动的时候禁止滑动
document.addEventListener('touchmove', eventDf, false);

//手指离开的时候恢复滑动
document.removeEventListener('touchmove', eventDf, false);
```
