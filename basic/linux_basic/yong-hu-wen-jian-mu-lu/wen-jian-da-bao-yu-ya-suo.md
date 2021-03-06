# 文件打包与压缩

打包就是把若干文件或文件夹放到一个tar文件中，但是不会压缩文件大小。

压缩就是在打包的基础上压缩文件的大小。

zip压缩

```text
zip -r -q -o shiyanlou.zip /home/shiyanlou
du -h shiyanlou.zip
file shiyanlou.zip

zip -r -9 -q -o shiyanlou_9.zip /home/shiyanlou -x ~/*.zip
zip -r -1 -q -o shiyanlou_1.zip /home/shiyanlou -x ~/*.zip
zip -r -e -o shiyanlou_encryption.zip /home/shiyanlou
zip -r -l -o shiyanlou.zip /home/shiyanlou
```

参数解释：r递归，q静默， o输出，9最大压缩级别，1最小压缩级别，e加密，x排除，l兼容Windows系统

unzip

```text
unzip shiyanlou.zip
unzip -q shiyanlou.zip -d ziptest
unzip -l shiyanlou.zip #只查看内容，不解压
unzip -O GBK 中文压缩文件.zip
```

rar

```text
rar a shiyanlou.rar .  #
rar d shiyanlou.rar .zshrc #d 删除其中的文件
rar l shiyanlou.rar #l 列出

unrar x shiyanlou.rar

mkdir tmp
unrar e shiyanlou.rar tmp/
```

tar

```text
tar -cf shiyanlou.tar ~ #将当前目录打包到shiyanlou.rar

mkdir tardir
tar -xf shiyanlou.tar -C tardir #解包
tar -tf shiyanlou.tar #查看包
tar -czf shiyanlou.tar.gz ~ #指定压缩方式
tar -xzf shiyanlou.tar.gz #指定解压方式
```

> 参数解释：c创建一个打包文件，f指定文件名，v可视化，x解包，C已存在的目录，t只查看不解包，z使用GZip来压缩文件。
>
> 参数分组：\[c\|x\|t\]只能选1，\[z\|j\]只能选1，v可选，f放在最后，之后可以接上目录名。

```text
tar -zcv -f filename.tar.gz dir/file #压缩
tar -ztv -f filename.tar.gz #查询
tar -zxv -f filename.tar.gz -C dir #解压
```

**gzip,gunzip**

```text
-d：表示解压
-v：verbose
-t：testgzip -c filename > filename.gz #压缩并保留原文件
gunzip -c filename.gz > filename #解压并保留原文件
gzip -c -d filename.gz >filename #同上
```

> 注：gunzip是个使用广泛的解压缩程序，它用于解开被gzip压缩过的文件，这些压缩文件预设最后的扩展名为“.gz”。事实上，gunzip就是gzip的硬连接，因此不论是压缩或解压缩，都可通过gzip指令单独完成。
>
> gzip可以处理compress，zip等。

