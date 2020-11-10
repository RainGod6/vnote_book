# 10-@property装饰器


@property可以将一个方法的调用方式变成“属性调用”。下面是一个简单的示例：

```
# 简单测试@property


class Employee:
    @property
    def salary(self):
        return 30000


emp1 = Employee()
print(emp1.salary)  # 打印30000
print(type(emp1.salary))  # 打印<class 'int'>

# emp1.salary()  # TypeError: 'int' object is not callable
# emp1.salary = 10000 # AttributeError: can't set attribute

```

@property主要用于帮助我们处理属性的读操作、写操作。对于某一个属性我们可以直接通过：emp1.salary = 30000

如上的操作读操作、写操作。但是，这种做法不安全。比如，我需要限制薪水必须为1-10000的数字。这时候，我们就要通过getter、setter方法来处理。

```
# 测试@property

class Employee:

    def __init__(self, name, salary):
        self.name = name
        self._salary = salary

    @property
    def salary(self):
        print("月薪为{0},年薪为{1}".format(self._salary, (self._salary * 12)))
        return self._salary

    @salary.setter
    def salary(self, salary):
        if (0 < salary < 10000):
            self._salary = salary
        else:
            print("薪水录入错误，只能在0-10000之间")


emp1 = Employee("LY", 1000)
print(emp1.salary)
emp1.salary = -200

```

执行结果如下：
```
月薪为1000,年薪为12000
1000
薪水录入错误，只能在0-10000之间

```

**属性和方法命名总结**

- _xxx:保护成员，不能用 from module import * 导入，只有类对象和子类对象能访问这些成员
- __xxx__: 系统定义的特殊成员
- __xxx：类中的私有成员，只有类对象自己能访问，子类对象也不能访问。（但，在类外部可以通过：对象名._类名__xxx这种特殊格式访问）。Python不存在严格意义的私有成员。

**方法和属性都遵循上面的规则**


**类编码风格**

- 类名首字母大写，多个单词之间采用驼峰原则
- 实例名、模块名采用小写，多个单词之间用下划线隔开
- 每个类，应该跟“文档字符串”，说明这个类的作用
- 可以用空行组织代码，但不能滥用。在类中，使用一个空行隔开方法；模块中，使用两个空行隔开多个类。