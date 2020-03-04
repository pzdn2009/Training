# Theory

## Theory

## 1. 数据模型

类似Unix目录结构的路径，每个路径成为Znode。

* Znode可以存储数据，数据限制在1MB以内
* 路径必须是完整路径，不支持正则元符号匹配
* /zookeeper是一个保留路径，用来保存管理信息，如配额。
* 临时节点。当客户端的会话断开时，就会删除的节点。
* 持久节点。永久保存在服务器上的节点。
* 顺序节点。在创建节点的时候指定了顺序号标识，那么ZK服务器就会创建一个单调递增的数字给此节点，由此节点的父节点维护这个节点的最大值。

## 2. 事务ID

Zk中的事务：能够改变ZK服务器状态的操作。包括节点创建于删除、数据节点内容更新和客户端会话创建与失效等。

分配一个全局的64位数字。

## 3. 节点状态信息

```text
my_data
cZxid = 0xa #Created ZXID，表示该数据节点被创建时的事务ID
ctime = Sat Oct 29 00:26:24 PDT 2016 #Created Time，创建时间
mZxid = 0xa #Modified ZXID，表示该节点最后一次被更新时的事务ID
mtime = Sat Oct 29 00:26:24 PDT 2016 #Modified Time，修改时间
pZxid = 0xa #表示该节点的子节点列表最后一次被修改时的事务ID。内部变更不会影响该值。
cversion = 0 #子节点的版本号
dataVersion = 0 #数据版本号
aclVersion = 0 #节点的ACL版本号
ephemeralOwner = 0x0 #创建该临时节点的会话ID。如果节点为持久节点，则值为0
dataLength = 7 #数据的长度
numChildren = 0 #子节点的个数
```

## 3. ACL

每个Znode都可以设置一个Acl列表。

**权限模式：Scheme**

* digest：用户名，密码。通常是{ username:BASE64\(SHA-1\(username:password\)\) }
* super：超级用户，此模式下用户可以对数据做任何操作。
* ip：ip地址，地址段。如192.168.0.1/24。
* world：最开放的。world:anyone

**授权对象：ID**

不同的模式对应不同的授权对象。

**ACL权限**

* CREATE:create
* READ:get\*
* WRITE:set
* DELETE:delete
* ADMIN：setACL

## 4. Watcher

服务端向客户端推送数据的变更通知。

* 只会触发一次，客户端需要重复注册才会收到更多的通知。
* 一次处理流程：客户端注册Watcher，服务端处理Watcher，客户端回调Watcher。
* 由Watcher接口定义了标准的处理流程，一般需要自己实现此接口。
* WatchedEvent定义了三个数据：KeeperState，EventType，Path，分别是连接状态，数据变更的类型，节点路径。

**可应用Watcher的接口**

* exists

  触发时机：节点创建、删除、变更数据时。

* getData

  触发时机：节点删除、变更数据时

* getChildren

  触发时机：子节点创建、删除时以及节点被删除时。

## 5. Session

* 客户端发起的tcp长连接，通过心跳检测。
* 自动故障切换，所有的会话状态仍然有效。

**会话状态**

CONNECTING，CONNECTED，RECONNECTING，RECONNECTED，CLOSE。

**属性**

* sessionID：标识唯一一次会员。
* timeOut：会话超时时间。
* tickTime：下次会话超时时间。
* isClosing：是否已经被关闭

## 6. Database

* ZKDatabase，内存数据库，负责管理ZK的所有会话，DataTree存储和事务日志。
* ZKDB会定时向磁盘dump快照数据。
* ZK的数据存储是通过DataTree实现的，是一棵树。最小的数据单元是DataNode，即DataTree上面的节点。

