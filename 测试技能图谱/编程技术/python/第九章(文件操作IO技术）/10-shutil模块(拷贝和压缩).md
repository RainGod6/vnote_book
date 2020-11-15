# 10-shutil模块(拷贝和压缩)



## 拷贝

shutil模块是python标准库中提供的，主要用来做文件和文件夹的拷贝、移动、删除等；还可以做文件和文件夹的压缩、解压缩操作。 

os模块提供了对目录或文件的一般操作。shutil模块作为补充，提供了移动、复制、压缩、解压缩等操作，这些os模块都没有提供。


案例：测试shutil文件的拷贝

```
# 测试shutil模块的用法：拷贝

import shutil

shutil.copyfile("1.txt", "1_copy.txt")   # 拷贝文件

shutil.copytree("movie/港台", '电影') # 拷贝文件数，如果拷贝后的目录存在会报错FileExistsError

shutil.copytree("movie/港台", '电影', ignore=shutil.ignore_patterns("*.txt", "*.heml"))  # 使用ignore排除不拷贝的文件


```

## 压缩

压缩和解压缩功能除了使用shutil模块还可以使用zipfile模块


案例如下：测试使用shutil模块对文件压缩

```
import shutil
shutil.make_archive("电影/gg", 'zip', 'movie/港台')   # 使用shutil 压缩文件 
```


使用zipfile进行压缩
```
z1 = zipfile.ZipFile("a.zip", 'w')
z1.write("1.txt")
z1.write("my01.py")
z1.close()
```



## 解压缩

测试文件的解压缩：
```
# 解压缩

z2 = zipfile.ZipFile("a.zip", 'r')
z2.extractall("解压")  # 提供解压文件路径
z2.close()
```
