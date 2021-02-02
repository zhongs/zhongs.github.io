---
title: Ubuntu系统部署nodejs项目
date: 2017-11-15 14:50:00
tags: nodejs
---
<img align="center" src="http://7xqrwh.com1.z0.glb.clouddn.com/image/deploy/banner.jpg" width="80%" height="50%">
最初学习nodejs，用express写了个项目，想部署到服务器运行；从来没有接触过服务器的我，对于项目的部署是一个模糊的概念；也搜索了很多部署相关的文章，看着也是晕头晕脑的没有看懂；这篇文章主要记录，我的部署过程；
<!-- more -->

## 购买服务器

我买的是阿里云服务器

* 操作系统：Ubuntu 14.04 64位
* cpu：1核
* 内存：1GB
* 高效云盘：40GB

买的最便宜的那一种，也不贵一年才330；

阿里云 https://www.aliyun.com

1.注册进入首页
2.点击`控制台`，进入控制台管理页面
3.点击`云服务器ECS`
4.点击`实例`
5.在点击最右边的`创建实例`

进入到创建实例页面，选择相应选项购买完了，回到管理控制台页面；能看见刚购买的实例；
还有一定要记住好服务器密码；

![实例](http://7xqrwh.com1.z0.glb.clouddn.com/image/deploy/1.png)

在windows上安装<a href="http://www.netsarang.com/products/xsh_overview.html" target="_blank">Xshell</a>
安装成功以后，打开Xshell通过
* 服务器id
* 服务器密码

登录你的服务器

## 用户管理

添加用户
````javascript
    sudo adduser zs 
````

赋予用户权限 
````javascript
    sudo vim /etc/sudoers
    
    root   ALL=(ALL:ALL) ALL   //找到这个位置
    zs     ALL=(ALL:ALL) ALL   //添加这一行
````

切换用户
````javascript
    su - zs
````

## 安装软件

* 更新软件厂库

````javascript
    sudo apt-get update
````

* 安装git

````javascript
    sudo apt-get install git
    git --version   //查看git版本，git是否安装成功
````


* <a href="https://github.com/creationix/nvm" target="_blank">安装nvm</a>

````javascript
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash
````

````javascript
    //如果提示：To run 'curl' please ask your administrator to install the package 'curl'
    //需要先安装curl模块，在安装nvm
    sudo apt-get install curl
````

````javascript
    //输入nvm --version，如果报错 nvm: command not found
    source ~/.bashrc
````

* 安装nodejs

````javascript
    nvm install 6  //会安装nodejs 6的最新版本
    nvm alias default 6.11.4 //设置默认版本
````

* <a href="https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/" target="_blank">安装mongoDB数据库</a>

````javascript
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
````

````javascript
    //这一步安装需要注意，你的Ubuntu 版本
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
````

````javascript
    sudo apt-get update
````

````javascript
    sudo apt-get install -y mongodb-org
````


````javascript
    //启动数据库
    sudo service mongod start
    
    //waiting for connections on port 27017 说明启动成功了
    vi /var/log/mongodb/mongod.log  
````

* 安装pm2

````javascript
    npm install pm2 -g
````


* 安装nginx

````javascript
    sudo apt-get install nginx

    //开机启动
    sudo apt-get install sysv-rc-conf
    sudo sysv-rc-conf nginx on
````


## 上传代码

软件安装的都差不多了，我用的是sublime text编辑器sftp软件上传代码；

在root用户下给zs临时用户添加权限 chown -R zs.zs /work/nbdemo/

此时，可以通过sftp把本地的项目文件上传到服务器的/work/nbdemo/目录下

sftp-config.json 文件

````json
{
    "type": "sftp",
    "save_before_upload": true,
    "upload_on_save": false,
    "sync_down_on_open": false,
    "sync_skip_deletes": false,
    "sync_same_age": true,
    "confirm_downloads": false,
    "confirm_sync": true,
    "confirm_overwrite_newer": false,
    
    "host": "服务器ip地址",
    "user": "用户名",
    "password": "用户密码",
    //"port": "22",
    
    "remote_path": "要上传到服务器的目录路径",
    //忽略上传的文件
    "ignore_regexes": [
        "\\.sublime-(project|workspace)", "sftp-config(-alt\\d?)?\\.json",
        "sftp-settings\\.json", "/venv/", "\\.svn/", "\\.hg/", "\\.git/",
        "\\.bzr", "_darcs", "CVS", "\\.DS_Store", "Thumbs\\.db", "desktop\\.ini","node_modules","./db/"
    ]
  
}

````

### 一个坑

代码上传上去了之后，并成功运行pm2 start ./bin/www

在浏览器地址栏输入服务器ip，不能访问；弄了好几个小时；才发现是要在阿里云控制台，配置安全组；

![实例](http://7xqrwh.com1.z0.glb.clouddn.com/image/deploy/11.png)

后面就可以访问了；

我这里是通过nginx做的端口代理，项目本身是8080端口；

nginx 代理文件， nbdemo-con-8080.conf

文件放在 /etc/nginx/conf.d/目录下

````javascript
    upstream test{
        server 127.0.0.1:8080;
    }

    server {
        listen 80;
        server_name 47.95.221.113;

        location / {
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_set_header X-NginX-Proxy true;
            proxy_redirect off;
            proxy_pass http://test;
        }
    }
````

至此，代码部署就差不多完了，项目也可以访问了；
