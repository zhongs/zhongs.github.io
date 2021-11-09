# 随机一个头像

* 随机一个头像

```
function rnd(n, m) {
      return Math.floor(Math.random() * (m - n + 1) + n);
}
export let randomImg = () => {
      return https://1251097942.cdn.myqcloud.com/1251097942/tv/scws/wozhidao/images/head/touxiang${rnd(1,1000)}.jpg;
}
```
