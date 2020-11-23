# 04-操作sqlite3修改和删除数据


案例：修改数据

```
# 测试操作sqlite3数据库修改数据

import sqlite3
# 创建连接
con = sqlite3.connect("/Users/user/study/sqlite3demo/demo.db")
# 创建游标对象
cur = con.cursor()
# 编写修改的sql语句
sql = 'update t_person set pname=? where pno=?'

try:
    # 执行sql
    cur.execute(sql, ('小张', 1))
    # 提交事务
    con.commit()
    print("数据修改成功")
except Exception as e:
    print(e)
    print("数据修改失败")
    # 将数据回滚
    con.rollback()
finally:
    # 关闭连接
    con.close()
```


案例：删除数据

```
# 测试sqlite3删除数据
import sqlite3
# 创建连接
con = sqlite3.connect("/Users/user/study/sqlite3demo/demo.db")
# 创建游标对象
cur = con.cursor()
# 编写删除sql语句
sql = 'delete from t_person where pno=?'
try:
    cur.execute(sql, (1,))
    con.commit()
    print("数据删除成功")
except Exception as e:
    print(e)
    con.rollback()
    print("数据删除失败")
finally:
    cur.close()
    con.close()
```