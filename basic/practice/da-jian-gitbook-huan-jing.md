# 搭建gitbook環境

```text
[pzdn@elkjz34 ~]$ docker pull hub.c.163.com/public/nodejs:5.7.0

[pzdn@elkjz34 ~]$ docker run -d -P --name gitbooknodejs hub.c.163.com/public/nodejs:5.7.0 

[pzdn@sunny ~]$ docker ps
CONTAINER ID        IMAGE                               COMMAND                  CREATED             STATUS                      PORTS                                                               NAMES
d8fa73de76eb        hub.c.163.com/public/nodejs:5.7.0   "/bin/sh -c '/usr/bin"   11 minutes ago      Up 8 minutes                0.0.0.0:32769->22/tcp                                               gitbooknodejs
0bf67989022d        common-gp-registry:9901/gitlab-ce   "/assets/wrapper"        3 months ago        Restarting (1) 2 days ago   0.0.0.0:9905->22/tcp, 0.0.0.0:9904->80/tcp, 0.0.0.0:9906->443/tcp   gitlab

[pzdn@elkjz34 ~]$ docker exec -it gitbooknodejs /bin/bash
```

```text
npm install gitbook-cli -g
cd ~
mkdir gitbook
cd gitbook
root@d8fa73de76eb:~/gitbook# node -v
v5.8.0

gitbook init

root@d8fa73de76eb:~/gitbook# gitbook build
info: 7 plugins are installed 
info: 6 explicitly listed 
info: loading plugin "highlight"... OK 
info: loading plugin "search"... OK 
info: loading plugin "lunr"... OK 
info: loading plugin "sharing"... OK 
info: loading plugin "fontsettings"... OK 
info: loading plugin "theme-default"... OK 
info: found 1 pages 
info: found 0 asset files 
info: >> generation finished with success in 0.9s ! 


root@d8fa73de76eb:~/gitbook# gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed 
info: loading plugin "livereload"... OK 
info: loading plugin "highlight"... OK 
info: loading plugin "search"... OK 
info: loading plugin "lunr"... OK 
info: loading plugin "sharing"... OK 
info: loading plugin "fontsettings"... OK 
info: loading plugin "theme-default"... OK 
info: found 1 pages 
info: found 0 asset files 
info: >> generation finished with success in 1.9s ! 

Starting server ...
Serving book on http://localhost:4000
```

