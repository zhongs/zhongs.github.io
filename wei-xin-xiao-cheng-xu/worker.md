# Worker

* 小程序支持Worker，在处理大量运算时可以考虑使用它；
* Worker其实就是一种发布订阅模式，基于事件的监听和触发；
* 初始化及监听接收数据

```
onLoad: function (options) {
    var _this = this;
    if (Worker) {
        //config.worker 是一个遵循同源策略的外网url,不能是小程序相对地址；
        this.worker = new Worker(config.worker);
        //接收信息
        this.worker.onmessage = function (e) {

            console.log('test:',e);

            _this.setData({
                result:e.data
            });
        }
    }
}
```

* 发数据

```
//发送了一个数组
this.worker.postMessage([this.data.one, this.data.two]);
```

* 新线程worker.js

```
//接收数据
onmessage = function (e) {
    console.log('worker.js e:', e);
    var result = e.data[0] * e.data[1]
    //发送数据  
    postMessage(123);
}
```
