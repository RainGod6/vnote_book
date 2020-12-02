# 06-创建任务task


协程对象不能直接运行，在注册事件循环的时候，其实是run_until_complete方法将协程包装成为了一个任务（task）对象。所谓task对象是future类的子类。保存类协程运行后的状态，用于未来获取协程的结果。

asyncio.ensure_future(coroutine) 和loop.create_task(coroutine)都可以创建一个task，run_until_complite的参数是一个future对象。当传入一个协程，其内部会自动封装成task，task是future的子类。isinstance(task,asyncio.Future)将会输出True。


案例：创建task

```
import asyncio,time

async def do_work(x):
    print("watting: ", x)

# 获取协程对象
coroutine = do_work(3)
# 创建事件循环对象
loop = asyncio.get_event_loop()
# 创建任务
# task = asyncio.ensure_future(coroutine)  # 创建任务第一种方式
task = loop.create_task(coroutine)   # 创建任务第二种方式
print(task)
# 将协程对象注册到事件循环中
loop.run_until_complete(task)
print(task)
print("task是否是future的子类：", isinstance(task, asyncio.Future))
```

运行结果：

<Task pending coro=<do_work() running at /Users/user/PycharmProjects/study/协程/07创建任务task.py:3>>
watting:  3
<Task finished coro=<do_work() done, defined at /Users/user/PycharmProjects/study/协程/07创建任务task.py:3> result=None>
task是否是future的子类： True