# 09-操作MySQL修改和删除数据


案例：

```

import pymysql
# 创建连接
con = pymysql.connect(host='localhost', user='root', password='12345678', database='python_db', port=3306)
# 创建游标对象
cur = con.cursor()
# 编写sql
sql = 'update t_student set sname = %s where sno=%s'

try:
    # 执行sql
    cur.execute(sql, ('张三', 1))
    # 提交事务
    con.commit()
    print("数据修改成功")
except Exception as e:
    print(e)
    print("数据修改失败")
    con.rollback()
finally:
    # 关闭连接
    con.close()



```


删除数据：

```
# 测试操作mysql删除数据

import pymysql
# 创建连接
con = pymysql.connect(host='localhost', user='root', password='12345678', database='python_db', port=3306)
# 创建游标对象
cur = con.cursor()
# 编写sql
sql = 'delete from t_student  where sname=%s'

try:
    # 执行sql
    cur.execute(sql, ('张三'))
    # 提交事务
    con.commit()
    print("数据删除成功")
except Exception as e:
    print(e)
    print("数据删除失败")
    con.rollback()
finally:
    # 关闭连接
    con.close()



```