# 软件安装

* 更新软件厂库

```
sudo apt-get update
```

* 安装git&#x20;

```
sudo apt-get install git

//查看git安装是否成功
git --version
```

* 安装nginx（后面会用它做80端口的反向代理，在服务器上跑多个node实例）

```
sudo apt-get install nginx
```

* 安装nvm （[https://github.com/creationix/nvm）](https://github.com/creationix/nvm%EF%BC%89)

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash

//如果提示：To run 'curl' please ask your administrator to install the package 'curl'
//需要先安装，curl 模块
sudo apt-get install curl

//nvm: command not found
source ~/.bashrc

nvm --version
```

* 安装nodejs

```
//nvm 安装nodejs 6最新版本
nvm install 6
//设置默认版本
nvm alias default 6.11.4
```

* 安装mongoDB 数据库 （[https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/）](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/%EF%BC%89)

```
1. sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

```
2. echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

```
3. sudo apt-get update
```

```
4. sudo apt-get install -y mongodb-org
```

```
//启动数据库
sudo service mongod start

//查看数据库是否启动
vi /var/log/mongodb/mongod.log
//waiting for connections on port 27017 说明启动成功
```

```
//创建数据库 超级角色
db.createUser({user:'new_base', pwd:'sjk123',roles:[{role:'userAdminAnyDatabase', db:'admin'}]})

db.auth('new_base','sjk123')

//创建其它数据库 角色 省略了

//修改数据库配置文件
sudo vi /etc/mongod.conf

    #security:

    security
      authorization: 'enabled'

//重启数据库
sudo service mongod restart

//在直接操作数据库 就会报错，需要先
use admin
//在
db.auth('new_base', 'sjk123')

//后面在 show dbs 就不报错了
```
