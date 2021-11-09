# 音频切换

* wx.playBackgroundAudio / wx.createAudioContext("audio") 切换音
  * wx.pauseBackgroundAudio() 方法暂停，再播放时，安卓手机seek无效；
  * wx.stopBackgroundAudio() 方法暂停，再播放时，苹果手机播放wx.playVoice临时文件，播放不完整；
