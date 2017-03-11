# 变量

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

