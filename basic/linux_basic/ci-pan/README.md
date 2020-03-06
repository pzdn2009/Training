# 磁盘

## 磁盘

* 硬件知识
* os相关
* 相关命令

### 1.硬件知识

**硬盘的组成**

> 硬盘：盘片，机械手臂，磁头，主轴马达。
>
> 盘片：扇区Sector，磁道Track，柱面（Cylinder）。
>
> > 磁道：当磁盘旋转时，磁头若保持在一个位置上，则每个磁头都会在磁盘表面划出一个圆形轨迹，这些圆形轨迹就叫做磁道。
> >
> > 扇区：磁盘上的每个磁道被等分为若干个弧段，这些弧段便是磁盘的扇区，每个扇区可以存放512个字节的信息，磁盘驱动器在向磁盘读取和写入数据时，要以扇区为单位。
> >
> > 柱面：硬盘通常由重叠的一组盘片构成，每个盘面都被划分为数目相等的磁道，并从外缘的“0”开始编号，具有相同编号的磁道形成一个圆柱，称之为磁盘的柱面。
>
> 硬盘的容量= 柱面数\*磁头数\*扇区数\*512B 即：=Head\*Cylinder\*Sector\*512

**IDE，SATA**

parellel ATA，一根四芯的电源线和一根80芯的数据线与主板相连接； Serial ATA，串口硬盘。

### 2.os相关

#### 2.1 Ext2文件系统

Linux second Extended file system，Ext2fs。

Ext2与FAT

> Ext2结构：基于块组的树形存储。
>
> U盘的FAT，单链表结构。

Ext2的组成——基于inode，由启动扇区 + 块组构成。

_Block Group块组 = Super Block + File System Description + Block Bitmap + Inode Bitmap + Inode Table + Data Block。_ 

**Super Block超级块**：

一个文件系统可能就第一个Block Group有超级块。

* 记录block与inode的总量
* 未使用与已使用的inode/block数量
* block与inode的大小（block为1K，2K，4K，inode为128bytes）
* 文件系统的挂载时间、最近一次写入数据的时间、最近一次检验磁盘（fsck）的时间等文件系统的相关信息
* 一个validbit数值，若此文件系统已被挂载，则valid bit为0，反之，为1

**File System Description文件系统描述说明**：

记录块组的开始和结束的block号码，使用使用dumpe2fs命令查看。

**Block Bitmap块地图**：

管理block。新增，释放。

**Inode Bitmap inode地图**：

管理inode的使用情况。

**Inodetable inode表**：

记录的数据有

* 文件的访问模式rwx
* 文件的所有者和组owner/group
* 文件的大小
* 文件的创建时间或状态改变时间ctime
* 最近一次的读取时间atime
* 最近修改时间mtime
* 文件特性标志，如SetUID
* 文件真正内容指针Pointer

inode特性

* 每个inode大小均固定为128bytes
* 每个文件仅会占用一个inode而已
* 文件系统能够创建的文件与inode的数量有关
* 读取文件时，先从inode判断权限，用户等信息，决定是否可能读取文件内容

**Data Block数据块**：

Ext2文件系统支持的block大小为1K，2K，4K。格式化的时候就固定好了，且每个block都有编号。

* block的大小与数量在格式化完毕就不变了，除非重新格式化
* 每个block内最多放置一个文件的数据
* 如果文件大于block的容量，则一个文件会占用多个block
* 如果文件小于block的容量，则剩余的空间再也不能用了 

## 3. 磁盘相关命令

* df
* du

查看磁盘和目录容量

```text
df -h
du -h
du -h -d 0 ~
du -h -d 1 ~
```

挂载与卸载挂载

用户在 Linux/UNIX 的机器上打开一个文件以前，包含该文件的文件系统必须先进行挂载的动作。

```text
sudo mount #打印挂载信息，名称 on 位置 type 文件类型 (挂载选项)
mount -o loop -t ext4 virtual.img /mnt
sudo umount /mnt
```

