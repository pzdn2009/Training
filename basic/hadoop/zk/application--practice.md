# Application & Practice

## Application & Practice

约定

* W\[c\|d\|e\] watcher：代表三种观察
  * We：exists
  * Wd：getData
  * Wc：getChildren
* N\[s\|e\]-R znode：节点，R代表角色。
  * N：永久非顺序节点
  * Ns：永久顺序节点
  * Ne：临时非顺序节点
  * Nes：临时顺序节点。
* C-R：client-rolename：客户端操作，rolename为角色。
* OP：操作
  * C：Create节点
  * R：Read节点
  * D：Delete节点
  * A：Admin节点，Acl
  * W：Write节点

## 1.发布订阅——配置中心

能力点：

* 多个订阅客户端，1个发布客户端充当管理员角色。即订阅客户端只能读取配置信息，发布客户端维护配置信息。权限分别为READ和ALL。
* zk服务器结合了Push和Pull，其中服务器向客户端的Push是通过Watcher实现的。
* 解耦物理配置文件。

设计：

N: \/company\/system\/config\/environment\/type\/keyi

* company：公司名称
* system：系统
* config：代表配置
* environment：环境，如开发，测试，生产
* type：类型，如连接字符串，AppSetting
* keyi：具体的键名。

C-sub：订阅客户端

* We\(N\)：在N添加We；
* R\(N\)：读取N的数据。
* 解释：
  * C-sub启动时，注册服务器，并We\(N\)；
  * C-sub当收到Watch时，更新本地配置缓存。
    * C-pub：发布客户端
  * A\/C\/R\/D\/W\(N\)：Admin，Create，Read，Delete，Write

## 2. 命名服务——服务发现中心

其实可以归结到配置中心，也可以独立出来。

能力点：

* 将一个SOA服务注册上线、下线、更新，通过到其他服务。
* 统计在线信息

设计：

Ne-self：\/company\/system\/soaSvc\/environment\/type\/keyi

Ne-other：\/company\/system\/soaSvc\/environment\/type\/keyj

C-sub\/pub：发布订阅客户端（同一个客户端可以充当两个角色）

* R\/W\/A\/C\/D \(Ne-self\)：可对Ne-self进行所有操作
* R\(Ne-other\)：对Ne-other进行读取
* We\(Ne-other\)：Watch Ne-other
* 解释：
  * 当作为C-sub时，启动的时候，R\(Ne-other\)，并We\(Ne-other\)；
  * 当作为C-pub时，可发布自己的数据，W\(Ne-self\)，以及其他操作。

## 3. 全局ID

能力点：

* 能够在分布式环境中生成唯一的ID。

设计：

Nes：\/company\/system\/globalID\/environment\/type\/keyi

C-genID：节点生成客户端

* C\(Nes\)：创建Nes
* 解释：
  * C-genID执行C\(Nes\)时，则顺序返回一个。

## 4.Master选举，分布式排它锁

在分布式环境中选出一个能够唯一运行的示例。

设计：

Ne：\/company\/system\/XLock\/environment\/type\/keyi

C-getXLock：获取锁的客户端

* C\(Ne\)：创建Ne
* Wc\(Ne\)：观察子节点
* 解释：
  * 执行创建之前，先设置一个观察，收到观察时，执行创建
  * C-getXLock执行C\(Ne\)时，成功则获得锁。

## 5. 分布式共享锁

能力点：获取共享锁。

Nes：\/company\/system\/SLock\/environment\/type\/keyi

C-getSLock：

* C\(Nes\)：创建Nes
* R\(Nes\)：读取Nes的子节点
* We\(Nes\)：观察Nes的子节点
* 解释：
  * 先执行创建，然后读取Nes的子节点，确定自己在子节点中的顺序。
  * 如果不是最小，且，前面有写请求，则对自己前面的最后一个写请求节点设置观察，重复；反之，获得锁

## 6. 分布式队列

能力点：排队执行。

Nes：\/company\/system\/Queue\/environment\/type\/keyi

C-queue：

* C\(Nes\)：创建Nes
* R\(Nes\)：读取Nes的子节点
* We\(Nes\)：观察Nes的子节点
* 解释：
  * 先执行创建，然后读取Nes的子节点，确定自己在子节点中的顺序。
  * 如果不是最小，则对自己前面的一个节点设置观察，重复；反之，获得锁。

## 7. 其他功能

成员组管理，机器监控，心跳检测，临时节点来做。

任务分发，发布到指定的节点上。

状态汇报，客户端机器定期向指定节点写入自己的状态信息。

工作进度汇报，实时地将自己执行的任务进度汇报给分发系统。

