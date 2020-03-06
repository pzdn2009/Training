# nexus3

sonatype/nexus3

```text
docker pull sonatype/nexus3
docker pull registry.docker-cn.com/sonatype/nexus3
```

```text
# GIT 地址：
https://github.com/sonatype/docker-nexus3.

# 對應的Dockerfile
https://github.com/sonatype/docker-nexus3/blob/master/Dockerfile

# 運行：

docker run -d -p 8081:8081 --name nexus sonatype/nexus3
curl -u admin:admin123 http://localhost:8081/service/metrics/ping

# To (re)build the image, copy the Dockerfile and do the build:
docker build --rm=true --tag=sonatype/nexus3 .
```

You can tail the log to determine once Nexus is ready:

```text
$ docker logs -f nexus
```

## Persistent Data

Use a data volume container. Since data volumes are persistent until no containers use them, a container can created specifically for this purpose. This is the recommended approach.

```text
$ docker run -d --name nexus-data sonatype/nexus3 echo "data-only container for Nexus"
$ docker run -d -p 8081:8081 --name nexus --volumes-from nexus-data sonatype/nexus3
```

Mount a host directory as the volume. This is not portable, as it relies on the directory existing with correct permissions on the host. However it can be useful in certain situations where this volume needs to be assigned to certain specific underlying storage.

```text
$ mkdir /some/dir/nexus-data && chown -R 200 /some/dir/nexus-data
$ docker run -d -p 8081:8081 --name nexus -v /some/dir/nexus-data:/nexus-data sonatype/nexus
```

