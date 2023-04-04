# 获取元素的坐标值

* 获取元素相对于页面的坐标（方法一）&#x20;

```
function offset(node){
    var left = node.offsetLeft, top = node.offsetTop;
    do{
        left += node.offsetLeft;
        top += node.offsetTop;
    }while( node = node.offsetParent );
    return {
        left:left,
        top:top
    }
}
```

* 获取元素相对于页面的坐标（方法二）

```
function offset(node){
    var pos = node.getBoundingClientRect(), doc = document.documentElement;
    return {
        left:pos.left + doc.scrollLeft,
        top:pos.top + doc.scrollTop
    }
}
```
