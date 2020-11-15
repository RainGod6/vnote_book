# 07-ThreadLocal的使用


我们知道多线程环境下，每一个线程均可以使用所属进程的全局变量。如果一恶搞线程对全局变量进行了修改，将会影响到其它所有的线程对全局变量对计算操作，从而出现数据混乱，即为脏数据。为了避免多个线程同时对变量进行修改，引入了线程同步机制，通过互斥锁来控制对全局变量的访问。所以有时候线程使用局部变量比全局变量好，因为局部变量只有线程自身可以访问，同一个进程下的其它线程不可访问。


但是局部变量也是有问题，就是在函数调用的时候，传递起来很麻烦。


因此，Python提供了ThreadLocal变量，它本身是一个全局变量，但是每个线程却可以用来保存属于自己的私有数据，这些私有数据对其它线程也是不可见的。


案例：ThreadLocal的使用

```
# 测试ThreadLocal的使用

import threading

# 创建ThreadLocal对象
local = threading.local()


def process_student():
    student_name = local.name
    print('线程名：{0} 学生名：{1}'.format(threading.current_thread().getName(), student_name))


def process_thread(name):
    # 将传入的值绑定到local的name上
    local.name = name
    process_student()


t1 = threading.Thread(target=process_thread, args=('张三',), name='Thread-A')
t2 = threading.Thread(target=process_thread, args=('李四',), name='Thread-B')

t1.start()
t2.start()
t1.join()
t2.join()
```

运行结果：

线程名：Thread-A 学生名：张三
线程名：Thread-B 学生名：李四