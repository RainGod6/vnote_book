# 03-高阶函数reduce的使用


   reduce把一个函数作用在一个序列[x1,x2,x3...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算。其实效果就是：

```
reduce(f,[x1,x2,x3,x4] = f(f(f(x1,x2),x3),x4)
```

使用reduce高阶函数需要导入包：

```
from functools import reduce
```

案例一：使用高阶函数reduce计算序列的求和

```
from functools import reduce

# 计算一个序列的求和

a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


def add(x, y):
    return x+y


print(reduce(add, a))

```

运行结果：

55



案例 二：如果想把序列[1,3,5,7,9] 换成整数13579。 将列表中的每个元素乘以10加上后一个元素

```
from functools import reduce
# reduce实现对一个序列求和

def fn(x, y):
    return x * 10 + y


b = [1, 3, 5, 7, 9]

print(reduce(fn, b))
```

运行结果：

13579

