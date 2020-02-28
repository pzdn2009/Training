# Mount原理和命令

## 原理

在 Linux 看来，任何硬件设备也都是文件，它们各有自己的一套文件系统（文件目录结构）。

当在 Linux 系统中使用这些硬件设备时，只有将Linux本身的文件目录与硬件设备的文件目录合二为一，硬件设备才能为我们所用。合二为一的过程称为“挂载”。

挂载点提供了一个其他文件系统打开的入口。

作为挂载点的目录通常不包含任何文件和子目录。它们只是一个空目录，建立的目录只为挂载文件系统。

### 文件类型

fstype：
* 光盘或光盘镜像：**iso9660**
* DOS fat16文件系统：msdos
* Windows 9x fat32文件系统：vfat
* Windows NT ntfs文件系统：ntfs
* Mount Windows文件网络共享：smbfs
* UNIX(LINUX) 文件网络共享：**nfs**


## mount命令

含义：mount a filesystem

格式：
* mount [-lhV]
* mount -a [-fFnrsvw] [-t vfstype] [-O optlist]
* mount [-fnrsvw] [-o option[,option]...]  device|dir
* mount [-fnrsvw] [-t vfstype] [-o options] device dir

Options：
* -r 以只读的方式挂载
* -w 以可读写的方式挂载
* -o 如下。
* -t fstype，文件类型


-o mount_options：
* loop：用来把一个文件**当成硬盘分区**挂接上系统
* ro：采用只读方式挂接设备
* rw：采用读写方式挂接设备

解释：

All files accessible in a Unix system are arranged in one big tree, the file hierarchy, rooted at /.  These files can be spread out over several devices. The mount command serves to attach the filesystem found on some device to the big file tree. Conversely, the umount(8) command will detach it again.

实践：
* 打印挂载信息，列信息
* 将/dev/hdc只读挂载到/mnt/cdrom下，格式为iso9960
* 卸载所有的NFS文件系统

```sh
mount
mount -rt iso9660 /dev/hdc /mnt/cdrom
umount -at nfs
```

## 示例

```sh
[root@localhost ~]# mount
#查看系统中已经挂载的文件系统，注意有虚拟文件系统
/dev/sda3 on / type ext4 (rw)  <--含义是，将 /dev/sda3 分区挂载到了 / 目录上，文件系统是 ext4，具有读写权限
proc on /proc type proc (rw)
sysfe on /sys type sysfs (rw)
devpts on /dev/pts type devpts (rw, gid=5, mode=620)
tmpfs on /dev/shm type tmpfs (rw)
/dev/sda1 on /boot type ext4 (rw)
none on /proc/sys/fe/binfmt_misc type binfmt_misc (rw)
sunrpc on /var/lib/nfe/rpc_pipefs type rpc_pipefs (rw)
```


