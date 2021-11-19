# 手机号，身份证显示 \*

```
 // 用户名
 const [firstName, ...lastName] = username.split('')
 const usernmae = `${firstName}${lastName.map(() => '*').join('')}`

 // 身份证
 const str = cardNumber
 const startStr = str.substring(0, 6)
 const endStr = str.substring(str.length - 6, str.length)
 const label = [...new Array(str.length - 12).keys()].map(() => '*').join('')
```
