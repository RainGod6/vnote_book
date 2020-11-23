# 02-match方法的使用


## match方法


re.match尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，mathc()就返回None。语法格式如下：

```
re.match(pattern,string,flags=0)
```

函数参数说明如下表所示：

| 参数     | 描述                                                                                         |
| :------ | :------------------------------------------------------------------------------------------- |
| pattern | 匹配的正则表达式                                                                               |
| string  | 要匹配的字符串                                                                                 |
| flags   | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。如下表列出正则表达式修饰符-可选标志 |



**正则表达式修饰符-可选标志**

| 修饰符 | 描述                                                 |
| :---- | :-------------------------------------------------- |
| re.I  | 使匹配对大小写不敏感                                   |
| re.L  | 做本地化识别（local-aware）匹配                        |
| re.M  | 多行匹配，影响^和$                                     |
| re.S  | 使.匹配包括换行在内的所有字符                           |
| re.U  | 根据Unicode字符集解析字符。这个标志影响\w，\W,\b,\B      |
| re.X  | 该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解 |


匹配字符串是正则表达式中最长用的一类应用。也就是设定一个文本模式，然后判断另外一恶搞字符串是否符号这个文本模式。

如果文本模式只是一个普通的字符串，那么待匹配的字符串和文本模式字符串在完全相等的情况下，match方法会认为匹配成功。如果匹配成功，则match方法返回匹配的对象，然后可以调用对象中的group方法获取匹配成功的字符串，如果文本模式就是一个普通的字符串，那么group方法返回的就是文本模式的字符串本身。


案例一：mathc方法的使用

```
import re

s = 'hello python'
pattern = 'hello'
o = re.match(pattern, s, re.I)
print(o)
print(o.group())  # 返回匹配的字符串
print(o.span())   # 返回匹配的范围
print(o.start())  # 返回匹配的起始位置

print("flags参数的使用")
pattern = 'Hello'
o = re.match(pattern, s, re.I)  # re.I 匹配不区分大小写
print(o)
print(o.group())

```

运行结果：

<re.Match object; span=(0, 5), match='hello'>
hello
(0, 5)
0
flags参数的使用
<re.Match object; span=(0, 5), match='hello'>
hello