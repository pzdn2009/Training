# 进程

## 命令工具

**top**

```text
top #display linux processes
```

![](../../../.gitbook/assets/top.png)

> 详细解释：
>
> * line 1：top的名称 - 系统当前时间 开机时间，用户数量，CPU的平均负载（1min,5min,15min）
> * line 2：进程总数，正在运行数，睡眠数，停止数，僵尸数
> * line 3：us用户空间占比，sy内核空间占比，ni用户进程空间内改变过优先级的进程占比，id空闲CPU占比，wa等待输入输出占比，hi硬中断占比，si软中断占比，st虚拟CPU等待实际CPU占比
> * line 4：物流内存总数，已使用，空闲，内核缓存
> * line 5：交换去总量，已使用，空闲，缓冲交换区总量
> * line 7：PID进程ID，USER用户，PR优先级，NI该进程的nice值，VIRT虚拟内存总数，RES驻留内存，SHR共享内存，S状态S/R/Z，%CPU占比CPU，%MEM内存占比，TIME+活跃总时间，COMMAND进程名称

| top的交互命令 | 解释 |
| :--- | :--- |
| q | 退出程序 |
| I | 切换显示平均负载和启动时间的信息 |
| P | 根据CPU使用百分比大小进行排序 |
| M | 根据驻留内存大小进行排序 |
| i | 忽略闲置和僵死的进程，这是一个开关式命令 |
| k | 终止一个进程，系统提示输入 PID 及发送的信号值。一般终止进程用15信号，不能正常结束则使用9信号。安全模式下该命令被屏蔽。 |

**ps工具**

> ps \#report a snapshot of the current processes.

```text
ps aux #打印进程列表
ps aux | grep zsh #找到某进程
ps axjf #将进程名打印成目录名
ps -afxo user,ppid,pid,pgid,command #指定参数

ps -l #只打印bash相关的进程
```

**pstree工具**

> pstree \# display a tree of processes

```text
pstree -up #u:user p:pid
```

**kill工具**

> kill \#send a signal to a process

```text
kill -9 1233
```

