# pid文件的作用

/var/run下面有很多\*.pid文件。而且往往新安装的程序在运行后也会在/var/run目录下面产生自己的pid文件。

那么这些pid文件有什么作用呢？它的内容又是什么呢？

1. pid文件的内容：pid文件为文本文件，内容只有一行, 记录了该进程的ID。

   用cat命令可以看到。

2. pid文件的作用：防止进程启动多个**副本**。只有获得pid文件\(固定路径固定文件名\)写入权限\(F\_WRLCK\)的进程才能正常启动并把自身的PID写入该文件中。

   其它同一个程序的多余进程则自动退出。

