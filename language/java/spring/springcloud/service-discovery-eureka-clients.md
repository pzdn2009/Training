# Service Discovery Eureka Clients

## Eureka Client篇

![](../../../../.gitbook/assets/eurekaclient%20%281%29.png)

### 1. 引入Client？

```markup
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Camden.SR5</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 2. 注册Client？

当客户端向服务器注册的时候，客户端会提供**元数据**，such as host and port, health indicator URL, home page etc。

Euraka从每一个客户端接收心跳包。如果指定的时间内没有收到心跳包，那么将会删除注册。

两种方式：

* @EnableEurekaClient。Eureka专用
* @EnableDiscoveryClient。通用

## 3. EnableEurekaClient ？

EnableEurekaClient 注释可以是应用拥有两种角色：

* instance。it registers itself
* client。 it can query the registry to locate other services

## 4. ServiceInstance接口？

DiscoveryClient：

```java
public interface DiscoveryClient {
    String description();

    ServiceInstance getLocalServiceInstance();

    List<ServiceInstance> getInstances(String var1);

    List<String> getServices();
}

public interface ServiceInstance {
    String getServiceId();

    String getHost();

    int getPort();

    boolean isSecure();

    URI getUri();

    Map<String, String> getMetadata();
}
```

可以看出：

* 主要是获取url，元数据。

### 5. @EurekaClient？

可以查询Application，Instance，InstanceStatus

### 6. EurekaClientConfigBean & eureka.client.\*

Docs:[https://github.com/spring-cloud/spring-cloud-netflix/blob/master/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaClientConfigBean.java](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaClientConfigBean.java)

```java
@ConfigurationProperties("eureka.client")
public class EurekaClientConfigBean implements EurekaClientConfig, EurekaConstants {
    public static final String PREFIX = "eureka.client";
    @Autowired(
        required = false
    )
    PropertyResolver propertyResolver;
    public static final String DEFAULT_URL = "http://localhost:8761/eureka/";
    public static final String DEFAULT_ZONE = "defaultZone";
    private static final int MINUTES = 60;
    private boolean enabled = true;
    private EurekaTransportConfig transport = new CloudEurekaTransportConfig();
    /**
     * Indicates how often(in seconds) to fetch the registry information from the eureka
     * server.
     */
    private int registryFetchIntervalSeconds = 30;

    /**
     * Indicates how often(in seconds) to replicate instance changes to be replicated to
     * the eureka server.
     */
    private int instanceInfoReplicationIntervalSeconds = 30;

    /**
     * Indicates how long initially (in seconds) to replicate instance info to the eureka
     * server
     */
    private int initialInstanceInfoReplicationIntervalSeconds = 40;

    /**
     * Indicates how often(in seconds) to poll for changes to eureka server information.
     * Eureka servers could be added or removed and this setting controls how soon the
     * eureka clients should know about it.
     */
    private int eurekaServiceUrlPollIntervalSeconds = 300;
    private String proxyPort;
    private String proxyHost;
    private String proxyUserName;
    private String proxyPassword;

    /**
     * Indicates how long to wait (in seconds) before a read from eureka server needs to
     * timeout.
     */
    private int eurekaServerReadTimeoutSeconds = 8;

    /**
     * Indicates how long to wait (in seconds) before a connection to eureka server needs
     * to timeout. Note that the connections in the client are pooled by
     * org.apache.http.client.HttpClient and this setting affects the actual connection
     * creation and also the wait time to get the connection from the pool.
     */
    private int eurekaServerConnectTimeoutSeconds = 5;
    private String backupRegistryImpl;
    private int eurekaServerTotalConnections = 200;
    private int eurekaServerTotalConnectionsPerHost = 50;
    private String eurekaServerURLContext;
    private String eurekaServerPort;
    private String eurekaServerDNSName;
    private String region = "us-east-1";
    private int eurekaConnectionIdleTimeoutSeconds = 30;
    private String registryRefreshSingleVipAddress;
    private int heartbeatExecutorThreadPoolSize = 2;
    private int heartbeatExecutorExponentialBackOffBound = 10;
    private int cacheRefreshExecutorThreadPoolSize = 2;
    private int cacheRefreshExecutorExponentialBackOffBound = 10;
    private Map<String, String> serviceUrl = new HashMap();
    private boolean gZipContent;
    private boolean useDnsForFetchingServiceUrls;
    private boolean registerWithEureka;
    private boolean preferSameZoneEureka;
    private boolean logDeltaDiff;
    private boolean disableDelta;
    private String fetchRemoteRegionsRegistry;
    ...
```

### 7. EurekaInstanceConfigBean？

Docs：[https://github.com/spring-cloud/spring-cloud-netflix/blob/master/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaInstanceConfigBean.java](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaInstanceConfigBean.java)

```java
@ConfigurationProperties("eureka.instance")
public class EurekaInstanceConfigBean implements CloudEurekaInstanceConfig, EnvironmentAware {
    private static final String UNKNOWN = "unknown";
    private HostInfo hostInfo;
    private InetUtils inetUtils;
    private String appname = "unknown";
    private String appGroupName;
    private boolean instanceEnabledOnit;
    private int nonSecurePort = 80;
    private int securePort = 443;
    private boolean nonSecurePortEnabled = true;
    private boolean securePortEnabled;
    /**
     * Indicates how often (in seconds) the eureka client needs to send heartbeats to
     * eureka server to indicate that it is still alive. If the heartbeats are not
     * received for the period specified in leaseExpirationDurationInSeconds, eureka
     * server will remove the instance from its view, there by disallowing traffic to this
     * instance.
     *
     * Note that the instance could still not take traffic if it implements
     * HealthCheckCallback and then decides to make itself unavailable.
     */
    private int leaseRenewalIntervalInSeconds = 30;

    /**
     * Indicates the time in seconds that the eureka server waits since it received the
     * last heartbeat before it can remove this instance from its view and there by
     * disallowing traffic to this instance.
     *
     * Setting this value too long could mean that the traffic could be routed to the
     * instance even though the instance is not alive. Setting this value too small could
     * mean, the instance may be taken out of traffic because of temporary network
     * glitches.This value to be set to atleast higher than the value specified in
     * leaseRenewalIntervalInSeconds.
     */
    private int leaseExpirationDurationInSeconds = 90;
    private String virtualHostName = "unknown";
    private String instanceId;
    private String secureVirtualHostName = "unknown";
    private String aSGName;
    private Map<String, String> metadataMap = new HashMap();
    private DataCenterInfo dataCenterInfo;
    private String ipAddress;
    private String statusPageUrlPath;
    private String statusPageUrl;
    private String homePageUrlPath;

    /**
     * Gets the absolute home page URL for this instance. The users can provide the
     * homePageUrlPath if the home page resides in the same instance talking to eureka,
     * else in the cases where the instance is a proxy for some other server, users can
     * provide the full URL. If the full URL is provided it takes precedence.
     *
     * It is normally used for informational purposes for other services to use it as a
     * landing page. The full URL should follow the format http://${eureka.hostname}:7001/
     * where the value ${eureka.hostname} is replaced at runtime.
     */
    private String homePageUrl;
    private String healthCheckUrlPath;
    private String healthCheckUrl;
    private String secureHealthCheckUrl;
    private String namespace;
    private String hostname;
    private boolean preferIpAddress;
    private InstanceStatus initialStatus;
    private String[] defaultAddressResolutionOrder;
    private Environment environment;

    public String getHostname() {
        return this.getHostName(false);
    }

    private EurekaInstanceConfigBean() {
        this.dataCenterInfo = new MyDataCenterInfo(Name.MyOwn);
        this.statusPageUrlPath = "/info";
        this.homePageUrlPath = "/";
        this.healthCheckUrlPath = "/health";
        this.namespace = "eureka";
        this.preferIpAddress = false;
        this.initialStatus = InstanceStatus.UP;
        this.defaultAddressResolutionOrder = new String[0];
    }

    public EurekaInstanceConfigBean(InetUtils inetUtils) {
        this.dataCenterInfo = new MyDataCenterInfo(Name.MyOwn);
        this.statusPageUrlPath = "/info";
        this.homePageUrlPath = "/";
        this.healthCheckUrlPath = "/health";
        this.namespace = "eureka";
        this.preferIpAddress = false;
        this.initialStatus = InstanceStatus.UP;
        this.defaultAddressResolutionOrder = new String[0];
        this.inetUtils = inetUtils;
        this.hostInfo = this.inetUtils.findFirstNonLoopbackHostInfo();
        this.ipAddress = this.hostInfo.getIpAddress();
        this.hostname = this.hostInfo.getHostname();
    }
}
```

#### 7.1 状态页和健康检查

```text
eureka:
  instance:
    statusPageUrlPath: ${management.context-path}/info
    healthCheckUrlPath: ${management.context-path}/health
```

#### 7.2 启用health

```text
eureka:
  client:
    healthcheck:
      enabled: true
```

### 8. 认证？

eureka.client.serviceUrl.defaultZone [http://user:password@localhost:8761/eureka](http://user:password@localhost:8761/eureka)

### 9. 元数据？

基础：hostname, IP address, port numbers, status page and health check. 其他：eureka.instance.metadataMap

