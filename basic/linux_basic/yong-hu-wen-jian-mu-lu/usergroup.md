# User&Group

## User & Group

## 0. 核心文件与概念

* /etc/passwd

  账号:密码:UID:GID:注释:用户主目录:默认Shell

  ```text
  root:x:0:0:root:/root:/bin/bash
  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
  bin:x:2:2:bin:/bin:/usr/sbin/nologin
  pzdn:x:1000:1000:DotnetCore,,,:/home/pzdn:/bin/bash
  ```

* /etc/shadow

  账号:密码:更改日期:密码不可被改动的天数:重新更改的天数:警告天数:失效日:失效日期:保留

  ```text
  daemon:*:16365:0:99999:7:::
  bin:*:16365:0:99999:7:::
  pzdn:$1$8U/0HIzpPMY1ICVXvIY.dObw1:17030:0:99999:7:::
  ```

* /etc/group

  用户组名称:用户组密码:GID:此用户组支持的账号名称

  ```text
  root:x:0:
  daemon:x:1:
  bin:x:2:
  sambashare:x:125:pzdn
  ```

* /etc/gshadow

**初始用户组**：GID。initial group。用户一登录，就获得这个组的权限。

**有效用户组**：使用newgrp来切换，那么创建的文件所属的用户组就属于切换之后的组。

newgrp：用户的环境配置\(例如环境变量等等其他数据\)不会有影响，但是使用者的『用户组权限』将会重新被计算

eg:

```text
$ groups 
pzdn adm cdrom sudo dip plugdev lpadmin sambashare docker
$ newgrp docker 
$ mkdir tes2
$ ll tes2
drwxrwxr-x  2 pzdn pzdn   4096 Nov  5 04:07 tes/
drwxrwxr-x  2 pzdn docker 4096 Nov  5 04:08 tes2/
```

## 1. 用户User

**用户：查看，新增，修改，删除更改密码**

查看用户

```text
w
who
who am i 
whoami

last #show listing of last logged in users
lastlog #打印每个账号的最近登录时间

id #打印真实和有效用户、用户组IDs
```

创建用户\(Debian的管理员工具，比useradd更高级\)

```text
sudo adduser username
ls /home #view the newuser's home
su -l username #switch to the new user.
```

useradd——create a new user or update default new user information

默认行为：

1. 建立一个新目录作为家目录
2. 建立同名新组
3. 把用户的主要组设为该组\(除非命令选项覆盖以上默认动作，比如–disall-homdirecry之类\)
4. 从\/etc\/skel目录下拷贝文件到家目录，完成初始化
5. 建立新用户的密码
6. 如果其存在的话，还会执行一个脚本。

```text
useradd -D #查看useradd的默认值信息

useradd pzdn #按照默认值创建一个用户
grep pzdn | /etc/passwd /etc/shadow /etc/group

useradd -u 700 -g users pzdn2 #将pzdn2加到users组，且UID为700
grep pzdn2 | /etc/passwd /etc/shadow /etc/group

useradd -r pzdn3 #创建一个系统账户，不会创建主文件夹
```

passwd——change user passwd

```text
passwd #修改自己的密码
passwd username #修改他人的密码
```

usermod——modify a user account

```text
usermod -aG myGroup pzdn #将pzdn添加到myGroup
usermod -u 578 pzdnnew # 修改UID
usermod -g 100 pzdn#修改初始用户组
usermod -L pzdn #冻结账户
usermod -l pzdnnew pzdn #修改用户名
```

删除用户

```text
sudo deluser username --remove-home
userdel [-r] pzdn #删除用户，r表示连同主目录
```

## 2. 用户组Group

查看用户组

```text
groups pzdn
```

**用户组：新增，修改，删除**

groupadd——create a new group

```text
-g 指定GID
-r 新建_系统_用户组。

groupadd group1
grep group1 /etc/group etc/gshadow
```

groupmod——modify a group definition on the system

```text
-g GID
-n name

groupmod -g 201 -n mygroup group1
grep mygroup /etc/group /etc/gshadow
```

groupdel——delete a group

```text
groupdel mygroup
groupdel pzdn #如果有用户的话，则删除不了。
```

## 3. SU

**su**

su——change user id or become super user

```text
su #切换到root，以non-login shell方式登录
id #目录为pzdn
env | grep pzdn
exit

su - #以login shell 方式切换为root
env | grep root
exit

su -l pzdn #切换到pzdn
su - -c “head -n 3 /etc/shadow”
```

## 4. 其他管理工具

### 4.1 密码管理

chage——change user password expiry information，change aging，维护一个用户账户的秘密过期限制。

选项：

* -d LAST\_DAY，set date of last password change to LAST\_DAY
* -E EXPIRE\_DATE，set account expiration date to EXPIRE\_DATE
* -I DAY，密码过期后，账户被锁定前，不活动的天数。
* -l 显示账户的过期密码过期信息

```text
chage -l root #列出root的密码过期信息
chage -l pzdn #列出pzdn的密码过期信息
chage -d 0 pzdn # 下次登录必须修改密码
```

### 4.2 finger指纹

**finger** ——user information lookup program

```text
$ finger root
$ finger -l #显示当前登录的用户信息，多行显示
$ finger -s #显示当前登录的用户信息，单行显示
$ finger pzdn #查询指定用户的信息

Login: pzdn                       Name: DotnetCore
Directory: /home/pzdn                   Shell: /bin/bash
On since Sat Nov  5 04:23 (PDT) on pts/3 from 192.168.2.105
   7 seconds idle
No mail.
No Plan.
```

