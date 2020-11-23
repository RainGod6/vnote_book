# 02-使用sqlite3插入数据


使用游标对象的excute执行插入的sql，使用excutemany()执行多条sql语句，使用excutemany()比循环使用excute()执行多条sql语句效率高。


案例：插入单条数据

```
# 测试操作sqlite3数据库插入一条数据

import sqlite3
# 创建连接
con = sqlite3.connect("/Users/user/study/sqlite3demo/demo.db")
# 创建游标对象
cur = con.cursor()
# 编写插入sql
sql = 'insert into t_person(pname,age) values (?,?)'
try:
    # 执行sql
    cur.execute(sql, ('张三', 24))
    # 提交事务
    con.commit()
    print("插入数据成功")
except Exception as e:
    print(e)
    # 插入失败进行事务回滚
    con.rollback()
    print("插入数据失败")
finally:
    # 关闭游标连接
    cur.close()
    # 关闭数据库连接
    con.close()
```


案例： 插入多条数据

```
# 测试操作sqlite3数据库插入多条数据

import sqlite3
# 创建连接
con = sqlite3.connect("/Users/user/study/sqlite3demo/demo.db")
# 创建游标对象
cur = con.cursor()
# 编写插入sql
sql = 'insert into t_person(pname,age) values (?,?)'
try:
    # 执行sql插入多条数据的sql
    cur.executemany(sql, [('李四', 25), ('小花', 27), ('小明', 30)])
    # 提交事务
    con.commit()
    print("插入多条数据成功")
except Exception as e:
    print(e)
    # 插入失败进行事务回滚
    con.rollback()
    print("插入多条数据失败")
finally:
    # 关闭游标连接
    cur.close()
    # 关闭数据库连接
    con.close()
```


总结：

- 插入单条数据使用的方法是，execute()方法，传递数据的时候，参数是一个元组。
- 插入多条数据使用的方法是：executemany()方法，传递数据的时候是一个列表，具体的数据使用多个元组存放。

