---
title: nodejs发短信验证码
date: 2017-11-27 16:58:16
tags: nodejs
---

<img align="center" src="http://7xqrwh.com1.z0.glb.clouddn.com/image/nodesms/timg.jpg" width="80%" height="50%">

用nodejs发短信验证码的一次实践

<!-- more -->

## [网易云平台](http://netease.im/)

* 登录网易云，创建应用，找到应用的 App Key , App Secret

![图片](http://7xqrwh.com1.z0.glb.clouddn.com/image/nodesms/key.png)

* [短信接入指引](http://dev.netease.im/docs/product/%E7%9F%AD%E4%BF%A1/%E7%9F%AD%E4%BF%A1%E6%8E%A5%E5%8F%A3%E6%8C%87%E5%8D%97?#发送短信/语音短信验证码)

* [接口概述](http://dev.netease.im/docs/product/IM%E5%8D%B3%E6%97%B6%E9%80%9A%E8%AE%AF/%E6%9C%8D%E5%8A%A1%E7%AB%AFAPI%E6%96%87%E6%A1%A3/%E6%8E%A5%E5%8F%A3%E6%A6%82%E8%BF%B0?#API调用说明)

## 业务代码

项目依赖的第三方模块

* request
* sha1

请求配置

```javascript
const options = {
  url: "https://api.netease.im/sms/sendcode.action",
  method: "POST",
  body: "mobile=13871441556",
  headers: {
    "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8"
  }
};
```

生成CheckSum,返回请求headers对象

````javascript
var createCheckSum = function() {
  const AppKey = "xxxxxxxxxxxxxxxx";
  const AppSecret = "xxxxxxxxxxxxxxxxx";
  const CurTime = parseInt(new Date().getTime() / 1000) + "";
  const Nonce = Math.random()
  .toString(36)
  .substr(2, 15);

  let str = AppSecret + Nonce + CurTime;

  return { AppKey, CurTime, CheckSum: sha1(str), Nonce };
};
````

在控制台输入node sendsms 调用代码 (我的js文件名叫sendsms)

````javascript
Object.assign(options.headers, createCheckSum());

request(options, (error, response, body) => {
  console.log("body:", body);  //body: {"code":200,"msg":"3","obj":"1599"}
  if (error) {
    throw error;
  }
  var info = JSON.parse(body);
  if (info.code == 200) {
      //其它业务代码
  }
});
  
````

手机收到的短信验证码

<img align="center" src="http://7xqrwh.com1.z0.glb.clouddn.com/image/nodesms/b.jpg" width="40%">
