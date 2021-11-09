# 清除浮动

* 方法一： #test为浮动元素的下一个兄弟元素 （要添加额外的div）

```
#test { 
    clear:both;
}
```

* 方法二：#test为浮动元素的父元素。zoom:1也可以替换为固定的width或height

```
#test {
    display:block;
    zoom:1;
    overflow:hidden;
}
```

* 方法三： #test为浮动元素的下一个兄弟元素

```
#test{ 
    zoom:1;
}
#test:after{ 
    display:block;
    clear:both;
    visibility:hidden;
    height:0;
    content:'';
}
```
