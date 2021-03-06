# 文本处理

## 1. tr

tr \[options\] \[string1 \[string2\]\]

> translate or delete characters.

* -d delete
* -s 去重

eg:

```text
tr -d '\r' < dos.txt # 删除 '\r'
echo 'hello' | tr -s 'l' #l去重
cat /etc/passwd | tr '[:lower:]' '[:upper:]' #小写转大写
cat /etc/passwd | tr a-z A-Z #小写转大写
cat file1 | tr -s '[:blank:]' #压缩file1中连续的空格
```

## 2. wc

> print newline, word, and byte counts for each file

* -c, --bytes，print the byte counts
* -m, --chars，print the character counts
* -l, --lines，print the newline counts
* -L, --max-line-length，print the length of the longest line
* -w, --words，print the word counts

eg:

```text
wc file[123]
wc -l file1
```

## 3. cut

> remove sections from each line of files

* -d指定分隔符
* -f 对应的字段序号

eg:

```text
cut -d: -f1 /etc/passwd
```

## 4. sort

> sort lines of text files

* -k 列的序号
* -n 以数值排序

eg:

```text
sort -k 2 name_list #按照第二列排序
ps aux | sort -k 6 -n
```

## 5.join

> join lines of two files on a common field

* -t 指定分隔符，默认为空格 
* -i 忽略大小写
* -1指明第一个文件要用哪个字段来对比，默认对比第一个字段
* -2指明第二个文件要用哪个字段来对比，默认对比第一个字段
* -j equivalent to '-1 FIELD -2 FIELD'

eg：

```text
join -j 1 file1 file2

pzdn@ubuntu:~$ echo '1 hello' >file1
pzdn@ubuntu:~$ echo '1 shiyanlou' >file2
pzdn@ubuntu:~$ join file1 file2
1 hello shiyanlou

sudo join -t':' -1 4 /etc/passwd -2 3 /etc/group
```

## 6. paste

> merge lines of files,它是在不对比数据的情况下，简单地将多个文件合并一起，以Tab隔开。

* -d 分隔符，默认为tab
* -s 将输入文件所有行合并为一行输出。有多个文件输入时，每个文件的内容输出为单独一行。

eg:

```text
paste file1 file2
paste -d’@’ file1 file2 
paste -s file1 file2

$ paste -d ':' file1 file2
1 hello:1 shiyanlou

$ paste -s file1 file2
1 hello
1 shiyanlou
```

## 7.uniq

> report or omit repeated lines

* -d 只显示重复的行
* -u 只显示不重复的行

eg:

```text
uniq file
sort file |uniq
sort file |uniq -u
sort file |uniq -d
```

## 8. col

> filter reverse line feeds from input

* -x tab转换为对等的空格键
* -h 将空格转为Tab（默认）

eg:

```text
# 查看 /etc/protocols 中的不可见字符，可以看到很多 ^I ，这其实就是 Tab 转义成可见字符的符号
$ cat -A /etc/protocols
# 使用 col -x 将 /etc/protocols 中的 Tab 转换为空格,然后再使用 cat 查看，你发现 ^I 不见了
$ cat /etc/protocols | col -x | cat -A

cat /etc/man.config | col -x | cat -A | more
```

## 9. expand

> convert tabs to spaces
>
> * -t 数字。代表一个tab用多少个空格代表，默认为8个空格。

eg:

```text
grep '^MANPATH' /etc/man.config | head -n 3 | expand -t 6 - | cat -A #替换为6个空格
```

## 10. unexpand

> convert spaces to tabs

* -t number。指定tab空白宽度。

