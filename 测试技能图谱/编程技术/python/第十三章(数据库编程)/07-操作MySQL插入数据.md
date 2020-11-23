# 07-操作MySQL插入数据


插入单条数据案例：

```
import pymysql

# 创建连接
con = pymysql.connect(host='localhost', user='root', password='12345678', database='python_db', port=3306)
# 创建游标对象
cur = con.cursor()
# 编写sql
sql = " insert into t_student(sname,age,score) values (%s,%s,%s)"
try:
    # 执行sql
    cur.execute(sql, ('小强', 18, 99.5))
    con.commit()
    print("数据插入成功")
except Exception as e:
    print(e)
    con.rollback()
    print("数据插入失败")
finally:
    # 关闭连接
    con.close()

```



插入多条数据：

```
# 测试操作mysql插入多条数据

import pymysql

# 创建连接
con = pymysql.connect(host='localhost', user='root', password='12345678', database='python_db', port=3306)
# 创建游标对象
cur = con.cursor()
# 编写sql
sql = " insert into t_student(sname,age,score) values (%s,%s,%s)"
try:
    # 执行sql插入多条数据
    cur.executemany(sql, [('小张', 19, 99.1), ('小李', 20, 88.5), ('小王', 27, 70.0)])
    con.commit()
    print("数据插入多条成功")
except Exception as e:
    print(e)
    con.rollback()
    print("数据插入多条失败")
finally:
    # 关闭连接
    con.close()

```


总结：

- 插入单条数据，方法使用excute()方法，且数据传递是一个元组
- 插入多条数据，方法使用executemany方法，且数据传递是一个列表，多条数据放在列表内使用元组。

