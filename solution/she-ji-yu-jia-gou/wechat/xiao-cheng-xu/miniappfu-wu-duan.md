# miniapp-服务端

## 1. 登录

auth.code2Session

根据wx.login获取的code获取SessionKey
```java
public WxMaJscode2SessionResult jsCode2SessionInfo(String jsCode) throws WxErrorException {
        WxMaConfig config = this.getWxMaConfig();
        Map<String, String> params = new HashMap(8);
        params.put("appid", config.getAppid());
        params.put("secret", config.getSecret());
        params.put("js_code", jsCode);
        params.put("grant_type", "authorization_code");
        String result = this.get("https://api.weixin.qq.com/sns/jscode2session", Joiner.on("&").withKeyValueSeparator("=").join(params));
        return WxMaJscode2SessionResult.fromJson(result);
    }
    
public class WxMaJscode2SessionResult implements Serializable {
    private static final long serialVersionUID = -1060216618475607933L;
    @SerializedName("session_key")
    private String sessionKey;
    @SerializedName("openid")
    private String openid;
    @SerializedName("unionid")
    private String unionid;
}
```

## 2. 用户信息

auth.getPaidUnionId

用户支付完成后，获取该用户的 UnionId，无需用户授权。


## 3. auth 

auth.getAccessToken

获取访问Token，调用绝大多数**后台接口**时都需使用 access_token

```java

String GET_ACCESS_TOKEN_URL = "https://api.weixin.qq.com/cgi-bin/token?
grant_type=client_credential&appid=%s&secret=%s";


@Override
  public String getAccessToken(boolean forceRefresh) throws WxErrorException {
    if (!this.getWxMaConfig().isAccessTokenExpired() && !forceRefresh) {
      return this.getWxMaConfig().getAccessToken();
    }

    Lock lock = this.getWxMaConfig().getAccessTokenLock();
    lock.lock();
    try {
      String url = String.format(WxMaService.GET_ACCESS_TOKEN_URL, this.getWxMaConfig().getAppid(),
        this.getWxMaConfig().getSecret());
      try {
        HttpGet httpGet = new HttpGet(url);
        if (this.getRequestHttpProxy() != null) {
          RequestConfig config = RequestConfig.custom().setProxy(this.getRequestHttpProxy()).build();
          httpGet.setConfig(config);
        }
        try (CloseableHttpResponse response = getRequestHttpClient().execute(httpGet)) {
          String resultContent = new BasicResponseHandler().handleResponse(response);
          WxError error = WxError.fromJson(resultContent, WxType.MiniApp);
          if (error.getErrorCode() != 0) {
            throw new WxErrorException(error);
          }
          WxAccessToken accessToken = WxAccessToken.fromJson(resultContent);
          this.getWxMaConfig().updateAccessToken(accessToken.getAccessToken(), accessToken.getExpiresIn());

          return this.getWxMaConfig().getAccessToken();
        } finally {
          httpGet.releaseConnection();
        }
      } catch (IOException e) {
        throw new RuntimeException(e);
      }
    } finally {
      lock.unlock();
    }

  }
```

## 4. 消息

customerServiceMessage.send
uniformMessage.send

## 5. 小程序码

wxacode.createQRCode：获取小程序二维码
wxacode.get：
wxacode.getUnlimited：无限制的二维码

## 5. OCR
* ocr.bankcard，识别银行卡
* ocr.businessLicense，识别营业执照
* ocr.driverLicense，识别驾照
* ocr.idcard，识别身份证
* ocr.printedText，识别印刷体
* ocr.vehicleLicense，识别车牌

## 6. 即时配送

“即时配送”，是指时长在45分钟以内，以生活圈为半径，覆盖范围小于5公里，以点对点为主的即时性、非计划性配送服务。

首要条件，需要签约

## 7. 物流助手

