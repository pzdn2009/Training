# miniapp-api列表


API|描述
--|--
wx.getSystemInfoSync()| 获取系统信息，主要偏设备状态信息
wx.getSystemInfo()| 同上
wx.getUpdateManager| 管理升级更新，包括下载、更新、重启
**wx.request**| 发起HTTPS请求，处理success,fail,complete
wx.downloadFile| 下载文件，包括进度管理
wx.uploadFile| 上传文件，包括进度管理
wx.setStorageSync| 设置缓存
wx.removeStorageSync| 删除缓存
wx.getStorageSync| 获取缓存
wx.clearStorageSync| 清除缓存
wx.previewImage| 预览图片
**wx.login**| 微信登录，并返回code，根据code到服务端进行会话连接
**wx.checkSession**| 检查会话是否已经过期，如过期，需要重新登录
**wx.getUserInfo**| 获取用户信息，`withCredentials`
**wx.requestPayment**| 发起微信支付
**wx.authorize**| 提前向用户发起授权请求，授权项通过scope指定
wx.scanCode| 调起客户端扫码界面进行扫码
