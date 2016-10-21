# Docker 命令

## attach
docker attach <选项> <容器名称，ID>
>用于将stdin和stdout连接到一个正在运行的容器。

- --no-stdin=false Do not attach STDIN，不接入STDIN。
- --sig-proxy=true Proxy all received signals to the process，将所有信号传递给进程，但不传送SIGCHLD，SIGKILL，SIGSTOP信号。

经常使用的信号：
- SIGINT：interrupt信号，输入Ctr+c时发生。
- SIGQUIT：quit信号，输入Ctr+\时发生。
- EOF：终止信号，输入Ctr+d时发生。

## exec

docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
>Run a command in a running container。

- -d, --detach=false Detached mode: run command in the background，以后台模式运行。
- -i, --interactive=false Keep STDIN open even if not attached，开启STDIN
- --privileged=false Give extended privileges to the command，
- -t, --tty=false Allocate a pseudo-TTY，使用TTY模式。若要使用Bash，必须使用此项。
- -u, --user= Username or UID (format: <name|uid>[:<group|gid>])

可以用于附加到守护进程的docker，执行命令。

eg:
```
docker exec name echo “hello world”
docker exec name pwd
docker exec -it hello /bin/bash
docker exec -it redis /bin/bash #连接到redis容器。
```

## history

docker history [OPTIONS] IMAGE
>查看历史
- -H, --human=true Print sizes and dates in human readable format，
- --no-trunc=false Don't truncate output，内容过长而省略。
- -q, --quiet=false Only show numeric IDs，只显示镜像ID。

eg:
```
docker history -H imgname
docker history -q imgname
```
## cp

docker cp [OPTIONS] CONTAINER:PATH LOCALPATH|-

docker cp [OPTIONS] LOCALPATH|- CONTAINER:PATH
>从容器复制到主机。

```
sudo docker cp networktest:/etc/ ./ # 将networktest的/etc目录复制到当前目录
```
## commit

docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
>Create a new image from a container's changes
- -a, --author= Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")，作者
- -c, --change=[] Apply Dockerfile instruction to the created image，
- -m, --message= Commit message，消息
- -p, --pause=true Pause container during commit，暂停当前容器

eg：
```
docker commit -m “commint msg” -a “author” containername newname:tag
```

## diff

docker diff redis 
>比较正在运行的容器与原始镜像文件的变化。

A 添加 C修改 D删除。

## save

docker save [OPTIONS] IMAGE [IMAGE …]
>Save an image(s) to a tar archive (streamed to STDOUT by default)，将镜像存为本地文件
- -o, --output= Write to a file, instead of STDOUT

eg：
```
docker save -o ubuntu_14.04.tar ubuntu:14.04
sudo docker save -o redis_latest.rar pzdn2009/redis:latest
```

## load

docker load
>Load an image from a tar archive or STDIN，从本地文件导入到镜像库
- -i, --input= Read from a tar archive file, instead of STDIN

eg：
```
docker load --input ubuntu_14.04.tar
docker load < ubuntu_14.04.tar
docker load -i redis_latest.rar
```