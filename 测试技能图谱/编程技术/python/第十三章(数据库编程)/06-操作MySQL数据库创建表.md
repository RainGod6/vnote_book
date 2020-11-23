# 06-操作MySQL数据库创建表


PyMySQL是在python3.x版本中用于连接MySQL服务器的一个库，pyhton2中则使用mysqldb。


在操作数据库mysql之前，我们需要安装pymysql。

```
pip install PyMySQL
```


## 创建数据库

在python程序中，可以使用excute()在数据库中创建一个新表。下面的实例代码演示


```
# 测试操作mysql创建表

import pymysql
# 创建连接
con = pymysql.connect(host='localhost', user='root', password='12345678', database='python_db', port=3306)
print(con)

# 创建游标对象
cur = con.cursor()
# 编写创建表的sql
sql = """
    create table t_student(
        sno int primary key auto_increment,
        sname varchar(30) not null,
        age int(2),
        score float(3,1)    
    )
"""
try:
    # 执行sql
    cur.execute(sql)
    print("创建表成功")
except Exception as e:
    print(e)
    print("创建表失败")
finally:
    # 关闭数据库连接
    con.close()
```