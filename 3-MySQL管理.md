<!-- order:3 -->
## MySQL 管理

### 创建新用户

```sql
# 列出 MySQL 的数据库列表
SHOW DATABASES

# 设置活动databeses
use mysql;

# 向mysql.user中添加内容
# 创建新用户
INSERT INTO user 
          (host, user, password, 
           select_priv, insert_priv, update_priv) 
           VALUES ('localhost', 'guest', 
           PASSWORD('guest123'), 'Y', 'Y', 'Y');
# 在添加用户时，请注意使用MySQL提供的 PASSWORD() 函数来对密码进行加密。

# 刷新授权表
FLUSH PRIVILEGES;

# 另一种创建用户的方法：使用GRANT命令
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP
ON TUTORIALS.*
TO 'zara'@'localhost'
IDENTIFIED BY 'zara123';

# 分配数据库给用户
create database db_web_monitor
# 然后将这个数据库授予一个叫xavier的用户使用
mysql> GRANT ALL PRIVILEGES ON db_web_monitor.* TO xavier@localhost IDENTIFIED BY "xavier";
# 这样就可以使用xavier用户，密码为xavier在本机登录MySQL操作db_web_monitor数据库了。

# 授予远程使用权限
mysql>GRANT ALL PRIVILEGES ON db_web_monitor.* TO xavier@"%" IDENTIFIED BY "xavier";
```

### 命令

```sql
# 显示指定数据库的所有表
SHOW TABLES
# 显示数据表的属性，属性类型，主键信息
SHOW COLUMNS FROM 数据表
# 显示数据表的详细索引信息
SHOW INDEX FROM 数据表
# 该命令将输出MySQL数据库管理系统的性能及统计信息。
SHOW TABLE STATUS from DB

```

### 数据库操作
```shell
# 删除数据库
mysqladmin -u root -p drop test
# 创建新数据库
mysqladmin -u root -p create test 

```
