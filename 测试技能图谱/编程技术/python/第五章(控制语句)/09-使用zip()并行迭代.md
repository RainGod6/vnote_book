# 09-使用zip()并行迭代

我们可以通过zip()函数对多个序列进行并行迭代，zip函数在最短序列“用完”时就会停止。


案例：测试zip()并行迭代

```
names = ("高小一", "高老二", "高老三", "高老四")
ages = (18, 20, 28, 30)
jobs = ("老师", "程序员", "公务员")

for name, age, job in zip(names, ages, jobs):
    print("{0}---{1}---{2}".format(name, age, job))

```

执行结果：
```
高小一---18---老师
高老二---20---程序员
高老三---28---公务员

```