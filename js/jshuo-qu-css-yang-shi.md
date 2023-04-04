# js获取css样式

* js获取css样式 （第一种写法）

```
function getStyle(obj, name) {
    return obj.currentStyle ? obj.currentStyle[name] : getComputedStyle(obj, false)[name];
}
```

* js获取css样式 （更好的写法，代码来自javascript框架设计）&#x20;

```
var getStyle = function(el, name){
    if(el.style){
        //如果传入的name 是这种格式 '-webkit-transform' 便将其转换为 'WebkitTransform'
        name = name.replace(/\-(\w)/g, function(all, letter){
            return letter.toUpperCase();
        });
        if(window.getComputedStyle){
            return el.ownerDocument.getComputedStyle(el, null)[name]
        }else{
            return el.currentStyle[name];
        }
    }
}
```
