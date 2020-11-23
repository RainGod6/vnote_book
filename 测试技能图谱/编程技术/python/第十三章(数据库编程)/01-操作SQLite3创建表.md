# 01-操作SQLite3创建表



## 操作SQLite3数据库

   从python3.x版本开始，在标准库中已经内置了SQLite3模块，他可以支持SQLlite3数据库的访问和相关的数据库操作。在需要操作SQLite3数据库数据时，只须在程序中导入SQLite3模块即可。Python语言操作SQLite3数据库的基本流程如下所示。

- （1）导入相关库或模块（SQLite3）
- （2）使用connect()连接数据库并获取数据库连接对象。它提供了以下方法：
   - .cursor() 方法来创建一个游标对象
   - .commit() 方法来处理事务提交
   - .rollback() 方法来处理事务回滚
   - .close() 方法来关闭一个数据库连接
- （3）使用con.cursor()获取游标对象。
- （4）使用游标对象的方法（exexute()、executemany()、fetchall()等）来操作数据库，实现插入、修改和删除操作，并查询获取显示相关等记录。在python程序中，连接函数sqlite3.connect()有如下两个常用参数。
  - databse：表示要访问的数据库名
  - timeout：表示访问数据的超时设定
- （5）使用close()关闭游标对象和数据库连接。数据库操作完成之后，必须及时调用其close()方法关闭数据库连接，这样做的目的是减轻数据库服务器的压力。



使用sqlit3模块的connect方法来创建打开数据库，需要指定数据库路径，不存在则创建一个新的数据库。
```
con = sqlite3.connect("/Users/user/study/sqlite3demo/python.db")
```

案例：使用sqlit3创建表

```
# 测试操作sqlite3创建表

"""
1、导入sqlite3模块
2、创建连接 sqlite3.connect()
3、创建游标对象
4、编写创建表的sql语句
5、执行sql
6、关闭连接
"""

# 导入模块
import sqlite3
# 创建连接
con = sqlite3.connect("/Users/user/study/sqlite3demo/demo.db")
print(con)
# 创建游标对象,用来执行sql
cur = con.cursor()
# 编写创建表的sql语句
sql = '''create table t_person(
    pno INTEGER  primary key autoincrement,
    pname varchar not null,
    age INTEGER 
)'''
try:
    # 执行sql
    cur.execute(sql)
    print("创建表成功")
except Exception as e:
    print(e)
    print("创建表失败")
finally:
    # 关闭游标
    cur.close()
    # 关闭连接
    con.close()

```