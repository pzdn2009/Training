# 微信开放平台

官方文档地址：
https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Access_Guide/iOS.html

* 拉起

# 网站登录

* 授权原理：基于OAuth2.0。
 * 请求qrconnect——前端二维码
 * 用户确认登录——微信手机端
 * 返回code——最好到后台服务接收
 * 根据code去拿access_token——后台服务`call`微信，并存储在服务端，并打通登录状态（轮训）
 * 根据access_token访问接口——
 * 根据refresh_token刷新
* 登录`scope：snsapi_login`
* 需要在开放平台注册应用，具有AppID，AppSecret。
* unionid：开放平台上，多个资源的聚合Id，如多个小程序，多个公众号，同一个开放平台下授权
* openId：普通用户的标识，对当前开发者帐号唯一.

掘金登录示例:
```http
https://open.weixin.qq.com/connect/qrconnect?
appid=wx1f78f78832fc2c16&
redirect_uri=https%3a%2f%2fgold.xitu.io%2foauth%2flogin&
response_type=code&
scope=snsapi_login&
state=wechat#wechat_redirect
```

有道云轮训：
```
https://note.youdao.com/ywx/login/query
```

## 获取用户个人信息（UnionID 机制）


```
GET https://api.weixin.qq.com/sns/userinfo?
access_token=ACCESS_TOKEN&openid=OPENID
```

```
{
  "openid": "OPENID",
  "nickname": "NICKNAME",
  "sex": 1,
  "province": "PROVINCE",
  "city": "CITY",
  "country": "COUNTRY",
  "headimgurl": "http://wx.qlogo.cn/mmopen/g3MonUZtNHkdmzicIlibx6iaFqAc56vxLSUfpb6n5WKSYVY0ChQKkiaJSgQ1dZuTOgvLLrhJbERQQ4eMsv84eavHiaiceqxibJxCfHe/0",
  "privilege": ["PRIVILEGE1", "PRIVILEGE2"],
  "unionid": " o6_bmasdasdsad6_2sgVt7hMZOPfL"
}
```

