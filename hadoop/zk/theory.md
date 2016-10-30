# Theory

# 1. 数据模型

类似Unix目录结构的路径，每个路径成为Znode。

- Znode可以存储数据，数据限制在1MB以内
- 路径必须是完整路径，不支持正则元符号匹配
- /zookeeper是一个保留路径，用来保存管理信息，如配额。
- 临时节点。当客户端的会话断开时，就会删除的节点。
- 持久节点。永久保存在服务器上的节点。
- 顺序节点。在创建节点的时候指定了顺序号标识，那么ZK服务器就会创建一个单调递增的数字给此节点，由此节点的父节点维护这个节点的最大值。

# 2. 事务ID

Zk中的事务：能够改变ZK服务器状态的操作。包括节点创建于删除、数据节点内容更新和客户端会话创建与失效等。

分配一个全局的64位数字。

# 3. 节点状态信息
```
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
# 3. ACL

每个Znode都可以设置一个Acl列表。

**权限模式：Scheme**

- digest：用户名，密码。通常是{ username:BASE64(SHA-1(username:password)) }
- super：超级用户，此模式下用户可以对数据做任何操作。
- ip：ip地址，地址段。如192.168.0.1/24。
- world：最开放的。world:anyone

**授权对象：ID**

不同的模式对应不同的授权对象。

**ACL权限**
- CREATE:create
- READ:get*
- WRITE:set
- DELETE:delete
- ADMIN：setACL


