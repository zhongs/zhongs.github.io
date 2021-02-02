---
title: ali-oss 同步到 minio
date: 2021-02-02 11:29:39
tags:
---

* `Rclone` 是一个命令行程序，用于管理云存储上的文件。它是云供应商Web存储界面的功能丰富的替代方案。超过40种云存储产品支持rclone，包括S3对象存储，业务和消费者文件存储服务以及标准传输协议。Rclone具有等效于unix命令rsync，cp，mv，mount，ls，ncdu，tree，rm和cat的强大的云功能。 Rclone熟悉的语法包括Shell管道支持和--dry-run保护。它可在命令行，脚本或通过其API使用。用户将rclone称为“云存储的瑞士军刀”和“与魔术不可区分的技术”。

* `amazon (S3)` 是一个公开的服务，Web 应用程序开发人员可以使用它存储数字资产，包括图片、视频、音乐和文档。 S3 提供一个 RESTful API 以编程方式实现与该服务的交互。

# 环境
* 服务器 CentOS 7 

# rclone 安装

```shell
curl https://rclone.org/install.sh | sudo bash
```

# 修改配置文件

*  `~/.config/rclone/rclone.conf`

```shell
[root@localhost ~]# cat .config/rclone/rclone.conf

[oss-cn-beijing]
type = s3
provider = Alibaba
env_auth = false
access_key_id = <YOUR_ACCESS_KEY_ID> # 阿里云后台查看
secret_access_key = <YOUR_SECRET_ACCESS_KEY> # 阿里云后台查看
endpoint = <YOUR_REGION>.aliyuncs.com  # 阿里云后台查看
acl = private
storage_class = Standard


[minio]
type = s3
env_auth = false
access_key_id = minio  
secret_access_key = minio123
region = us-east-1
endpoint = http://127.0.0.1:9000
location_constraint =
server_side_encryption =
```

# 同步
* 将 ali-oss 同步到 minio `--transfers` 设置并发数量 `-P` 显示实时进度

``` shell
rclone sync -P oss-cn-beijing:bucket-test007 minio:test --transfers=10
```

# 参阅资料
* https://rclone.org/
* https://sunpma.com/864.html
* https://yq.aliyun.com/articles/749107