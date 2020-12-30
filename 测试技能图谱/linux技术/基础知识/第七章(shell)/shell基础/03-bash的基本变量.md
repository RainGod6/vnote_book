# 03-bash的基本变量


**什么是变量？**

变量是计算机内存的单元，其中存放的值可以改变。当shell脚本需要保存一些信息时，如一个文件名或一个数字，就把它存放在一个变量中。每个变量有一个名字，所以很容易引用它。使用变量可以保存有用信息，使系统获知用户相关设置，变量也可以用于保存暂时信息。


变量设置规则：

- 变量名称可以由字母、数字和下划线组成，但是不能以数字开头。如果变量名是“2name”则是错误的。
- 在bash中，变量的默认类型都是字符串型，如果要进行数值运算，则必修指定变量类型为数值型。

案例：数字开头的变量会报错
```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# 2name = sc
-bash: 2name: command not found
```

- 变量用等号连接值，等号左右两侧不能有空格。
- 变量的值如果有空格，需要使用单引号或双引号包括。
- 在变量的值中，可以使用“\”转义符
- 如果需要增加变量的值，那么可以进行变量的叠加。不过变量需要用双引号包含“$变量名”或用${变量名}包含
- 如果是把命令的结果作为变量值赋予变量，则需要使用反引号或$()包含命令
- 环境变量名建议大写，便于区分


案例：
```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# name=$(date)
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $name
Tue Dec 22 09:32:58 CST 2020
```

**变量分类**

- 用户自定义变量
- 环境变量：这种变量中主要保存的是和系统操作环境相关的数据
- 位置参数变量：这种变量主要是用来向脚本当中传递参数或数据的，变量名不能自定义，变量作用是固定的
- 预定义变量：是bash中已经定义好的变量，变量名不能自定义，变量作用也是固定的


## 本地变量

也就是自定义变量，

变量定义：

```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# name=longyu
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $name
longyu
```

变量叠加：

```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# aa=123
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# aa=${aa}456
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $aa
123456
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# aa="$aa"789
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $aa
123456789
```


变量调用：

```
使用echo $变量名
```

变量查看：
```
set
```

变量删除：
```
unset name
```

案例：
```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# unset name
```

## 环境变量


**环境变量是什么？**

用户自定义变量只在当前的shell中生效，而环境变量会在当前shell和这个shell的所有子shell当中生效。如果环境变量写入相应的配置文件，那么这个环境变量就会在所有的shell中生效。


**设置环境变量**

```
申名变量：
export 变量名=变量值
查询变量：
env
删除变量：
unset 变量名
```


pstree命令可以查看进程树，如下：

```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# pstree
systemd─┬─AliSecGuard───6*[{AliSecGuard}]
        ├─AliYunDun───22*[{AliYunDun}]
        ├─AliYunDunUpdate───3*[{AliYunDunUpdate}]
        ├─CmsGoAgent.linu─┬─exe───13*[{exe}]
        │                 └─6*[{CmsGoAgent.linu}]
        ├─2*[agetty]
        ├─aliyun-service───7*[{aliyun-service}]
        ├─assist_daemon───7*[{assist_daemon}]
        ├─atd
        ├─auditd───{auditd}
        ├─chronyd
        ├─containerd─┬─containerd-shim─┬─gunicorn───gunicorn
        │            │                 └─9*[{containerd-shim}]
        │            └─12*[{containerd}]
        ├─crond
        ├─dbus-daemon
        ├─dhclient
        ├─dockerd─┬─docker-proxy───5*[{docker-proxy}]
        │         └─10*[{dockerd}]
        ├─firewalld───{firewalld}
        ├─mysqld───33*[{mysqld}]
        ├─polkitd───6*[{polkitd}]
        ├─rsyslogd───2*[{rsyslogd}]
        ├─sshd───sshd───bash───bash───pstree
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        └─tuned───4*[{tuned}]
```


可以将已经定义好的本地变量，升级成环境变量：

```
name=ly
export name #将name改变成环境变量
```

**系统常见环境变量**


- PATH：系统查找命令路径

```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
```

- PATH变量叠加："$PATH":/root/sh


**定义系统提示符的变量**

```
\d:    显示日期，格式为：“星期 月 日”
\h：   显示简写主机名。如默认主机名：localhost
\t:    显示24小时制时间，格式为“HH：MM：SS”
\T：   显示12小时制时间，格式为“HH：MM：SS”
\A：   显示24小时制时间，格式为“HH：MM”
\u:    显示当前用户名
\w:    显示当前所在目录的完整名称
\W：    显示当前所在目录的最后一个目录
\#：    执行的第几个命令
\$:    提示符。如果是root用户会显示提示符为“#”，如果是普通用户会显示提示符为“$”
```

案例：查看本机的默认提示符格式

```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $PS1
[\u@\h \W]\$
```

举例：
```
PS1='[\u@\t\w]\$ '
```


## 位置参数变量


1、位置参数变量

| 位置参数变量 |                                       作用                                        |
| ---------- | -------------------------------------------------------------------------------- |
| $n         | n为数字，$0代表命令本身，$1-$9代表第一个到第九个参数，十以上的参数需要用大括号包含，如${10} |
| $*         | 这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体                               |
| $@         | 这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待                               |
| $#         | 这个变量代表命令行中所有参数的个数                                                    |


编写一个位置参数脚本，如下：

```
#!/bin/bash
echo $0
echo $1
echo $2
echo $3
```
执行：

```
[root@iZ2vcdckpocdm8z7a36gl1Z sh]# ./params.sh abc 12 
./params.sh #第一个参数
abc         #第二个参数
12          #第三个参数
```


加法运算案例：
```
#!/bin/sh
num1=$1
num2=$2
sum=$(($num1+$num2))
echo $sum
```
调用执行：
```
[root@iZ2vcdckpocdm8z7a36gl1Z sh]# bash addsum.sh 10 90
100
```

## 预定义变量

| 预定义变量 |                                                    作用                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------- |
| $?        | 最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0，则证明上一个命令执行不正确 |
| $$        | 当前进程的进程号，PID                                                                                         |
| $!        | 后台运行的最后一个进程的进程号(PID)                                                                             |

案例：

```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $$
16702    显示当前进程ID
```


**接收键盘输入**  

```
read [选项] [变量名]
选项：
    -p：提示信息，在等待read输入时，输出提示信息
    -t：秒数，read命令会一直等待用户的输入，使用此选项可以指定等待时间
    -n：字符数，read命令只接收指定的字符数，就会执行
    -s：隐藏输入的数据，适用于机密信息的输入
```

案例；：
```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# read -p "请输入：" var
请输入：test
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $var
test
```

案例：
```
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# read -n 1 -p "请输入[Y/N]: " gender
请输入[Y/N]: y[root@iZ2vcdckpocdm8z7a36gl1Z ~]# echo $gender 
y
```

