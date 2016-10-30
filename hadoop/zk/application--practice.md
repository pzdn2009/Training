# Application & Practice

从以下三个方面来做设计

- **W**[c|d|e] watcher：代表三种观察
  - We：exists
  - Wd：getData
  - Wc：getChildren
- **N**[s|e] znode：节点，s为顺序节点，e为临时节点，默认为永久非顺序节点。
- **C** client-rolename：客户端操作，rolename为角色。

# 1.发布订阅——配置中心

能力点：

- 多个订阅客户端，1个发布客户端充当管理员角色。即订阅客户端只能读取配置信息，发布客户端维护配置信息。权限分别为READ和ALL。
- zk服务器结合了Push和Pull，其中服务器向客户端的Push是通过Watcher实现的。
- 解耦物理配置文件。

设计：

/company/system/config/environment/type/keyi

- company：公司名称
- system：系统
- config：代表配置
- environment：环境，如开发，测试，生产
- type：类型，如连接字符串，AppSetting
- keyi：具体的键名。



# 2. 命名服务——服务发现中心

其实可以归结到配置中心，也可以独立出来。

能力点：
- 将一个SOA服务注册上线、下线、更新，通过到其他服务。
- 统计在线信息

设计：

/company/system/soaSvc/environment/type/keyi
- soaSvc：命名服务类型。

# 3. 全局ID

能力点：
- 能够在分布式环境中生成唯一的ID。



设计：

/company/system/globalID/environment/type/keyi
- globalID：全局ID类型
