# 03-chattr&sudo权限


## 文件系统属性chattr权限

1、chattr 命令格式

```
chattr [+-=] [选项] 文件或目录名
+： 增加权限
-： 删除权限
=： 等于某权限

选项：
  i： 如果对文件设置i属性，那么不允许对文件进行删除、改名，也不能添加和修改数据；如果对目录设置i属性，那么只能修改目录下文件的数据，但不允许建立和删除文件
  a： 如果对文件设置a属性，那么只能在文件中增加数据，但是不能删除也不能修改数据；如果对目录设置a属性，那么只允许在目录中建立和修改文件，但是不允许删除
```



2、查看文件系统属性

```
lsattr 选项 文件名
选项：
    -a 显示所有文件和目录
    -d 若目标是目录，仅列出目录本身对属性，而不是子文件的
```


案例：

```
[root@iZ2vcdckpocdm8z7a36gl1Z project]# chattr +i def
[root@iZ2vcdckpocdm8z7a36gl1Z project]# ll
total 0
-rw-r--r--  1 root root 0 Dec 28 15:27 abc
-rw-rw----+ 1 root root 0 Dec 28 15:29 bcd
-rw-rw----+ 1 root root 0 Dec 28 18:54 def
[root@iZ2vcdckpocdm8z7a36gl1Z project]# lsattr def
----i--------e-- def
```

接下来我们对文件进行写入内容：
```
[root@iZ2vcdckpocdm8z7a36gl1Z project]# echo 123>def
-bash: def: Permission denied
```

可以发现提示权限不够。



## sudo权限


1、sudo权限

- root把本来只能超级用户执行的命令赋予普通用户执行
- sudo的操作对象是系统命令


2、sudo使用

```
visudo

实际修改的是/etc/sudores文件，因此也可以直接修改文件

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin

root    ALL=(ALL)       ALL
# 用户名  被管理的主机地址=（可使用的身份） 授权命令（绝对路径）

## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL
%组名  被管理的主机地址=（可使用的身份） 授权命令（绝对路径）
```


案例： 授权test用户可以重启服务器

```
visudo
test ALL= /sbin/shutdown -r now
```


3、普通用户执行sudo赋予的命令

```
# 切换用户
su - test
# 查看可用的sudo 命令
sudo -l 
# 普通用户执行sudo赋予的命令
sudo /sbin/shutdown -r now
```
注意必须以绝对路径命令的形式才能执行。不能只实现shutdown

