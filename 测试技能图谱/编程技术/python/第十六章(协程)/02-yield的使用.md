# 02-yield的使用


python对协程的支持是通过generator实现的。在generator中，不但可以通过for循环来迭代，还可以不断调用next()函数获取由yield语句返回的下一个值。

先把yield看作“return”，这个是直观的，它首先是个return，普通的return是什么意思，就是程序中返回某个值，返回之后程序就不再往下运行了。看作return之后再把它看做一个是生成器（generator）的一部分（带yield的函数才是真正的迭代器）。



案例：yield的使用

```
def foo():
    print("starting")
    while True:
        res = yield 4
        print("res:", res)


"""
在函数中使用了yield，则该函数就成为了一个生成器
yield的理解：
1、当成return 程序返回
2、当成生成器
"""

g = foo()  # g就指向了一个生成器对象
print(type(g))
print(next(g))
print("*" * 20)
print(next(g))

```

运行结果如下：

<class 'generator'>
starting
4
\********************
res: None
4


总结：运行过程

1、首先函数内使用yield代表这个函数是一个生成器对象
2、使用next函数可以获取生成器对象的值
3、当第一次使用next函数时，会执行到yield的地方，进行返回，后面的语句将不再执行
3、当第二次调用next函数时，会接着第一次执行的地方向下执行，就输出res的结果，因为使用yield程序返回，res就为None。
4、由于是个循环，所以又会输出4，然后又停止，等待下一次next调用。




案例：使用yield实现简单协程

```
import time


def A():
    while True:
        print("----------A-----------")
        yield
        time.sleep(0.5)


def B(c):
    while True:
        print("-----------B------------")
        c.__next__()
        time.sleep(0.5)


a = A()  # 生成一个生成器对象
B(a)
```

运行结果：

-----------B------------
----------A-----------
-----------B------------
----------A-----------
-----------B------------
----------A-----------
-----------B------------
----------A-----------
-----------B------------
.
.
.