# YARN提交过程分析

角色：
* client
* ResMgr
* NodeMgr
* AppMst
* Container


大致阶段：
1. 启动AppMstr；
2. AppMstr向ResMgr申请资源；
3. AppMstr向NodeMgr申请启动资源；
4. NodeMgr执行，并报告给AppMstr；
5. 资源关闭；

涉及协议：
1. resource-request；AppMaster与ResourceManager之间
2. container-launch-specification；AppMstr与NodeMgr之间
3. application-specific；Container与AppMstr之间

详细流程：

1. `client`程序向`ResourceManager`提交`application`并请求一个`ApplicationMaster`实例

2. `ResourceManager`找到可以运行一个`Container`的`NodeManager`，并在这个`Container`中启动`ApplicationMaster`实例

3. `ApplicationMaster`向`ResourceManager`进行`Register`，注册之后`client`就可以查询`ResourceManager`获得自己`ApplicationMaster`的详细信息，以后就可以和自己的ApplicationMaster直接交互了

4. 在平常的操作过程中，`ApplicationMaster`根据`resource-request协议`向`ResourceManager`发送`resource-request请求`

5. 当`Container`被成功分配之后，`ApplicationMaster`通过向`NodeManager`发送`container-launch-specification`信息来启动`Container`， `container-launch-specification`信息包含了能够让`Container`和`ApplicationMaster`交流所需要的资料

6. `Application`的代码在启动的`Container`中运行，并把运行的进度、状态等信息通过`application-specific协议`发送给`ApplicationMaster`

7. 在应用程序运行期间，提交应用的client主动和`ApplicationMaster`交流获得应用的运行状态、进度更新等信息，交流的协议也是`application-specific协议`

8. 一但应用程序执行完成并且所有相关工作也已经完成，`ApplicationMaster`向`ResourceManager`取消注册然后关闭，用到所有的Container也归还给系统.

