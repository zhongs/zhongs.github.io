---
title: GIT凭据
date: 2021-07-02 17:06:28
categories:
  - 工具
tags: 
  - git
---


# 查看凭证

```shell
git config --global credential.helper

manager
```
* manager 这种方式，在windows环境下会被window凭证管理
* store git凭证会默认创建在`cat ~/.git-credentials`文件下


# 参阅资料
* https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%87%AD%E8%AF%81%E5%AD%98%E5%82%A8