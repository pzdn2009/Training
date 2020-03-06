# vim环境设置

~/.viminfo：记录了vim的操作历史，vim自动创建。 ~/.vimrc：配置文件，可以为当前用户做一个全局地配置。

常用配置

```text
vim ~/.vimrc
set all #查看全部配置
set nu #设置行号
set hlsearch #高亮泛白
set autoindent #自动缩进
set backspace=2 #可随时用backspace进行删除
set ruler #可显示最后一行的状态
set showmode #左下角那一行的状态
set tabstop=4 #tab缩进的字符数
```

**窗口拆分**

vim内的翻页：ctr+f，PageUp \| ctr+b，PageDown。

窗口拆分解决大文件比对。

```text
:sp{filename}，可以使用tab补全，即可对已有文件或者新文件进行拆分
窗口切换命令：
Ctr+w+j ，切换到下一个窗口。先按ctr,w，放开w，再按j或者down。
Ctr+w+k ，切换到上一个窗口。同上方式。
:q，退出当前窗口 k
```

**文档加密**

```text
vim -x file1 #进入加密模式，提示输入密码
:w newfileName #另存为一个加密文件，下次打开的时候就需要输入密码
```

多文件编辑

```text
vim 1.txt 2.txt #同时打开多个文件
:n #换到下一个文件
:N #换到上一个文件
:ls #列出文件
:files #列出文件
```

