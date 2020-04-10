# miniapp-登录授权


# 1. 登录时序图

![](https://res.wx.qq.com/wxdoc/dist/assets/img/api-login.2fcc9f35.jpg)

# 2. 获取UnionID的方式

1. 小程序：wx.getUserInfo获取；
2. 小程序+后端 == wx.login=>code + code2Session；
3. 后端：getPaidUnionId，支付完成5分钟以内有效；
4. 云函数

# 3. 授权

* 小程序端
* 弹窗
* scope内：用户信息，地理位置，后台定位，通讯地址，摄像头，录音。
* wx.getSetting
* wx.openSetting

Ref wx：
>如果用户未接受或拒绝过此权限，会弹窗询问用户，用户点击同意后方可调用接口；
如果用户已授权，可以直接调用接口；
如果用户已拒绝授权，则不会出现弹窗，而是直接进入接口 fail 回调

# 4. 获取手机号

* openType
* bindgetphonenumber

```html
<button open-type="getPhoneNumber" bindgetphonenumber="getPhoneNumber"></button>
```

Ref wx：
>**使用方法
**需要将 `button` 组件 `open-type` 的值设置为 `getPhoneNumber`，当用户点击并同意之后，可以通过 `bindgetphonenumber` 事件回调获取到微信服务器返回的加密数据， 然后在**第三方服务端结合** session_key 以及 app_id 进行**解密**获取手机号。

# 5. 签名 解密

![](https://res.wx.qq.com/wxdoc/dist/assets/img/signature.8a30a825.jpg)

* 签名验证
* 密文解密
* 获取到的密文，需要在后端进行解密
* encrypteddata + iv


示例：https://juejin.im/post/5a559a48518825733060d11b
