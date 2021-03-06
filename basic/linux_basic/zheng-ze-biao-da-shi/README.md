# 正则表达式

## 正则表达式

## 1. 基础介绍

**类别**

* 基本正则表达式：BRE，Basic regular expression，grep 
* 扩展正则表达式：ERE，Extended regular expression，egrep
* 快速正则表达式：FRE，Fast greo，fgrep 

**POSIX方括号表达式bracket expression**

**字符集**

1. \[:alnum:\]
2. \[:alpha:\]
3. \[:digit:\]
4. \[:lower:\]
5. \[:pirnt:\]
6. \[:blank:\]

**排序符号**

1. \[.ch.\]，则匹配时，ch作为一个整体进行匹配

**等价字符集**

1. \[=e=\]，如果在法语环境中，则可以匹配e的各种字符。

**后向引用backreference**

使用\(与\)包含分组模式，使用\digit来引用。

Eg:

```text
\(ab\)\(cd\)[def]*\2\1
```

> \1代表\(ab\)，\2代表\(cd\)，以此类推。那么替换之后表示\(ab\)\(cd\)\[def\]\* \(cd\)\(ab\)，那么含义就是匹配abcd开头，cdab结尾，中间是def任意字符的闭包的字符串。

