# 摇一摇

* 摇一摇 [代码来自](http://www.jb51.net/article/119117.htm)

```
isShow:false,
onShake(e){
    if (!this.isShow) {
        return
    }
    console.log('x:',e.x, 'y:', e.y, 'z:', e.z);
    if (e.x > 1 && e.y > 1) {
        wx.showToast({
            title: '摇一摇成功',
            icon: 'success',
            duration: 1000
        })
    }
},
onShow: function () {
    this.isShow = true;
    wx.onAccelerometerChange(this.onShake);
},
onHide: function () {
    this.isShow = false;
}
```

* 方式二

```
 onShake(e) {

    var getDelFlag = function (val_1, val_2) {
        return (Math.abs(val_1 - val_2) >= 1);
    }

    var x = e.x.toFixed(4),
        y = e.y.toFixed(4),
        z = e.z.toFixed(4);

    var flagX = getDelFlag(x, this.data.shakeData.x),
        flagY = getDelFlag(y, this.data.shakeData.y),
        flagZ = getDelFlag(z, this.data.shakeData.z);

    this.data.shakeData = {
        x: e.x.toFixed(4),
        y: e.y.toFixed(4),
        z: e.z.toFixed(4)
    };

    if (flagX && flagY || flagX && flagZ || flagY && flagZ) {
        if (this.data.shakeInfo.enabled) {
            //console.log('触发了摇一摇');       
        }
    }
},
```
