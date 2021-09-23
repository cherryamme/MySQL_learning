## MySQL备忘录

### 连接 MySQL

```
mysql -u <user> -p

mysql [db_name]

mysql -h <host> -P <port> -u <user> -p [db_name]

mysql -h <host> -u <user> -p [db_name]
```

### 公共部分

#### 数据库

| `create database` db `;` | 创建数据库   |
| ------------------------ | ------------ |
| `show databases;`        | 列出数据库   |
| `use` db`;`              | 切换到数据库 |
| `drop database` db`;`    | 删除数据库   |

#### 数据表

| `show tables;`           | 列出当前数据库的表 |
| ------------------------ | ------------------ |
| `show fields from` t`;`  | 列出表的字段       |
| `desc` t`;`              | 显示表结构         |
| `show create table t``;` | 显示创建表sql      |
| `truncate table t``;`    | 删除表中的所有数据 |
| `drop table t``;`        | 删除表             |

#### 进程

| `show processlist;` | 列出进程 |
| ------------------- | -------- |
| `kill` 进程号`;`    | 杀死进程 |

#### 其他

| `exit` 或者 `\q` | 退出 MySQL 会话 |
| ---------------- | --------------- |
|                  |                 |

### 备份

创建备份

```
mysqldump -u user -p db_name > db.sql
```

导出没有架构的数据库

```
mysqldump -u user -p db_name --no-data=true --add-drop-table=false > db.sql
```

恢复备份

```
mysql -u user -p db_name < db.sql
```

## MySQL 示例

### 管理表

创建一个包含三列的新表

```
CREATE TABLE t (
     id INT PRIMARY KEY,
     name VARCHAR NOT NULL,
     price INT DEFAULT 0
);
```

从数据库中删除表

```
DROP TABLE t ;
```

向表中添加一个新列

```
ALTER TABLE t ADD column;
```

从表中删除列 c

```
ALTER TABLE t DROP COLUMN c ;
```

添加约束

```
ALTER TABLE t ADD constraint;
```

删除约束

```
ALTER TABLE t DROP constraint;
```

将表从 t1 重命名为 t2

```
ALTER TABLE t1 RENAME TO t2;
```

将列 c1 重命名为 c2

```
ALTER TABLE t1 RENAME c1 TO c2 ;
```

删除表中的所有数据

```
TRUNCATE TABLE t;
```

### 从表中查询数据

从表中查询 c1、c2 列中的数据

```
SELECT c1, c2 FROM t
```

查询表中的所有行和列

```
SELECT * FROM t
```

查询数据并使用条件过滤行

```
SELECT c1, c2 FROM t
WHERE condition
```

从表中查询不同的行

```
SELECT DISTINCT c1 FROM t
WHERE condition
```

按升序或降序对结果集进行排序

```
SELECT c1, c2 FROM t
ORDER BY c1 ASC [DESC]
```

跳过行的偏移量并返回接下来的 n 行

```
SELECT c1, c2 FROM t
ORDER BY c1 
LIMIT n OFFSET offset
```

使用聚合函数对行进行分组

```
SELECT c1, aggregate(c2)
FROM t
GROUP BY c1
```

使用 HAVING 子句过滤组

```
SELECT c1, aggregate(c2)
FROM t
GROUP BY c1
HAVING condition
```

### 从多个表查询

内连接 t1 和 t2

```
SELECT c1, c2 
FROM t1
INNER JOIN t2 ON condition
```

左连接 t1 和 t1

```
SELECT c1, c2 
FROM t1
LEFT JOIN t2 ON condition
```

右连接 t1 和 t2

```
SELECT c1, c2 
FROM t1
RIGHT JOIN t2 ON condition
```

执行全外连接

```
SELECT c1, c2 
FROM t1
FULL OUTER JOIN t2 ON condition
```

生成表中行的笛卡尔积

```
SELECT c1, c2 
FROM t1
CROSS JOIN t2
```

另一种执行交叉连接的方法

```
SELECT c1, c2 
FROM t1, t2
```

使用 INNER JOIN 子句将 t1 连接到自身

```
SELECT c1, c2
FROM t1 A
INNER JOIN t1 B ON condition
```

使用 SQL 运算符组合来自两个查询的行

```
SELECT c1, c2 FROM t1
UNION [ALL]
SELECT c1, c2 FROM t2
```

返回两个查询的交集

```
SELECT c1, c2 FROM t1
INTERSECT
SELECT c1, c2 FROM t2
```

从另一个结果集中减去一个结果集

```
SELECT c1, c2 FROM t1
MINUS
SELECT c1, c2 FROM t2
```

使用模式匹配 %, _ 查询行

```
SELECT c1, c2 FROM t1
WHERE c1 [NOT] LIKE pattern
```

查询列表中的行

```
SELECT c1, c2 FROM t
WHERE c1 [NOT] IN value_list
```

查询两个值之间的行

```
SELECT c1, c2 FROM t
WHERE  c1 BETWEEN low AND high
```

检查表中的值是否为 NULL

```
SELECT c1, c2 FROM t
WHERE  c1 IS [NOT] NULL
```

### 使用 SQL 约束

将 c1 和 c2 设置为主键

```
CREATE TABLE t(
    c1 INT, c2 INT, c3 VARCHAR,
    PRIMARY KEY (c1,c2)
);
```

将 c2 列设置为外键

```
CREATE TABLE t1(
    c1 INT PRIMARY KEY,  
    c2 INT,
    FOREIGN KEY (c2) REFERENCES t2(c2)
);
```

使 c1 和 c2 中的值唯一

```
CREATE TABLE t(
    c1 INT, c1 INT,
    UNIQUE(c2,c3)
);
```

确保 c1 > 0 和 c1 >= c2 中的值

```
CREATE TABLE t(
  c1 INT, c2 INT,
  CHECK(c1> 0 AND c1 >= c2)
);
```

在 c2 列中设置值不为 NULL

```
CREATE TABLE t(
     c1 INT PRIMARY KEY,
     c2 VARCHAR NOT NULL
);
```

### 修改数据

在表格中插入一行

```
INSERT INTO t(column_list)
VALUES(value_list);
```

在表中插入多行

```
INSERT INTO t(column_list)
VALUES (value_list), 
       (value_list), …;
```

将 t2 中的行插入到 t1

```
INSERT INTO t1(column_list)
SELECT column_list
FROM t2;
```

为所有行更新 c1 列中的新值

```
UPDATE t
SET c1 = new_value;
```

更新 c1、c2 列中与条件匹配的值

```
UPDATE t
SET c1 = new_value, 
        c2 = new_value
WHERE condition;
```

删除表中的所有数据

```
DELETE FROM t;
```

删除表中的行子集

```
DELETE FROM t
WHERE condition;
```

### 管理视图

创建一个包含 c1 和 c2 的新视图

```
CREATE VIEW v(c1,c2) 
AS
SELECT c1, c2
FROM t;
```

使用检查选项创建新视图

```
CREATE VIEW v(c1,c2) 
AS
SELECT c1, c2
FROM t;
WITH [CASCADED | LOCAL] CHECK OPTION;
```

创建递归视图

```
CREATE RECURSIVE VIEW v 
AS
select-statement -- anchor part
UNION [ALL]
select-statement; -- recursive part
```

创建临时视图

```
CREATE TEMPORARY VIEW v 
AS
SELECT c1, c2
FROM t;
```

删除视图

```
DROP VIEW view_name;
```

### 管理触发器

创建或修改触发器

```
CREATE OR MODIFY TRIGGER trigger_name
WHEN EVENT
ON table_name TRIGGER_TYPE
EXECUTE stored_procedure;
```

#### 什么时候调用

| `BEFORE` | 在事件发生之前调用 |
| -------- | ------------------ |
| `AFTER`  | 事件发生后调用     |

#### 事件

| `INSERT` | 调用 INSERT |
| -------- | ----------- |
| `UPDATE` | 调用更新    |
| `DELETE` | 调用删除    |

#### 触发器类型

| `FOR EACH ROW`       |      |
| -------------------- | ---- |
| `FOR EACH STATEMENT` |      |

### 管理索引

在 t 表的 c1 和 c2 上创建索引

```
CREATE INDEX idx_name 
ON t(c1,c2);
```

在t表的c3、c4上创建唯一索引

```
CREATE UNIQUE INDEX idx_name
ON t(c3,c4)
```

删除索引

```
DROP INDEX idx_name;
```

## MySQL 数据类型

### 字符串

| `CHAR`       | 字符串 (0 - 255)        |
| ------------ | ----------------------- |
| `VARCHAR`    | 字符串 (0 - 255)        |
| `TINYTEXT`   | 字符串 (0 - 255)        |
| `TEXT`       | 字符串 (0 - 65535)      |
| `BLOB`       | 字符串 (0 - 65535)      |
| `MEDIUMTEXT` | 字符串 (0 - 16777215)   |
| `MEDIUMBLOB` | 字符串 (0 - 16777215)   |
| `LONGTEXT`   | 字符串 (0 - 4294967295) |
| `LONGBLOB`   | 字符串 (0 - 4294967295) |
| `ENUM`       | 预设选项之一            |
| `SET`        | 预设选项的选择          |

### 日期时间

| `DATE`      | yyyy-MM-dd          |
| ----------- | ------------------- |
| `TIME`      | hh:mm:ss            |
| `DATETIME`  | yyyy-MM-dd hh:mm:ss |
| `TIMESTAMP` | yyyy-MM-dd hh:mm:ss |
| `YEAR`      | yyyy                |

### 数值

| `TINYINT x`   | 整数（-128 到 127）                                 |
| ------------- | --------------------------------------------------- |
| `SMALLINT x`  | 整数（-32768 到 32767）                             |
| `MEDIUMINT x` | 整数（-8388608 到 8388607）                         |
| `INT x`       | 整数（-2147483648 到 2147483647）                   |
| `BIGINT x`    | 整数（-9223372036854775808 到 9223372036854775807） |
| `FLOAT`       | 十进制（精确到 23 位）                              |
| `DOUBLE`      | 十进制（24 到 53 位）                               |
| `DECIMAL`     | “DOUBLE”存储为字符串                                |

## MySQL 函数和运算符

### 字符串

- [ASCII()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_ascii)
- [BIN()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_bin)
- [BIT_LENGTH()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_bit-length)
- [CHAR()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_char)
- [CHARACTER_LENGTH()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_character-length)
- [CHAR_LENGTH()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_char-length)
- [CONCAT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_concat)
- [CONCAT_WS()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_concat-ws)
- [ELT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_elt)
- [EXPORT_SET()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_export-set)
- [FIELD()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_field)
- [FIND_IN_SET()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_find-in-set)
- [FORMAT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_format)
- [FROM_BASE64()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_from-base64)
- [HEX()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_hex)
- [INSERT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_insert)
- [INSTR()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_instr)
- [LCASE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_lcase)
- [LEFT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_left)
- [LENGTH()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_length)
- [LIKE](https://dev.mysql.com/doc/refman/8.0/en/string-comparison-functions.html#operator_like)
- [LOAD_FILE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_load-file)
- [LOCATE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_locate)
- [LOWER()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_lower)
- [LPAD()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_lpad)
- [LTRIM()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_ltrim)
- [MAKE_SET()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_make-set)
- [MATCH](https://dev.mysql.com/doc/refman/8.0/en/fulltext-search.html#function_match)
- [MID()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_mid)
- [NOT LIKE](https://dev.mysql.com/doc/refman/8.0/en/string-comparison-functions.html#operator_not-like)
- [NOT REGEXP](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_not-regexp)
- [OCT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_oct)
- [OCTET_LENGTH()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_octet-length)
- [ORD()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_ord)
- [POSITION()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_position)
- [QUOTE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_quote)
- [REGEXP](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_regexp)
- [REGEXP_INSTR()](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-instr)
- [REGEXP_LIKE()](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-like)
- [REGEXP_REPLACE()](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-replace)
- [REGEXP_SUBSTR()](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-substr)
- [REPEAT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_repeat)
- [REPLACE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_replace)
- [REVERSE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_reverse)
- [RIGHT()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_right)
- [RLIKE](https://dev.mysql.com/doc/refman/8.0/en/regexp.html#operator_regexp)
- [RPAD()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_rpad)
- [RTRIM()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_rtrim)
- [SOUNDEX()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_soundex)
- [SOUNDS LIKE](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#operator_sounds-like)
- [SPACE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_space)
- [STRCMP()](https://dev.mysql.com/doc/refman/8.0/en/string-comparison-functions.html#function_strcmp)
- [SUBSTR()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substr)
- [SUBSTRING()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring)
- [SUBSTRING_INDEX()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring-index)
- [TO_BASE64()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_to-base64)
- [TRIM()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_trim)
- [UCASE()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_ucase)
- [UNHEX()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_unhex)
- [UPPER()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_upper)
- [WEIGHT_STRING()](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_weight-string)

### 日期和时间

- [ADDDATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_adddate)
- [ADDTIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_addtime)
- [CONVERT_TZ()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_convert-tz)
- [CURDATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_curdate)
- [CURRENT_DATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_current-date)
- [CURRENT_TIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_current-time)
- [CURRENT_TIMESTAMP()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_current-timestamp)
- [CURTIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_curtime)
- [DATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date)
- [DATE_ADD()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-add)
- [DATE_FORMAT()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format)
- [DATE_SUB()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-sub)
- [DATEDIFF()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_datediff)
- [DAY()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_day)
- [DAYNAME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_dayname)
- [DAYOFMONTH()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_dayofmonth)
- [DAYOFWEEK()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_dayofweek)
- [DAYOFYEAR()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_dayofyear)
- [EXTRACT()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_extract)
- [FROM_DAYS()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_from-days)
- [FROM_UNIXTIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_from-unixtime)
- [GET_FORMAT()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_get-format)
- [HOUR()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_hour)
- [LAST_DAY](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_last-day)
- [LOCALTIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_localtime)
- [LOCALTIMESTAMP()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_localtimestamp)
- [MAKEDATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_makedate)
- [MAKETIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_maketime)
- [MICROSECOND()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_microsecond)
- [MINUTE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_minute)
- [MONTH()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_month)
- [MONTHNAME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_monthname)
- [NOW()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_now)
- [PERIOD_ADD()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_period-add)
- [PERIOD_DIFF()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_period-diff)
- [QUARTER()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_quarter)
- [SEC_TO_TIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_sec-to-time)
- [SECOND()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_second)
- [STR_TO_DATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_str-to-date)
- [SUBDATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_subdate)
- [SUBTIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_subtime)
- [SYSDATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_sysdate)
- [TIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_time)
- [TIME_FORMAT()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_time-format)
- [TIME_TO_SEC()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_time-to-sec)
- [TIMEDIFF()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_timediff)
- [TIMESTAMP()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_timestamp)
- [TIMESTAMPADD()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_timestampadd)
- [TIMESTAMPDIFF()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_timestampdiff)
- [TO_DAYS()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_to-days)
- [TO_SECONDS()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_to-seconds)
- [UNIX_TIMESTAMP()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_unix-timestamp)
- [UTC_DATE()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_utc-date)
- [UTC_TIME()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_utc-time)
- [UTC_TIMESTAMP()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_utc-timestamp)
- [WEEK()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_week)
- [WEEKDAY()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_weekday)
- [WEEKOFYEAR()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_weekofyear)
- [YEAR()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_year)
- [YEARWEEK()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_yearweek)
- [GET_FORMAT()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_get-format)

### 数字

- [%, MOD](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_mod)
- [*](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_times)
- [+](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_plus)
- [-](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_minus)
- [-](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_unary-minus)
- [/](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_divide)
- [ABS()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_abs)
- [ACOS()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_acos)
- [ASIN()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_asin)
- [ATAN()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_atan)
- [ATAN2(), ATAN()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_atan2)
- [CEIL()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_ceil)
- [CEILING()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_ceiling)
- [CONV()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_conv)
- [COS()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_cos)
- [COT()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_cot)
- [CRC32()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_crc32)
- [DEGREES()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_degrees)
- [DIV](https://dev.mysql.com/doc/refman/8.0/en/arithmetic-functions.html#operator_div)
- [EXP()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_exp)
- [FLOOR()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_floor)
- [LN()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_ln)
- [LOG()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_log)
- [LOG10()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_log10)
- [LOG2()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_log2)
- [MOD()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_mod)
- [PI()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_pi)
- [POW()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_pow)
- [POWER()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_power)
- [RADIANS()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_radians)
- [RAND()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_rand)
- [ROUND()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_round)
- [SIGN()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_sign)
- [SIN()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_sin)
- [SQRT()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_sqrt)
- [TAN()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_tan)
- [TRUNCATE()](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_truncate)

### 总计的

- [平均（）](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_avg)
- [BIT_AND()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_bit-and)
- [BIT_OR()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_bit-or)
- [BIT_XOR()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_bit-xor)
- [数数（）](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_count)
- [计数（不同）](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_count-distinct)
- [GROUP_CONCAT()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_group-concat)
- [JSON_ARRAYAGG()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_json-arrayagg)
- [JSON_OBJECTAGG()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_json-objectagg)
- [最大限度（）](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_max)
- [最小值()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_min)
- [性病()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_std)
- [标准差（）](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_stddev)
- [STDDEV_POP()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_stddev-pop)
- [STDDEV_SAMP()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_stddev-samp)
- [和（）](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_sum)
- [VAR_POP()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_var-pop)
- [VAR_SAMP()](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_var-samp)
- [方差（）](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html#function_variance)

### JSON

- [->](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#operator_json-column-path)
- [->>](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#operator_json-inline-path)
- [JSON_ARRAY()](https://dev.mysql.com/doc/refman/8.0/en/json-creation-functions.html#function_json-array)
- [JSON_ARRAY_APPEND()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-array-append)
- [JSON_ARRAY_INSERT()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-array-insert)
- [JSON_CONTAINS()](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-contains)
- [JSON_CONTAINS_PATH()](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-contains-path)
- [JSON_DEPTH()](https://dev.mysql.com/doc/refman/8.0/en/json-attribute-functions.html#function_json-depth)
- [JSON_EXTRACT()](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-extract)
- [JSON_INSERT()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-insert)
- [JSON_KEYS()](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-keys)
- [JSON_LENGTH()](https://dev.mysql.com/doc/refman/8.0/en/json-attribute-functions.html#function_json-length)
- [JSON_MERGE()（已弃用）](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge)
- [JSON_MERGE_PATCH()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-patch)
- [JSON_MERGE_PRESERVE()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-preserve)
- [JSON_OBJECT()](https://dev.mysql.com/doc/refman/8.0/en/json-creation-functions.html#function_json-object)
- [JSON_OVERLAPS()（8.0.17 引入）](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-overlaps)
- [JSON_PRETTY()](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html#function_json-pretty)
- [JSON_QUOTE()](https://dev.mysql.com/doc/refman/8.0/en/json-creation-functions.html#function_json-quote)
- [JSON_REMOVE()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-remove)
- [JSON_REPLACE()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-replace)
- [JSON_SCHEMA_VALID()（8.0.17 引入）](https://dev.mysql.com/doc/refman/8.0/en/json-validation-functions.html#function_json-schema-valid)
- [JSON_SCHEMA_VALIDATION_REPORT()（8.0.17 引入）](https://dev.mysql.com/doc/refman/8.0/en/json-validation-functions.html#function_json-schema-validation-report)
- [JSON_SEARCH()](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-search)
- [JSON_SET()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-set)
- [JSON_STORAGE_FREE()](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html#function_json-storage-free)
- [JSON_STORAGE_SIZE()](https://dev.mysql.com/doc/refman/8.0/en/json-utility-functions.html#function_json-storage-size)
- [JSON_TABLE()](https://dev.mysql.com/doc/refman/8.0/en/json-table-functions.html#function_json-table)
- [JSON_TYPE()](https://dev.mysql.com/doc/refman/8.0/en/json-attribute-functions.html#function_json-type)
- [JSON_UNQUOTE()](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-unquote)
- [JSON_VALID()](https://dev.mysql.com/doc/refman/8.0/en/json-attribute-functions.html#function_json-valid)
- [JSON_VALUE()（8.0.21 引入）](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#function_json-value)
- [MEMBER OF()（8.0.17 引入）](https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html#operator_member-of)

### Cast

- [BINARY](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#operator_binary)
- [CAST()](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_cast)
- [CONVERT()](https://dev.mysql.com/doc/refman/8.0/en/cast-functions.html#function_convert)

### 流量控制

- [CASE](https://dev.mysql.com/doc/refman/8.0/en/flow-control-functions.html#operator_case)
- [IF()](https://dev.mysql.com/doc/refman/8.0/en/flow-control-functions.html#function_if)
- [IFNULL()](https://dev.mysql.com/doc/refman/8.0/en/flow-control-functions.html#function_ifnull)
- [NULLIF()](https://dev.mysql.com/doc/refman/8.0/en/flow-control-functions.html#function_nullif)

### 信息

- [BENCHMARK()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_benchmark)
- [CHARSET()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_charset)
- [COERCIBILITY()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_coercibility)
- [COLLATION()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_collation)
- [CONNECTION_ID()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_connection-id)
- [CURRENT_ROLE()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_current-role)
- [CURRENT_USER()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_current-user)
- [DATABASE()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_database)
- [FOUND_ROWS()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_found-rows)
- [ICU_VERSION()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_icu-version)
- [LAST_INSERT_ID()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id)
- [ROLES_GRAPHML()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_roles-graphml)
- [ROW_COUNT()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_row-count)
- [SCHEMA()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_schema)
- [SESSION_USER()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_session-user)
- [SYSTEM_USER()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_system-user)
- [USER()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_user)
- [VERSION()](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_version)

### 加密和压缩

- [AES_DECRYPT()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_aes-decrypt)
- [AES_ENCRYPT()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_aes-encrypt)
- [COMPRESS()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_compress)
- [MD5()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_md5)
- [RANDOM_BYTES()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_random-bytes)
- [SHA1(), SHA()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_sha1)
- [SHA2()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_sha2)
- [STATEMENT_DIGEST()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_statement-digest)
- [STATEMENT_DIGEST_TEXT()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_statement-digest-text)
- [UNCOMPRESS()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_uncompress)
- [UNCOMPRESSED_LENGTH()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_uncompressed-length)
- [VALIDATE_PASSWORD_STRENGTH()](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html#function_validate-password-strength)

### 锁定

- [GET_LOCK()](https://dev.mysql.com/doc/refman/8.0/en/locking-functions.html#function_get-lock)
- [IS_FREE_LOCK()](https://dev.mysql.com/doc/refman/8.0/en/locking-functions.html#function_is-free-lock)
- [IS_USED_LOCK()](https://dev.mysql.com/doc/refman/8.0/en/locking-functions.html#function_is-used-lock)
- [RELEASE_ALL_LOCKS()](https://dev.mysql.com/doc/refman/8.0/en/locking-functions.html#function_release-all-locks)
- [RELEASE_LOCK()](https://dev.mysql.com/doc/refman/8.0/en/locking-functions.html#function_release-lock)

### 少量

- [&](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html#operator_bitwise-and)
- [>>](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html#operator_right-shift)
- [<<](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html#operator_left-shift)
- [^](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html#operator_bitwise-xor)
- [BIT_COUNT()](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html#function_bit-count)
- [|](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html#operator_bitwise-or)
- [~](https://dev.mysql.com/doc/refman/8.0/en/bit-functions.html#operator_bitwise-invert)

### 各种各样的

- [ANY_VALUE()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_any-value)
- [BIN_TO_UUID()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_bin-to-uuid)
- [DEFAULT()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_default)
- [GROUPING()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_grouping)
- [INET_ATON()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet-aton)
- [INET_NTOA()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet-ntoa)
- [INET6_ATON()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet6-aton)
- [INET6_NTOA()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_inet6-ntoa)
- [IS_IPV4()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_is-ipv4)
- [IS_IPV4_COMPAT()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_is-ipv4-compat)
- [IS_IPV4_MAPPED()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_is-ipv4-mapped)
- [IS_IPV6()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_is-ipv6)
- [IS_UUID()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_is-uuid)
- [MASTER_POS_WAIT()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_master-pos-wait)
- [NAME_CONST()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_name-const)
- [SLEEP()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_sleep)
- [UUID()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_uuid)
- [UUID_SHORT()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_uuid-short)
- [UUID_TO_BIN()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_uuid-to-bin)
- [VALUES()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_values)

## 另见

- [MySQL 中的正则表达式](https://www.w3cschool.cn/regexp/regexp-dgbp3khn.html#regex-in-mysql) *(quickref.me)*