# 07-break&continue&else语句


## break语句

break语句可用于while和for循环，用来结束整个循环。当有嵌套循环时，break语句只能跳出最近一层的循环。


案例：使用break语句结束循环

```
while True:
    a = input("请输入一个字符：(输入q或Q结束)")
    if a.upper() == 'Q':
        print("循环结束，退出")
        break
    else:
        print(a)
```


##  continue语句

continue语句用于结束本次循环，继续下一次。多个循环嵌套时，continue也是应用于最近的一层循环。


案例：要求输入员工的薪资，若薪资小于0则重新输入。最后打印出录入员工的数量和薪资明细，以及平均薪资

```
emp_num = 0
salary_sum = 0
salarys = []

while True:
    s = input("请输入员工的薪资（按Q或q结束）")
    if s.upper() == 'Q':
        print("录入完成，退出")
        break
    if float(s) < 0:
        continue
    emp_num += 1
    salarys.append(float(s))
    salary_sum += float(s)
print("员工数{0},录入薪资{1},平均薪资{2}".format(emp_num, salary_sum, (salary_sum / emp_num)))
```


## else语句

while、for循环可以附带一个else语句（可选）。如果for、while语句没有被break语句结束，则会执行else子句，否则不执行。语法格式如下：

while 条件表达式：
    循环体
else：
    语句块

或者：

for 变量 in 可迭代对象：
    循环体
else：
    语句块


案例：员工一共4人。录入这4位员工的薪资。全部录入后，打印提示“您已经全部录入4名员工的薪资”。最后，打印输出录入的薪资和平均薪资
```
salaySum = 0
salarys = []

for i in range(4):
    s = input("请输入一共4名员工的薪资（按q或Q中途结束）")
    if s.upper() == 'Q':
        print("录入完成，退出")
        break
    if float(s) < 0:
        continue
    salarys.append(float(s))
    salaySum += float(s)
else:
    print("你已经全部录入4名员工的薪资")
print("录入薪资:{0},平均薪资{1}".format(salarys, salaySum/4))
```