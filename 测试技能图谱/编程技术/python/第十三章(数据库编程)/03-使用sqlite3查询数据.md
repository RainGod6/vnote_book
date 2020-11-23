# 03-使用sqlite3查询数据


查询数据，游标对象提供了fetchall()和fetchone()方法。fetchall()方法获取所有数据，返回一个列表。fetchone()方法获取其中一个结果，返回一个元组。

案例：查询所有数据

```
# 测试操作sqlite3数据库查询所有数据

import sqlite3
# 创建连接
con = sqlite3.connect("/Users/user/study/sqlite3demo/demo.db")
# 创建游标对象
cur = con.cursor()
# 创建查询sql
sql = 'select * from t_person'
try:
    # 执行sql
    cur.execute(sql)
    # 获取结果集
    person_all = cur.fetchall()
    for person in person_all:
        print(person)
except Exception as e:
    print(e)
    print("查询数据失败")
finally:
    # 关闭游标
    cur.close()
    con.close()
```

运行结果如下：

(1, '张三', 24)
(2, '李四', 25)
(3, '小花', 27)
(4, '小明', 30)


案例：查询一条数据

```
# 测试操作sqlite3数据库查询一条数据

import sqlite3
# 创建连接
con = sqlite3.connect("/Users/user/study/sqlite3demo/demo.db")
# 创建游标对象
cur = con.cursor()
# 创建查询sql
sql = 'select * from t_person'
try:
    # 执行sql
    cur.execute(sql)
    # 获取结果集, 获取一条数据
    person_one = cur.fetchone()
    print(person_one)
except Exception as e:
    print(e)
    print("查询数据失败")
finally:
    # 关闭游标
    cur.close()
    con.close()
```

运行结果：

(1, '张三', 24)