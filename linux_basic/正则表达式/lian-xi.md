# 练习

Ref:https://www.cnblogs.com/tsw1107/p/2264a01aeec481d2044dfeda01417c64.html

1. 显示/proc/meminfo文件中以大写或小写S开头的行。

  `egrep -i "^s" /proc/meminfo`。
2. 显示/etc/passwd文件中其默认shell为非/sbin/nologin的用户

   `egrep -v "sbin/nologin" /etc/passwd`
3. 显示/etc/passwd文件中其默认shell为/bin/bash的用户中ID号最大的用户
   
   `egrep -v "sbin/nologin" /etc/passwd | sort -t: -k3 -n | tail -1 | cut -d: -f1`
4. 找出/etc/passwd文件中的一位数或两位数

   ``
5. 显示/boot/grub/grub.conf中以至少一个空白字符开头的行

6. 显示/etc/rc.d/rc.sysinit文件中，以#开头，后面跟至少一个空白字符，而后又有至少一个非空白字符的行

7. 找出netstat -tan命令执行结果中以'LISTEN'结尾的行

8. 找出当前系统上其用户名和默认shell相同的用户

9. 显示当前系统上root或scholar用户的默认shell12

10. 找出/etc/rc.d/init.d/functions文件中某单词后跟一组小括号“()”行

11. 使用echo命令输出一个路径，而后使用grep取出其基名

12. 找出ifconfig命令结果中的1-255之间的数字
