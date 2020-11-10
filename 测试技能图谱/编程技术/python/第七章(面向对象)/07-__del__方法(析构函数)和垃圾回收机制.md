# 07-__del__方法(析构函数)和垃圾回收机制


## __del__方法

__del__方法称为“析构方法”,用于实现对象被销毁时所需的操作。比如：释放对象占用的资源，例如：打开的文件资源、网络连接等。

Python实现自动的垃圾回收，当对象没有被引用时（引用计数为0），由垃圾回收器调用__del__方法。


我们也可以通过del语句删除对象，从而保证调用__del__方法。

系统会自动提供__del__方法，一般不需要自定义析构方法。

```
# 析构函数 ，当对象没有引用时，垃圾回收器自动调用析构函数__del__


class Person:
    def __del__(self):
        print("销毁对象：{0}".format(self))


P1 = Person()
P2 = Person()
del P2
print("程序结束")

```

执行结果如下：
```
销毁对象：<__main__.Person object at 0x1032133d0>
程序结束
销毁对象：<__main__.Person object at 0x1030a5490>
```

可以发现，del删除对象时，系统调用了析构函数，程序执行完成，对象释放也调用了析构函数。



## __call__方法

定义了__call__方法的对象，称为“可调用对象”，即该对象可以像函数一样被调用。

```
# 测试__call__ ，可调用对象


class SalaryAccount:
    '''工资计算类'''

    def __call__(self, salary):
        year_salary = salary * 12
        day_salary = salary // 30
        hour_salary = salary // 8
        return dict(month_salary=salary, year_salary=year_salary,
                    day_salary=day_salary, hour_salary=hour_salary)


s = SalaryAccount()
print(s(5000))

```

执行结果：
```
{'month_salary': 5000, 'year_salary': 60000, 'day_salary': 166, 'hour_salary': 625}
```