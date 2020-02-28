# 文件IO

## 打开文件
open
```
int open(const char * name,int flags)；
int open(const char * name ,int flags , mode_t mode);
```
* flags：O_APPEND，O_SYNC，O_CREATE；
* 文件的所有者：effective uid，用户组id就比较复杂；
* 创建新文件的权限
  S_IRWXU[G|O]，S_IRUSR[GRP|OTH]，S_IWURS[GRP|OTH]，S_IXUSR[GRP|OTH]
O_CREATE的时候需要。
也可以直接给数字，如0644。

create
```
int create(const char *name, mode_t mode);
```
等价于`open(name,O_WRONLY|O_CREATE|O_TRUNC,mode)；//mode 为权限`

* 返回值
-1表示有异常，则需要对照错误代码

