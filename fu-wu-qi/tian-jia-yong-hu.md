# 添加用户

* 添加用户

```
sudo adduser zs
```

* 赋予用户权限

```
sudo vi /etc/sudoers

root    ALL=(ALL:ALL) ALL
zs      ALL=(ALL:ALL) ALL
```

* 切换用户

```
su - zs
```
