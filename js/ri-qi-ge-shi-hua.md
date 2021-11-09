# 日期格式化

* 日期格式化 [代码来自](http://www.cnblogs.com/pifoo/archive/2011/10/21/format\_javascript\_date\_time.html)

```
function date2str(x, y) {
    var z = { 
        y: x.getFullYear(), 
        M: x.getMonth() + 1, d: x.getDate(), 
        h: x.getHours(), m: x.getMinutes(), 
        s: x.getSeconds() 
    };
    return y.replace(/(y+|M+|d+|h+|m+|s+)/g, function(v) { 
        return ((v.length > 1 ? "0" : "") + eval('z.' + v.slice(-1))).slice(-(v.length > 2 ? v.length : 2)) 
    });
}
alert(date2str(new Date(), "yy-M-d h:m:s"));  // 17-8-28 14:26:10
alert(date2str(new Date(), "yyyy-MM-d h:m:s"));  // 2017-08-28 14:26:10
```

* 方法二

```
function formatTime(date) {
    var year = date.getFullYear()
    var month = date.getMonth() + 1
    var day = date.getDate()

    var hour = date.getHours()
    var minute = date.getMinutes()
    var second = date.getSeconds()

    function formatNumber(n) {
        n = n.toString()
        return n[1] ? n : '0' + n
    }

    return [year, month, day].map(formatNumber).join('/') + ' ' + [hour, minute, second].map(formatNumber).join(':')
}

formatTime(new Date())  // "2017/08/31 15:15:32"
```
