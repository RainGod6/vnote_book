# 08-操作MySQL查询数据


python查询mysql使用fetchone()方法获取单条数据，使用fetchall()方法获取多条数据。

fetchone(): 该方法获取下一个查询结果集。结果集是一个对象
fetchall()：接收全部的返回结果行
rowcount: 这是一个只读属性，并返回执行execute()方法后影响的行数。


案例：查询所有数据

```
import pymysql
# 创建连接
con = pymysql.connect(host='localhost', user='root', password='12345678', database='python_db', port=3306)
# 创建游标对象
cur = con.cursor()
# 编写sql
sql = 'select * from t_student'

try:
    # 执行sql
    cur.execute(sql)
    # 处理查询结果
    students = cur.fetchall()
    for student in students:
        print(student)
except Exception as e:
    print(e)
    print("查询所有数据失败")
finally:
    # 关闭连接
    con.close()



```



案例： 查询单条数据


```
# 测试操作mysql查询单条数据

import pymysql
# 创建连接
con = pymysql.connect(host='localhost', user='root', password='12345678', database='python_db', port=3306)
# 创建游标对象
cur = con.cursor()
# 编写sql
sql = 'select * from t_student'

try:
    # 执行sql
    cur.execute(sql)
    # 处理查询结果
    student = cur.fetchone()
    print(student)
except Exception as e:
    print(e)
    print("查询所有数据失败")
finally:
    # 关闭连接
    con.close()

```


总结：

- 查询所有数据使用fetchall()方法，返回所有数据，可以使用for循环进行遍历获取
- 查询单条数据使用，fetchone()方法，返回一行数据
- fetchmany()方法，可以指定查询的数据size，返回size行数据


