# 08-阻塞和await


使用async可以定义协程对象，使用await可以针对耗时的操作进行挂起，就像生成器里的yield一样。函数让出控制权。协程遇到await，事件循环将会挂起该协程，执行别的协程，直到其他的协程也挂起或执行完毕，再进行下一个协程的执行。


耗时操作一般是一些IO操作，例如网络请求，文件读取等。我们使用asyncio.sleep函数来模拟IO操作，协程的目的也是让这些IO操作异步化。


案例：asyncio.sleep函数模拟IO操作

```
import asyncio, time


async def do_work(x):
    print("watting: ", x)
    await asyncio.sleep(x)
    return "Done after {}".format(x)


start = time.time()
# 创建协程对象
coroutine = do_work(3)
# 创建事件循环
loop = asyncio.get_event_loop()
# 创建任务
task = asyncio.ensure_future(coroutine)
# 执行任务
loop.run_until_complete(task)
# 获取返回结果
print("task reslut: ", task.result())
print("time:", time.time() - start)

```

运行结果：

watting:  3
task reslut:  Done after 3
time: 3.005718946456909