# 03-yield实现生成者消费者


## send发送数据

send是发送一个参数给res的，因为上面学习到，return的时候，并没有把4赋值给res，下次执行的时候只好继续执行赋值操作，只好赋值None了，而如果send的话，开始执行的时候，先接着上一次（return 4之后）执行，先把10赋值给了res，然后执行next的作用，遇见下一回的yield，return出结果后结束。


案例：使用send发送数据

```
def foo():
    print("starting")
    while True:
        res = yield 4
        print("res:", res)


g = foo()
print(next(g))
print(g.send(10))
print(next(g))

```

运行结果如下：

starting
4
res: 10
4
res: None
4



案例二： 使用yield实现生成者消费者传递数据

```
def produce(c):
    for i in range(1, 11):
        print("生产者生产产品:{}".format(i))
        c.send(str(i))


def consumer():
    while True:
        res = yield
        print("消费者消费产品:", res)


c = consumer()  # 生成器对象
next(c)
produce(c)

```

运行结果如下：

生产者生产产品:1
消费者消费产品: 1
生产者生产产品:2
消费者消费产品: 2
生产者生产产品:3
消费者消费产品: 3
生产者生产产品:4
消费者消费产品: 4
生产者生产产品:5
消费者消费产品: 5
生产者生产产品:6
消费者消费产品: 6
生产者生产产品:7
消费者消费产品: 7
生产者生产产品:8
消费者消费产品: 8
生产者生产产品:9
消费者消费产品: 9
生产者生产产品:10
消费者消费产品: 10