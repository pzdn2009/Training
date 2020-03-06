# ComposeYml

## 示例

```yaml
version: '2'
services:
  web:
    image: dockercloud/hello-world
    ports:
      - 8080
    networks:
      - front-tier
      - back-tier

  redis:
    image: redis
    links:
      - web
    networks:
      - back-tier

  lb:
    image: dockercloud/haproxy
    ports:
      - 80:80
    links:
      - web
    networks:
      - front-tier
      - back-tier
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock 

networks:
  front-tier:
    driver: bridge
  back-tier:
    driver: bridge
```

可以看到一份标准配置文件应该包含 version、services、networks 三大部分，其中最关键的就是 services 和 networks 两个部分，下面先来看 services 的书写规则。

| 标签 | 层级 | 解释 |
| :--- | :--- | :--- |
| image | services.服务名.image | 指定服务的镜像名称或镜像 ID |
| build | services.服务名.build | 指定Dockerfile的路径，可以绝对，可以相对。如果你同时指定了 image 和 build 两个标签，那么 Compose 会构建镜像并且把镜像命名为 image 后面的那个名字。 |
| ports |  | 映射端口的标签。 |
| volumes |  | 挂载一个目录或者一个已存在的数据卷容器 |

