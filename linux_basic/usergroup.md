#  User & Group

**用户：查看，新增，修改，删除更改密码**

查看用户

```
w
who
who am i 
whoami

last #show listing of last logged in users
lastlog #打印每个账号的最近登录时间

id #打印真实和有效用户、用户组IDs
```

创建用户\(Debian的管理员工具，比useradd更高级\)

```
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

```
useradd -D #查看useradd的默认值信息

useradd pzdn #按照默认值创建一个用户
grep pzdn | /etc/passwd /etc/shadow /etc/group

useradd -u 700 -g users pzdn2 #将pzdn2加到users组，且UID为700
grep pzdn2 | /etc/passwd /etc/shadow /etc/group

useradd -r pzdn3 #创建一个系统账户，不会创建主文件夹

```

passwd——change user passwd

```
passwd #修改自己的密码
passwd username #修改他人的密码
```

usermod——modify a user account

```
usermod -aG myGroup pzdn #将pzdn添加到myGroup
usermod -u 578 pzdnnew # 修改UID
usermod -g 100 pzdn#修改初始用户组
usermod -L pzdn #冻结账户
usermod -l pzdnnew pzdn #修改用户名
```

删除用户

```
sudo deluser username --remove-home
userdel [-r] pzdn #删除用户，r表示连同主目录
```

查看用户组

```
groups pzdn
```

**用户组：新增，修改，删除**

groupadd——create a new group

```
-g 指定GID
-r 新建_系统_用户组。

groupadd group1
grep group1 /etc/group etc/gshadow
```

groupmod——modify a group definition on the system

```
-g GID
-n name

groupmod -g 201 -n mygroup group1
grep mygroup /etc/group /etc/gshadow
```

groupdel——delete a group

```
groupdel mygroup
groupdel pzdn #如果有用户的话，则删除不了。

```

**su**

su——change user id or become super user

```
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

