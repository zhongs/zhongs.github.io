# 显示省略号及多行显示省略号

* 第一行省略号

```
#test{
    width:150px; 
    white-space:nowrap;
    overflow:hidden;
    text-overflow:ellipsis;
}
```

* 第二行省略号

```
#test{
      overflow:hidden; 
      text-overflow:ellipsis;
      display:-webkit-box; 
      -webkit-box-orient:vertical;
      -webkit-line-clamp:2; 
}
```
