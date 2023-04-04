# 省略号动画

* 省略号动画

```
.loading:after {
    overflow: hidden;
    display: inline-block;
    vertical-align: bottom;
    animation: ellipsis 2s infinite;
    /* 省略号的 ascii 码 */
    content: "\2026";
}

@keyframes ellipsis {
    from {
        width: 2px;
    }
    to {
        width: 15px;
    }
}
```
