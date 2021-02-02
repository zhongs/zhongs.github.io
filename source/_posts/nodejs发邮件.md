---
title: nodejs发邮件
date: 2017-11-30 12:32:37
tags: nodejs
---

<img align="center" src="http://7xqrwh.com1.z0.glb.clouddn.com/image/nodesms/timg.jpg" width="80%" height="50%">

用nodejs发送邮件

<!-- more -->

````javascript
const nodemailer = require("nodemailer");

//http://blog.csdn.net/zzwwjjdj1/article/details/51878392

nodemailer.createTestAccount((err, account) => {
  let transporter = nodemailer.createTransport({
    service: "qq",
    auth: {
      user: "191492580@qq.com", // 发送人邮箱
      pass: "xxxxxxxxxxxxxxxxx" // 进入QQ个人邮箱, 设置-账户-开启服务POP3/SMTP服务,并生成授权码,现在获取授权码需要验证手机号等.
    }
  });

  let mailOptions = {
    from: "191492580@qq.com",  // 发送人邮箱
    to: "826177087@qq.com",   //接收人邮箱
    subject: "Hello ✔",
    // text: "Hello world?", // 接受者,可以同时发送多个,以逗号隔开
    //定义内容
    html: ` 
    <h2>hello </h2>
    <a href="https://github.com/"></a>
    `,
    //发文件
    attachments: [
      { filename: "package.json", path: "./package.json" },
      { filename: "content", content: "发送内容" }
    ]
  }; 

  transporter.sendMail(mailOptions, (error, info) => {
    if (error) {
      return console.log(error);
    }
    console.log("Message sent: %s", info.messageId);
    console.log("Preview URL: %s", nodemailer.getTestMessageUrl(info));
    // Message sent: <b658f8ca-6296-ccf4-8306-87d57a0b4321@blurdybloop.com>
    // Preview URL: https://ethereal.email/message/WaQKMgKddxQDoou...
  });
});

````