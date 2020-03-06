# CrossDomain

## WebAPI 支持跨域

## 1. web.config 配

```markup
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="Access-Control-Allow-Origin" value="*" />
        <add name="Access-Control-Allow-Headers" value="Content-Type" />
        <add name="Access-Control-Allow-Methods" value="GET, POST, PUT, DELETE, OPTIONS" />
      </customHeaders>
    </httpProtocol>
    <handlers>
      <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
      <remove name="OPTIONSVerbHandler" />
      <remove name="TRACEVerbHandler" />
      <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
    </handlers>
  </system.webServer>
```

## 2. 提供Option方法.

### 2.1 對於每個Controller，可以加上

```csharp
[HttpOptions]
public string Options()
{
    return null; // HTTP 200 response with empty body
}
```

### 2.2 或者使用Handler

跨越Handler：

```csharp
/// <summary>
/// 跨域支持
/// </summary>
public class CrossDomainHandler : DelegatingHandler
{
    /// <summary>
    /// 
    /// </summary>
    /// <param name="request"></param>
    /// <param name="cancellationToken"></param>
    /// <returns></returns>
    protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        if (request.Method == HttpMethod.Options)
        {
            HttpResponseMessage response = request.CreateResponse<string>(HttpStatusCode.OK, null);
            TaskCompletionSource<HttpResponseMessage> tcs = new TaskCompletionSource<HttpResponseMessage>();
            tcs.SetResult(response);
            return tcs.Task;
        }
        return base.SendAsync(request, cancellationToken);
    }
}
```

加入攔截：

```csharp
public static class WebApiConfig
{
    public static void Register(HttpConfiguration config)
    {
        config.MessageHandlers.Add(new CrossDomainHandler());
    }
}
```

