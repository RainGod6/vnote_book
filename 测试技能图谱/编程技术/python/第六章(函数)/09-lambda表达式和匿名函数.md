# 09-lambda表达式和匿名函数

lambda表达式可以用来申明匿名函数。lambda函数是一种简单的、在同一行中定义函数的方法。lambda函数实际生成了一个函数对象。


lambda表达式只允许包含一个表达式，不能包含复杂语句，该表达式的计算结果就是函数的返回值。

基本语法如下：

**lambda arg1,arg2,arg3......:<表达式>**

arg1/arg2/arg3为函数的参数。<表达式>相当于函数体。运算结果是：表达式的运算结果。


案例：lambda表达式使用

```
f = lambda a, b, c: a + b + c
print(f)
print(f(2, 3, 4))

g = [lambda a: a * 2, lambda b: b * 3, lambda c: c * 4]
print(g[0](6), g[1](7), g[2](8))

执行结果如下：
<function <lambda> at 0x102b06cb0>
9
12 21 32

```
