# 04-with上下文管理


finally块由于是否发生异常都会执行，通常我们释放资源的代码。其实，我们可以通过with上下文管理，更方便的实现释放资源的操作。

with上下文管理的语法结构如下：

with context_expr[as var]:
    语句块


with上下文管理可以自动管理资源，在with代码块执行完毕后自动还原进入该代码之前的现场或上下文。不论何种原因跳出with块，总能保证资源正常释放。极大的简化了工作，在文件操作、网络通信相关的场合非常常用。


案例：
```
# 测试with上下文管理文件操作，with不是用来取代try...except...finally,只是作为补充，方便我们在文件管理、网络通信的开发

with open("/Users/user/desktop/log.txt") as f:
    for line in f:
        print(line)

print("程序执行结束")
```

