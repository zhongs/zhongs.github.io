# 获取url参数

* 获取url参数

```
function getParam(attr, paramStr) {
    var str = paramStr || window.location.search;
    var match = RegExp('[?&]' + attr + '=([^&]*)').exec(str)
    return match && decodeURIComponent(match[1].replace(/\+/g, ' '))
}
```

* 返回一个对象

```
/**
 *用来解析url参数
 */
export var parseQueryString = function (url) {
    var obj = {};
    var start = url.indexOf("?") + 1;
    var str = url.substr(start);
    var arr = str.split("&");
    for (var i = 0; i < arr.length; i++) {
        var arr2 = arr[i].split("=");
        obj[arr2[0]] = arr2[1];
    }
    return obj;
}
```
