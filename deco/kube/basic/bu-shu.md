# 部署

查看節點信息：

```text
> kubectl get nodes
NAME STATUS AGE
host01 Ready 1m
```

run創建一個部署（需要提供部署的名稱，app鏡像地址）：

```text
>kubectl run kubernetes-bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8080
deployment "kubernetes-bootcamp" created
#Error from server (AlreadyExists): deployments.extension
```

* --port 指定了端口

run命令的動作：

* 搜索一個適合的節點來運行app
* 調度app

查看部署：

```text
> kubectl get deployments
NAME DESIRED CURRENT UP-TO-DATE AVAILABLE AGE
kubernetes-bootcamp 1 1 1 1 1m
```

在終端和Kubenetes集群中建立一個代理：

```text
>kubectl proxy
Starting to serve on 127.0.0.1:8001
```

以下命令新開一個終端來執行：

獲取pod名稱：

```text
> export POD_NAME=$(kubectl get pods -o go-template --
nge .items}}{{.metadata.name}}{{"\n"}}{{end}}')

> echo $POD_NAME
Name of the Pod: kubernetes-bootcamp-390780338-sqb9v
```

curl POD：

```text
> curl http://localhost:8001/api/v1/proxy/namespaces/default/pods/$POD_NAME/
The url is the route to the API of the Pod.
```

