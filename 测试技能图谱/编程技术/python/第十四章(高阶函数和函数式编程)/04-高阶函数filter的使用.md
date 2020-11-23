# 04-高阶函数filter的使用

    
   python内建的filter()函数用于过滤序列。和map()类似，filter()也接收一个函数和一个序列。和map()不同的是，filter()把传入的函数依此作用于每个函数，然后根据返回值是True还是False决定保留还是丢弃该元素。



案例一： 过滤偶数，只留下奇数

```
a = [1, 2, 3, 4, 5, 6, 7, 8, 9]


def is_num(n):
    return n % 2 == 1


print(list(filter(is_num, a)))
```
运行结果：

[1, 3, 5, 7, 9]



案例二：删除一个序列的空字符串

```
# 一个序列中的空字符串删掉
a = ['A', '', 'B', None, 'C', '   ']


def not_empty(s):
    return s and s.strip()


print(list(filter(not_empty, a)))
```

运行结果：


['A', 'B', 'C']