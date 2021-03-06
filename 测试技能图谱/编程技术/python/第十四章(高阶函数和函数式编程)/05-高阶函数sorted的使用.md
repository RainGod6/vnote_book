# 05-高阶函数sorted的使用


   排序算法，排序也是程序中经常用到的算法。无论使用冒泡排序还是快速排序，排序的核心是比较两个元素的大小。如果是数字，我们可以直接比较，但如果是字符串或者两个dict呢？直接比较数学上的大小是没有意义的，因此，比较的过程必须通过函数抽象出来。通常规定，对于两个元素x和y，如果认为x<y，则返回-1，如果认为x==y，则返回0，如果认为x>y,则返回1，这样排序算法就不用关心具体的比较过程，而是比较结果直接排序。



案例一：# 对数值进行排序

```

sort_list = sorted([42, 11, -10, -5, 0, 100])
print("默认升序排列：", sort_list)

# 使用降序排列，给sorted添加reverse参数
sort_list = sorted([42, 11, -10, -5, 0, 100], reverse=True)
print("使用降序排列：", sort_list)

```

运行结果如下：

默认升序排列： [-10, -5, 0, 11, 42, 100]
使用降序排列： [100, 42, 11, 0, -5, -10]


案例二：# 对字符串排序(使用对是字符对应对ascii码，取每个字符进行比较)

```
sort_list = sorted(['abc', 'ad', 'ABC', 'D', 'd', 'c'])
print('字符串排序', sort_list)
```

运行结果如下：

字符串排序 ['ABC', 'D', 'abc', 'ad', 'c', 'd']



**sorted是高阶函数，它还可以接收一个key函数来实现自定义的排序**



案例三：对一个数值列表，按绝对值对大小排序,如下


```

sort_list = sorted([42, 11, -10, -5, 0, 100], key=abs)
print("按绝对值大小排序", sort_list)

```

运行结果如下：

按绝对值大小排序 [0, -5, -10, 11, 42, 100]