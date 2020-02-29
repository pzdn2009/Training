# http和https代理


Ref：https://docs.docker.com/config/daemon/systemd/

The Docker daemon uses the **HTTP_PROXY**, **HTTPS_PROXY**, and **NO_PROXY** environmental variables in its start-up environment to configure HTTP or HTTPS proxy behavior. 

该方法是持久化的，修改后会一直生效。该方法覆盖了默认的docker.service文件。

```
# 创建systemd drop-in文件
mkdir -p /etc/systemd/system/docker.service.d

# 编辑http代理文件
vim /etc/systemd/system/docker.service.d/http-proxy.conf

[Service]
Environment="HTTP_PROXY=http://proxy.example.com:80/" "NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp"

# 编辑https代理文件
vim /etc/systemd/system/docker.service.d/https-proxy.conf

[Service]
Environment="HTTPS_PROXY=https://proxy.example.com:443/"

# 重启服务
systemctl daemon-reload
systemctl restart docker
```