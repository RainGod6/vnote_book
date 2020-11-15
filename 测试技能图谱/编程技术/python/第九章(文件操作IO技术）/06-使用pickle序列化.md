# 06-使用pickle序列化


Python中，一切皆对象，对象本质上就是一个“存储数据的内存块”。有时候，我们需要将内存块的数据，保存到硬盘上，或者通过网络传输到其它计算机上。这时候，就需要对象的序列化和反序列化。对象的序列化机制广泛的应用在分布式、并行系统上。

序列化指的是：将对象转化成“串行化”数据形式，存储到硬盘或通过网络传输到其它地方。
反序列化指相反的过程，将读取到的“串行化”数据转化成对象。

我们可以使用pickle模块中的函数，实现序列化操作。


序列化我们使用：
```
pickle.dump(obj,file)   obj就是要被序列化的对象，file指的是存储的文件
pickle.load(file)       从file中读取数据，反序列化成对象
```


案例：将对象反序列化到文件中
```
# 测试将对象序列化至文件中

import pickle

with open("data.dat", 'wb') as f:
    a1 = "测试对象"
    a2 = 999
    a3 = [10, 20, 30]

    pickle.dump(a1, f)
    pickle.dump(a2, f)
    pickle.dump(a3, f)

```


案例：将文件反序列化成对象
```
# 测试将文件中的数据反序列化成对象

with open("data.dat", "rb") as f:
    a4 = pickle.load(f)
    a5 = pickle.load(f)
    a6 = pickle.load(f)
    print(a4)
    print(a5)
    print(a6)
    print(id(a1))
    print(id(a4))

执行结果如下：
测试对象
999
[10, 20, 30]
4348578096
4348578192
```