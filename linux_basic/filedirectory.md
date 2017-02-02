# File & Directory

**文件权限——drwxrwxrwx**

第一位表示文件类型，d directory ,- file ,l link,b block device,c charset device,s socket,p pipline。

r:read,w:write,x:execute。

**权限的意义**

* 文件：read表示是否可以读取此文件，write指写入、编辑、新增、修改，但是不代表删除，execute是运行文件。
* 目录：read表示是否具有权限来读取目录结构，write指新建文件和目录，**删除**文件和目录，重命名，移动，execute代表是否能够进入此目录，即使用cd能够跳转到。

修改用户组

```
chgrp [-R] GROUP FILE #-R表示递归，GROUP组名，FILE文件名
chgrp testing install.log #将install.log文件的组改为testing 
```

修改文件所有者

```
chown [-R] [OWNER][:[GROUP]] FILE#-R表示递归，OWNER文件所有者，GROUP组名，FILE文件名
sudo chown username filename
chown bin install.log #改到bin组
chown root:root install.log #改到root用户，root组
```

修改文件权限

```
chmod 700 FILE #改为rdx------，使用三位二进制表示的权限组合111(rwx)，110(rx-)，101（r-x），100（r--），0(---)
chmod go-rw iphone # g:group,o:other,u:user,使用guo来代表三个用户组，使用+/-来添加删除权限。
```

**目录**

```
tree / #显示目录树
```

**目录解释**

\/bin

> 重要执行文件，比如bash,cat,chmod,date,mkdir等。

\/etc

> FHS中属于不变的，且不可分享的。
> 系统主要的配置文件几乎都放置在这个目录中。比如\/etc\/password，\/etc\/group，\/etc\/shadow等。FHS建议不要放置可执行文件在这个目录中。其中，重要的目录：
> 
> * \/etc\/init.d\/：所有服务的默认启动脚本都是放在这里的。
> * \/etc\/xinetd.d\/：super daemon。
> * \/etc\/X11：X Window有关的配置文件。

\/usr

>Unix system resource,可分享不可以变动
* /usr/bin 绝大部分的命令都放在这里
* /usr/lib 动态链接库
* /usr/locale 本地软件的安装目录，建议放在此，比较容易管理
* /usr/local/bin 本地用户安装命令，比如redis-server命令


$PATH的作用就是提供命令搜索路径:

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games

绝对路径以\/开头；相对路径：.为当前目录，..上级目录，省略了\/默认从当前目录开始

```
pwd #当前工作路径
cd .. #上级路径
cd ~  #home
cd -  #上一次work directory
```

**基本操作**

新建文件

```
touch test
```

新建目录

```
mkdir mydir
mkdir -p father/son/grandson #创建parent路径
```

复制文件

```
cp test father/son/grandson
```

复制目录

```
cp -r father family #r递归
```

删除文件

```
rm test
rm -f test #f强制
rm -i family #interactive
```

删除目录

```
rm -r family
```

移动文件

```
mv file1 Documents
mv -i file1 Documents #interactive
mv -u file1 Documents #源比目标更新，或者目标丢失的时候才移动

```

重命名文件

```
mv file1 myfile
rename ‘s/\.txt/\.c/’ *.txt
rename ‘y/a-z/A-Z/’ *.c
```

查看文件

```
cp /etd/passwd .
cat passwd
cat -n passwd #n行数
nl -b a file
more passwd
less passwd
tail /etc/passwd
tail -n 1 /etc/passwd
```

查看文件类型

```
file /bin/ls
```

**od**——dump file in octal and other formats
```
echo abcdef g > tmp
cat tmp

od -b tmp #单字节八进制
od -t o1 tmp

od -c tmp #字符
od -t c tmp

od -A d -c tmp #偏移量计数为十进制
od -A d -t c tmp

od -j 2 -c tmp #跳过两字节
od -j2 -t c tmp

od -N 2 -j 2 -c tmp #跳过两字节，长度为两字节
od -N 2 -j 2 -t c tmp

od -w1 -c tmp #字符显示宽度
od -w2 -c tmp
od -w3 -c tmp

od -t oCc tmp #八进制和字符同时显示
od -t o1c tmp

od -t oCcXc tmp #字符，八进制，十六进制
```

**touch**——change file timestamps

touch [-acdmt]... FILE...

更新文件的_访问时间_和_修改时间_为当前时间。

- -a 仅修改访问时间
- -m 仅修改修改时间
- -c 不创建文件
- -d 时间字符串
- -t [CC]YYMMDDhhmm[.ss]格式代替当前时间

> * mtime：文件的数据内容修改时间。
* ctime：文件状态（属性）的修改时间。
* atime：当“文件内容被取用”的时间，如cat。

示例：
```
ls -l /etc/manpath.config
ls -l --time=atime /etc/manpath.config
ls -l --time=ctime /etc/manpath.config

cd ~/code/shell
cp -a ~/.bashrc bashrc
ll bashrc;ll --time=atime bashrc;ll --time=ctime=bashrc

touch -d "2 days ago" bashrc
ll bashrc;ll --time=atime bashrc;ll --time=ctime=bashrc

touch -t 0709150202 bashrc
ll bashrc;ll --time=atime bashrc;ll --time=ctime=bashrc
```

**ln**——make links between files

ln 主要用于在两个文件中创建链接，链接又分为 Hard Links (硬链接)和 Symbolic Links (符号链接或软链接)，其中默认为创建硬链接，使用 -s 参数指定创建软链接。

- 硬链接主要是增加一个文件的链接数，只要该文件的链接数不为 0 ，该文件就不会被物理删除，所以删除一个具有多个硬链接数的文件，必须删除所有它的硬链接才可删除。
- 软链接简单来说是为文件创建了一个类似快捷方式的东西，通过该链接可以访问文件，修改文件，但不会增加该文件的链接数，删除一个软链接并不会删除源文件，即使源文件被删除，软链接也存在，当重新创建一个同名的源文件，该软链接则指向新创建的文件。
- 硬链接只可链接文件，不可链接目录，而软链接可链接目录，所以软链接是非常灵活的

```
ln [-sf] 源文件 目标文件

ln -s /etc/crontab crontab2
ll -i /etc/crontab crontab2

Result：
1114589 lrwxrwxrwx 1 pzdn pzdn 12 Sep 30 07:30 crontab2 -> /etc/crontab
262290 -rw-r--r-- 2 root root 722 Jul 7 2014 /etc/crontab

ln /etc/crontab .
ll -i /etc/crontab crontab #输出是一致的。
```

**umask**——set file mode creation mask

默认情况下，用户创建文件的最大权限：rw-rw-rw- 666，创建文件夹的最大权限为rwxrwxrwx777。

umask表示当前用户创建文件或文件夹要拿掉的分数。后三位分别对应user,group,other需要减掉（用默认666或777来减）的值。分数的计算是使用4,2,1的组合来算的。
```
umask # 0002，即创建文件或目录去掉other的w
umask -S #符号输出具有的权限。
```

