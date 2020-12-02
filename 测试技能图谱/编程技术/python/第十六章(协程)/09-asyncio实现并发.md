# 09-asyncio实现并发

并发和并行一直是容易混淆的概念。并发通常指多个任务需要同时进行，并行则是同一时刻有多个任务执行。用上课来举例就是，并发情况下是一个老师在同一时间段辅助不同的人功课。并行则是好几个老师分别同时辅助多个学生功课。简而言之就是一个人同时吃三个馒头还是三个人同时分别吃一个的情况，吃一个馒头算一个任务。

asyncios实现并发，就需要多个协程来完成任务，每当有任务阻塞的时候就await，然后其他协程继续工作。创建多个协程的列表，然后将这些协程注册到事件循环中。


案例：asyncio实现并发

```
import asyncio, time


async def do_work(x):
    print("watting: ", x)
    await asyncio.sleep(x)
    return "Done after {} s".format(x)


start = time.time()
# 创建多个协程对象
coroutine1 = do_work(1)
coroutine2 = do_work(2)
coroutine3 = do_work(4)

# 创建任务列表

tasks = [
    asyncio.ensure_future(coroutine1),
    asyncio.ensure_future(coroutine2),
    asyncio.ensure_future(coroutine3)
]

# 将任务注册到事件循环中
loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks))  # 添加多个任务时，需要使用asyncio.wait方法
# 获取返回的结果
for task in tasks:
    print("task result: ", task.result())
print("time: ", time.time() - start)
```

运行结果：

watting:  1
watting:  2
watting:  4
task result:  Done after 1 s
task result:  Done after 2 s
task result:  Done after 4 s
time:  4.003508806228638


可以发现，使用并发多个协程执行任务只需要4s多的时间，如果是单任务的话就需要7s以上的时间。