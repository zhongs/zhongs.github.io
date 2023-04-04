# 将argument对象转换成数组

* 将argument对象转成数组  [代码来自](https://juejin.im/entry/5998f8396fb9a0247c6ec9cd)

```
var argArray = Array.prototype.slice.call(arguments);

或者ES6：

var argArray = Array.from(arguments)
```
