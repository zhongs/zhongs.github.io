# 正则相关

* 手机号验证

```
function isPhone(phone){
    var reg = /^0{0,1}(13[0-9]|15[3-9]|15[0-2]|18[0-9]|17[0-9])[0-9]{8}$/;
    var my = false; 
    if (reg.test(phone))my=true; 
    return my;
}
```

* 金钱格式化   [来源](https://juejin.im/entry/5998f8396fb9a0247c6ec9cd)

```
var test1 = '1234567890'
var format = test1.replace(/\B(?=(\d{3})+(?!\d))/g, ',')

console.log(format) // 1,234,567,890
```
