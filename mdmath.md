# MarkDown Math

ref:https://www.zybuluo.com/codeep/note/163962?utm_source=tuicool&utm_medium=referral#cmd-markdown-公式指导手册

# 1. 插入公式

$$ $$

# 2. 插入上下标

^ 表示上标, _ 表示下标。

如果上下标的内容多于一个字符，需要用 {} 将这些内容括成一个整体。

上下标可以嵌套，也可以同时使用。

\$\$ x^{y^z}=(1+{\rm e}^x)^{-2xy^w} \$\$


$$ x^{y^z}=(1+{e}^x)^{-2xy^w} $$

# 3. 如何输入括号和分隔符

()、[] 和 | 表示符号本身，使用 \{\} 来表示 {} 。

当要显示大号的括号或分隔符时，要用 \left 和 \right 命令：
$$ \left(...\right) $$


一些特殊的括号：

输入|显示|输入|显示
--|--|--|--
\langle |$$\langle$$|\rangle|$$\rangle$$
\lceil| $$ \lceil $$ |\rceil	|$$ \rceil $$
\lfloor| $$ \lfloor $$|\rfloor| $$ \rfloor $$
\lbrace|$$ \lbrace $$|\rbrace|$$ \rbrace $$

f(x,y,z) = 3y^2z \left( 3+ \frac{7x+5}{1+ y^2} \right)

$$ f(x,y,z) = 3y^2z \left( 3+ \frac{7x+5}{1+ y^2} \right) $$

# 4. 如何输入分数

通常使用 \frac {分子} {分母} 命令产生一个分数，分数可嵌套。 

便捷情况可直接输入 \frac ab 来快速生成一个  。 

如果分式很复杂，亦可使用 分子 \over 分母 命令，此时分数仅有一层。

\frac{a-1}{b-1} \quad and \quad {a+1\over b+1}

$$ \frac{a-1}{b-1} \quad and \quad {a+1\over b+1} $$

# 5. 如何开平方

使用 \sqrt [根指数，省略时为2] {被开方数} 命令输入开方。

\sqrt{2} \quad and \quad \sqrt[n]{3}

$$ \sqrt{2} \quad and \quad \sqrt[n]{3} $$

# 6. 如何输入省略号

数学公式中常见的省略号有两种，\ldots 表示与文本底线对齐的省略号，\cdots 表示与文本中线对齐的省略号。

f(x_1,x_2,\underbrace{\ldots}_{\rm ldots} ,x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}_{\rm cdots} + x_n^2

$$f(x_1,x_2,\underbrace{\ldots}_{ ldots} ,x_n) = x_1^2 + x_2^2 + \underbrace{\cdots}_{ cdots} + x_n^2$$

# 7. 如何输入矢量

使用 \vec{矢量} 来自动产生一个矢量。也可以使用 \overrightarrow 等命令自定义字母上方的符号。

\vec{a} \cdot \vec{b} = 0

$$ \vec{a} \cdot \vec{b} = 0 $$

\overleftarrow{xy} \quad and \quad \overleftrightarrow{xy} \quad and \quad \overrightarrow{xy}

$$\overleftarrow{xy} \quad and \quad \overleftrightarrow{xy} \quad and \quad \overrightarrow{xy}$$

# 8. 如何输入积分

使用 \int_积分下限^积分上限 {被积表达式} 来输入一个积分。

\int_0^1 {x^2} \,{\rm d}x 

$$ \int_0^1 {x^2} \,{d}x $$

# 9. 如何 输入极限运算

使用 \lim_{变量 \to 表达式} 表达式 来输入一个极限。如有需求，可以更改 \to 符号至任意符号。

\lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim_{x\leftarrow{示例}} \frac{1}{n(n+1)}

$$ \lim_{n \to +\infty} \frac{1}{n(n+1)} \quad and \quad \lim_{x\leftarrow{示例}} \frac{1}{n(n+1)} $$


# 10. 如何输入累加、累乘运算

使用 \sum_{下标表达式}^{上标表达式} {累加表达式} 来输入一个累加。 
与之类似，使用 \prod \bigcup \bigcap 来分别输入累乘、并集和交集。 
此类符号在行内显示时上下标表达式将会移至右上角和右下角。

\sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R

$$\sum_{i=1}^n \frac{1}{i^2} \quad and \quad \prod_{i=1}^n \frac{1}{i^2} \quad and \quad \bigcup_{i=1}^{2} R$$

# 11．如何输入希腊字母

输入 \小写希腊字母英文全称 和 \首字母大写希腊字母英文全称 来分别输入小写和大写希腊字母。 

对于大写希腊字母与现有字母相同的，直接输入大写字母即可。

# 12．如何输入其它特殊字符

## 12.1  关系运算符

输入|显示|输入|显示|输入|显示|输入|显示
--|--|--|--|--|--|--|--
\pm|$$\pm$$|\times|$$\times$$|\div|$$\div$$|\mid|$$\mid$$

## 12.2  集合运算符

## 12.3  对数运算符
## 12.4  三角运算符
## 12.5  微积分运算符
## 12.6  逻辑运算符



