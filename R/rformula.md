# RFormula
R中公式的基本格式：

response variable ~ explanatory variables 

响应变量~解释变量。

~读作"is modeled by" or "is modeled as a function of." 

符号列表：


symbol |example|meaning
--|--|--
+|+ x| 包含变量，分隔预测变量
-|- x|delete this variable
:|x : z|include the interaction between these variables
*|x * z|include these variables and the interactions between them
/|x / z|nesting: include z nested within x
\| | \|x | \| z	conditioning: include x given z
^|(u + v + w + z)^3|include these variables and all interactions up to three way
poly|poly(x,3)|	polynomial regression: orthogonal polynomials
Error|Error(a/b)|specify an error term
I|I(x*z)|as is: include a new variable consisting of these variables multiplied;I(x^2) means include this variable squared, etc. In other words .I( ) isolates the mathematic operations inside it.
1|- 1|intercept: delete the intercept (regress through the origin)

