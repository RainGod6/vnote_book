# 08-search方法的使用




## search方法

search在一个字符串中搜索满足文本模式的字符串。语法格式如下：

```
re.search(pattern,string,flags=0)
```

函数参数与match方法类似，如下表所示：

| 参数     | 描述           |
| :------ | :------------- |
| pattern | 匹配的正则表达式 |
| string  | 要匹配的字符串   |
| flags   |    标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等            |



案例：search方法的使用


```
import re
pattern = 'hello'
s = 'hello python'
m = re.match(pattern, s)
m = re.search(pattern, s)
print(m)
print(m.group())
```


## match与search的区别


re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配识别，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。


案例：测试mathc和search的区别

```
pattern = 'love'
s = 'I love you'
m = re.match(pattern, s)
print("使用match进行匹配：", m)
o = re.search(pattern, s)
print("使用search进行匹配：", o)
```

运行结果如下：

使用match进行匹配： None
使用search进行匹配： <re.Match object; span=(2, 6), match='love'>


## 匹配多个字符串

search方法搜索一个字符串，要想搜索多个字符串，如搜索aa、bb和cc,最简单的方法是在文本模式中使用择一匹配符号（|）。择一匹配符和逻辑或类似，只要满足任何一个，就是匹配成功。



案例：

```
import re

pattern = 'aa|bb|cc'
s = 'aa'
s = 'bb'
s = 'cc'
o = re.match(pattern, s)
print(o)

s = 'my name is cc'
o = re.search(pattern, s)
print(o)

# 匹配0-100之间所有的数字 0-99 | 100

pattern = r'[1-9]?\d$|100$'
s = '1' 
s = '11'
s = '0'
s = '100'
s = '99'
s = '101'  # 返回None

o = re.match(pattern, s)
print(o)
```

