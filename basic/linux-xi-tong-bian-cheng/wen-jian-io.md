# 文件IO

## 打开文件

open

```text
int open(const char * name,int flags)；
int open(const char * name ,int flags , mode_t mode);
```

* flags：O\_APPEND，O\_SYNC，O\_CREATE；
* 文件的所有者：effective uid，用户组id就比较复杂；
* 创建新文件的权限

  S\_IRWXU\[G\|O\]，S\_IRUSR\[GRP\|OTH\]，S\_IWURS\[GRP\|OTH\]，S\_IXUSR\[GRP\|OTH\]

  O\_CREATE的时候需要。

  也可以直接给数字，如0644。

create

```text
int create(const char *name, mode_t mode);
```

等价于`open(name,O_WRONLY|O_CREATE|O_TRUNC,mode)；//mode 为权限`

* 返回值

  -1表示有异常，则需要对照错误代码

