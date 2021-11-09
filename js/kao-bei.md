# 拷贝

* 浅拷贝

```
function extend(a, b) {
    for (var key in b) {
        if (b.hasOwnProperty(key)) {
            a[key] = b[key];
        }
    }
    return a;
}
```

* 深拷贝
* [源码来自particles.js](https://github.com/VincentGarreau/particles.js/blob/master/particles.js)

```
Object.deepExtend = function(destination, source) {
    for (var property in source) {
        // 只要 property 它是一个对象 就会为true，并且递归调用自身
        if (source[property] && source[property].constructor && source[property].constructor === Object) {

            destination[property] = destination[property] || {};

             //arguments.callee指向当前函数
             //es6 严格模式 不能使用 arguments.callee / caller
            arguments.callee(destination[property], source[property]);

        } else {
            destination[property] = source[property];
        }
    }
    return destination;
};
```
