

请实现一个函数用来匹配包括'.'和'*'的正则表达式。

模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。

在本题中，匹配是指字符串的所有字符匹配整个模式。

例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配。

样例
输入：

s="aa"
p="a*"

输出:true

动态规则
f(i,j)表示字符串a从i到结尾是否匹配字符串b从j到结尾
