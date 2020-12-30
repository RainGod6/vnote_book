# 05-case语句


## 多分支case语句


- case语句和if...elif...else语句一样都是多分支条件语句，不过和if多分支语句不同的是，case语句只能判断一种条件关系，而if语句可以判断多种条件关系


```
case $变量名 in "值1"）
    如果变量的值等于值1，则执行程序1;;
    "值2"）
    如果变量的值等于值2，则执行程序2;;
    ....省略其它分支
    *）
    如果变量的值都不是以上的值，则执行此程序;;
esac
```


案例：

```
#!/bin/bash

read -p "please choose yes/no: " -t 30 cho
case $cho in 
   "yes")
        echo "your choose is yes! " ;;

   "no")
        echo "your choose is no! " ;;
   *)
        echo "your choose is error " ;;
esac

```

