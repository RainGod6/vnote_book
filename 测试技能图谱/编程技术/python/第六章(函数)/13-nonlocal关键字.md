# 13-nonlocal关键字

nonlocal 用来申明外层的局部变量。
global 用来申明全局变量。


案例：使用nonlocal申明外层局部变量

```
a = 100


def outer():
    b = 10

    def inner():
        nonlocal b #申明外部函数的局部变量，内部使用修改时必须用nonlocal申明
        print("inner b :", b)
        b = 20
        global a #申明全局变量，使用全局变量需要申明
        a = 1000

    inner()
    print("outer b:", b)


outer()
print("a:", a)

执行结果如下：
inner b : 10
outer b: 20
a: 1000
```