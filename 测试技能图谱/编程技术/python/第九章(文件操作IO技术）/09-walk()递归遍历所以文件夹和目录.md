# 09-walk()递归遍历所以文件夹和目录


os.walk()方法：

返回一个3个元素的元组，（dirpath,dirnames,filenames）:

- dirpath: 要列出指定目录的路径
- dirnames：目录下的所有文件夹
- filenames：目录下的所有文件



案例：递归遍历工作目录下的所有子目录和文件

```
# 测试遍历工作目录下的子目录和子文件

import os

path = os.getcwd()
list_files = os.walk(path)
all_file = []

for dirpath, dirnames, filenames in list_files:
    for dir in dirnames:
        all_file.append(os.path.join(dirpath, dir))
    for file in filenames:
        all_file.append(os.path.join(dirpath, file))

for file in all_file:
    print(file)

```

执行结果如下：
/Users/user/PycharmProjects/study/os/my03.py
/Users/user/PycharmProjects/study/os/my02.py
/Users/user/PycharmProjects/study/os/my01.py
/Users/user/PycharmProjects/study/os/my05.py
/Users/user/PycharmProjects/study/os/my04.py