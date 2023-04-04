# 内容垂直居中

* 方法一

```
.container {
    min-height: 6.5em;
    display: table-cell;
    vertical-align: middle;
}
```

* 方法二

```
.container{
    position:absolute;
    top:50%;
    transform:translate3d(0,-50%,0);
}
```

* 方法三

```
.container{
    display:flex;
    align-items:center;
}
```
