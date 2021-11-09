# 主动授权

* 微信选择地址

```
/**
 * 微信选择地址
 */
export let wxAddress = (cb) => {

    let chooseAddress = () => {
        wx.chooseAddress({
            success: (res) => {
                cb && cb(res)
            }
        })
    }

    wx.getSetting({
        success(res) {
            if (!res.authSetting['scope.address']) {
                wx.authorize({
                    scope: 'scope.address',
                    success() {
                        chooseAddress();
                    },
                    fail: () => {
                        wx.openSetting({
                            success: (res) => {
                                if (res.authSetting["scope.address"]) {
                                    chooseAddress();
                                }
                            }
                        })
                    }
                })
            } else {
                chooseAddress();
            }
        }
    })
}
```
