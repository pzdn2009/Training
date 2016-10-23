# Shell

**什么是Bash？**

Bash（GNU Bourne-Again Shell）是一个命令解释器。 

**特殊符号**

- \# 注释
- ; 分号，写多个命令
- ;; 双分号，结束case选项。
- . 等价于source
- “ 双引号，可以有变量，可以有转义字符
- ‘ 单引号，单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的
- / 文件路径
- \ 转义
` 反引号，优先执行
- : NOP，或者true，while : 等价于while true，if then:中做占位符
- $ 变量替换，命令替换
- () 命令组，在括号中的命令列表，将会作为一个子shell来执行；用来初始化数组
- {} t.{txt,bak}
- [] 测试表达式；数组访问
- <> 重定向，&>，
- | 管道
- ~ 表示Home目录

**退出状态**

每一条命令，不管是内置的、Shell函数，还是外部的，当它退出时，都会返回一个小的整数值给引用它的程序。即退出状态exit status。


0表示成功，其他都表示失败。

使用$?来获取上一次退出状态。

$ bash #启动一个子shell。



# 1. 变量

$variable，如“$hello”

‘$hello’使用单引号引用变量时候，变量的值不会被引用，只是简单的保持原始字符串

readonly url #只读变量

unset variable_name #删除变量，不能被再次使用，unset不能删除只读变量

**Prompt Sign**
```
echo $PS1 #\u@\h:\w\$
echo $PS2 # >
```
>\u,user　\h,host　\w,workspace　\d,date　\$，提示符（root我#，普通用户为$）

**命令替换**
```
$(command)
`command` #后置引用方式
sys=$(uname -a)
echo $sys

```

# 2.测试

运算符 |如果。。。则为真
--|--
string|string不是null
-b file|file是块设备文件
-c file|file是字符设备文件
-d file|file是目录
-e file|file存在
-f file|file为一般文件
-g file|file有setgid位
-h file|file是一符号链接
-n string|string非null
-p file|file是管道文件
-r file|file可读
-S file|file是socket文件
-s file|file不是空的
-t n|文件描述符n指向一终端
-u file|file有setuid位
-w file|file可写
-x file|file可执行
-z string|string为null
s1 = s2|两字符串相同
s1 != s2|不相同
n1 -eq n2|两整数相等
n1 -ne n2|不等
n1 -lt n2|小于
n1 -gt n2|大于
n1 -le n2|小于等于
n1 -ge n2|大于等于

