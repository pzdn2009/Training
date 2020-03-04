# Docker命令及参数

## attach

docker attach &lt;选项&gt; &lt;容器名称，ID&gt;

> 用于将stdin和stdout连接到一个正在运行的容器。

* --no-stdin=false Do not attach STDIN，不接入STDIN。
* --sig-proxy=true Proxy all received signals to the process，将所有信号传递给进程，但不传送SIGCHLD，SIGKILL，SIGSTOP信号。

经常使用的信号：

* SIGINT：interrupt信号，输入Ctr+c时发生。
* SIGQUIT：quit信号，输入Ctr+\时发生。
* EOF：终止信号，输入Ctr+d时发生。

## exec

docker exec \[OPTIONS\] CONTAINER COMMAND \[ARG...\]

> Run a command in a running container。

* -d, --detach=false Detached mode: run command in the background，以后台模式运行。
* -i, --interactive=false Keep STDIN open even if not attached，开启STDIN
* --privileged=false Give extended privileges to the command，
* -t, --tty=false Allocate a pseudo-TTY，使用TTY模式。若要使用Bash，必须使用此项。
* -u, --user= Username or UID \(format: \[:\]\)

可以用于附加到守护进程的docker，执行命令。

eg:

```text
docker exec name echo “hello world”
docker exec name pwd
docker exec -it hello /bin/bash
docker exec -it redis /bin/bash #连接到redis容器。
```

## history

docker history \[OPTIONS\] IMAGE

> 查看历史
>
> * -H, --human=true Print sizes and dates in human readable format，
> * --no-trunc=false Don't truncate output，内容过长而省略。
> * -q, --quiet=false Only show numeric IDs，只显示镜像ID。

eg:

```text
docker history -H imgname
docker history -q imgname
```

## cp

docker cp \[OPTIONS\] CONTAINER:PATH LOCALPATH\|-

docker cp \[OPTIONS\] LOCALPATH\|- CONTAINER:PATH

> 从容器复制到主机。

```text
sudo docker cp networktest:/etc/ ./ # 将networktest的/etc目录复制到当前目录
```

## commit

docker commit \[OPTIONS\] CONTAINER \[REPOSITORY\[:TAG\]\]

> Create a new image from a container's changes
>
> * -a, --author= Author \(e.g., "John Hannibal Smith [hannibal@a-team.com](mailto:hannibal@a-team.com)"\)，作者
> * -c, --change=\[\] Apply Dockerfile instruction to the created image，
> * -m, --message= Commit message，消息
> * -p, --pause=true Pause container during commit，暂停当前容器

eg：

```text
docker commit -m “commint msg” -a “author” containername newname:tag
```

## diff

docker diff redis

> 比较正在运行的容器与原始镜像文件的变化。

A 添加 C修改 D删除。

## save

docker save \[OPTIONS\] IMAGE \[IMAGE …\]

> Save an image\(s\) to a tar archive \(streamed to STDOUT by default\)，将镜像存为本地文件
>
> * -o, --output= Write to a file, instead of STDOUT

eg：

```text
docker save -o ubuntu_14.04.tar ubuntu:14.04
sudo docker save -o redis_latest.rar pzdn2009/redis:latest
```

## load

docker load

> Load an image from a tar archive or STDIN，从本地文件导入到镜像库
>
> * -i, --input= Read from a tar archive file, instead of STDIN

eg：

```text
docker load --input ubuntu_14.04.tar
docker load < ubuntu_14.04.tar
docker load -i redis_latest.rar
```

## search

docker search \[OPTIONS\] TERM

> Search the **Docker Hub** for images
>
> * --automated=false Only show automated builds
> * --no-trunc=false Don't truncate output
> * -s, --stars=0 Only displays with at least x stars

eg:

```text
docker search -s 10 ubuntu
```

## export

docker export \[OPTIONS\] CONTAINER

> Export a container's filesystem as a tar archive，将容器打包，和save不同的是，save是对镜像打包。
>
> * -o, --output= Write to a file, instead of STDOUT

eg:

```text
docker run -itd --name hello ubuntu:14.04 /bin/bash
docke export hello > hello.tar
```

## import

docker import \[OPTIONS\] file\|URL\|- \[REPOSITORY\[:TAG\]\]

> Import the contents from a tarball to create a filesystem image，从文件、URL、标准输入、REPOSITORY\[:TAG\]导入。 如果是-，则表示STDIN。

* -c, --change=\[\] Apply Dockerfile instruction to the created image
* -m, --message= Set commit message for imported image

eg:

```text
docker import http://example.com/hello.tar.gz hello
cat hello.tar | docker import - hello
tar -c . | sudo docker import -hello
```

基于一个os模板文件导入。OpenVZ

```text
wget http://openvz.org/Download/template/precreated
cat ubuntu-14.04-x86_64-minimal.tar.gz | docker import -ubuntu:14.04
```

## tag

docker tag \[OPTIONS\] IMAGE\[:TAG\] \[REGISTRYHOST/\]\[USERNAME/\]NAME\[:TAG\]

> Tag an image into a repository，标记一个镜像到仓库。

* -f, --force=false Force

eg:

```text
docker tag hello:latest hello:0.1
docker tag hell0:latest DECO/hello:0.1
docker tag hello:latest ip/hello:0.1
```

## build

docker build \[OPTIONS\] PATH \| URL \| -

> Build an image from a Dockerfile，从Dockerfile构建镜像。
>
> * --build-arg=\[\] Set build-time variables
> * --cpu-shares=0 CPU shares \(relative weight\)
> * --cgroup-parent= Optional parent cgroup for the container
> * --cpu-period=0 Limit the CPU CFS \(Completely Fair Scheduler\) period
> * --cpu-quota=0 Limit the CPU CFS \(Completely Fair Scheduler\) quota
> * --cpuset-cpus= CPUs in which to allow execution \(0-3, 0,1\)
> * --cpuset-mems= MEMs in which to allow execution \(0-3, 0,1\)
> * --disable-content-trust=true Skip image verification
> * -f, --file= Name of the Dockerfile \(Default is 'PATH/Dockerfile'\)，指定Dockerfile
> * --force-rm=false Always remove intermediate containers
> * -m, --memory= Memory limit，内存限制
> * --memory-swap= Total memory \(memory + swap\), '-1' to disable swap，内存交换
> * --no-cache=false Do not use cache when building the image
> * --pull=false Always attempt to pull a newer version of the image
> * -q, --quiet=false Suppress the verbose output generated by the containers，静默模式
> * --rm=true Remove intermediate containers after a successful build，镜像创建成功时，删除临时容器
> * -t, --tag= Repository name \(and optionally a tag\) for the image，标记tag
> * --ulimit=\[\] Ulimit options

eg:

```text
docke build -t hello .
```

