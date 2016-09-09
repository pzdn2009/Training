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


# 变量

$variable，如“$hello”

‘$hello’使用单引号引用变量时候，变量的值不会被引用，只是简单的保持原始字符串

readonly url #只读变量

unset variable_name #删除变量，不能被再次使用，unset不能删除只读变量

