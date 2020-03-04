# nohup

不挂断地运行命令。

**语法**：nohup Command \[ Arg ... \] \[ & \]

描述：nohup 命令运行由 Command 参数和任何相关的 Arg 参数指定的命令，忽略所有挂断（SIGHUP）信号。

如果你正在运行一个进程，而且你觉得在退出帐户时该进程还不会结束，那么可以使用nohup命令。该命令可以在你退出帐户/关闭终端之后继续运行相应的进程。nohup就是不挂起的意思\( no hang up\)。 退出状态：该命令返回下列出口值：

* 126 可以查找但不能调用 Command 参数指定的命令。
* 127 nohup 命令发生错误或不能查找由 Command 参数指定的命令。　　
* 否则，nohup 命令的退出状态是 Command 参数指定命令的退出状态。 　

如果使用nohup命令提交作业，那么在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件：

```text
nohup command > myout.file 2>&1 & #输出被重定向到myout.file文件中
```

```text
nohup bin/logstash -f redisconfig.conf & # 后台运行logstash
nohup bin/elasticsearch & # 后台运行elasticsearch
nohup bin/kibana & #后台运行kibana
jobs # 查看nohup命令运行的进程
fg %n # n为进程的序号（1,2,3...），输入以后可以关闭nohup命令
```

