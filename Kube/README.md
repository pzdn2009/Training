# Kubernetes

Master,Node,Pod,Label,Replicaiton Controller,Deployment,Horizontal Pod Autoscaler,StatefulSet,Service,Volume,Persistent Volume,Namespace,Annotation

kubeadm,kubectl,DNS,Ingress,Proxy,安全，PV，PVC，StorageClass，RESTAPI

Helm，Dashboard

# V6 

S1：技术趋势

# V5

S1：Helm，实验，Heapster，Chart，Release，Repository，HelmClient，TillerServer，search，install，status，upgrade，rollback，delete，目录结构 ，私有repository，

# V4

S1：运维
Node类型，unscheduled，patch，cordon，uncordon，扩容，Label更新，namespace，set-context，set-cluster，use-context，Requests，Limits，CPU，Memory，单位，cpu-shares，查看node描述，强行终止，LimitRange，资源配额，ResourceQuota，驱逐。

etcd集群，master集群，cAdvisor，Heapster。

Fluentd，FEK，Audit Log，Dashboard。

Event，容器日志，kubectl logs pod_name，kube服务日志

# V3

S1:核心原理


# V2
S1:

kubeadm安装，私有云，kubelet，启动参数，CA
kubectl：cluster-info,create,desribe,expose,get,label,rolling-update,scale,delete,exec,logs。创建资源对象，查看资源对象，描述资源对象，删除资源对象，执行容器命令，查看容器日志。

POD：全部配置格式，静态POD，describe pod，configmap，env，envFrom，DownwardAPI，Pending、Running、Succeeded、Failed、Unknown，Always、OnFailure、Never，LivenessProbe，ReadinessProbe，健康检查方式，ExecAction，TCPSocketAction，HTTPGetAction。Deployment/RC自动调度，nodeSelector，nodeAffinity，Taints，Tolerations，DaemonSet，Job，CronJob，Init Container。kubectl edit，Rollover，rollout，pause/resume更新，scale，HPA，StatefulSet。

SVC，配置格式，expose，Create SVC，port，targetPort，selector，RoundRobin，SessionAffinity，多端口服务，HeadLess Service，clusterIP None，Seedprovider，端口映射，NodePort，LoadBalance，DNS，Ingress

# V1

S1:

禁用防火墙，单机集群，创建RC，服务端口号ports，containerPort，创建服务，Cluster IP，Pause容器，NodePort->Port。
* RC，SVC配置格式
* 熟悉kubectl子命令
* 手动停止SVC对应容器的进程，观察后续反映
* 修改RC数量，重新发布，观察结果
* SVC与Pod的关联

Master节点，kube-apiserver，kube-controller-manager，kube-scheduler，etcd。

Node节点，kubelet，kube-proxy，Docker Engine。

Pod，Pause，PodIP，Flannel，Open vSwitch，Event，CPU，Memory，Requests,Limits，Endpoint（PodIP+Container Port）

Label，版本，环境，架构tier,frontend,backend,middleware，基于集合，matchLabels，matchExpressions。

RC，Pod模板，scale命令，Rolling Update，Replia Set，伸缩。
* 大多数情况下，通过定义RC实现创建过程及副本数量的自动控制
* RC包括完整的Pod定义模板
* RC通过Label Selector机制实现对Pod副本的自动控制
* 通过改变RC里的Pod副本数量，可以实现Pod的扩容或缩容功能
* 通过改变RC里POD模板中的镜像版本，可以实现POD的滚动升级功能。

Deployment，POD编排，Replica Set，应用场景，HPA，Headless Service，DNS域名。

SVC，kube-proxy，SVCName+ SVCClusterIP，多端口，容器SVC发现，NodeIP，PODIP，ClusterIP，集群外访问，NodePort。

Volume，容器挂在路径，POD声明，emptyDir，hostPath，gcePersistentDisk，PD，awsElasticBlockStore，NFS。

持久Volume，PV，ReadWriteOnce，ReadOnlyMany，ReadWriteMany，PersistenceVolumeClaim，PVC定义，POD申请挂载。

Namespace，多租户。Annotation。

# V1.1
1. 基礎概念
2. 創建集群
3. 部署
