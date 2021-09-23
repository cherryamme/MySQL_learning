<!-- order:15 -->
## MySQL 常用函数

### 时间函数

#### `date_add()、date_sub()`

增加/减少一天

```sql
 DATE_ADD(date,INTERVAL expr type)
```

| Type 值            |
| :----------------- |
| MICROSECOND        |
| SECOND             |
| MINUTE             |
| HOUR               |
| DAY                |
| WEEK               |
| MONTH              |
| QUARTER            |
| YEAR               |
| SECOND_MICROSECOND |
| MINUTE_MICROSECOND |
| MINUTE_SECOND      |
| HOUR_MICROSECOND   |
| HOUR_SECOND        |
| HOUR_MINUTE        |
| DAY_MICROSECOND    |
| DAY_SECOND         |
| DAY_MINUTE         |
| DAY_HOUR           |
| YEAR_MONTH         |

#### `datediff()`

DATEDIFF() 函数返回两个日期之间的天数。

```sql
DATEDIFF(date1,date2)
```

#### `date_format()`

DATE_FORMAT() 函数用于以不同的格式显示日期/时间数据。

 ```sql
 DATE_FORMAT(date,format)
 ```

| 格式 | 描述                                           |
| :--- | :--------------------------------------------- |
| %a   | 缩写星期名                                     |
| %b   | 缩写月名                                       |
| %c   | 月，数值                                       |
| %D   | 带有英文前缀的月中的天                         |
| %d   | 月的天，数值（00-31）                          |
| %e   | 月的天，数值（0-31）                           |
| %f   | 微秒                                           |
| %H   | 小时（00-23）                                  |
| %h   | 小时（01-12）                                  |
| %I   | 小时（01-12）                                  |
| %i   | 分钟，数值（00-59）                            |
| %j   | 年的天（001-366）                              |
| %k   | 小时（0-23）                                   |
| %l   | 小时（1-12）                                   |
| %M   | 月名                                           |
| %m   | 月，数值（00-12）                              |
| %p   | AM 或 PM                                       |
| %r   | 时间，12-小时（hh:mm:ss AM 或 PM）             |
| %S   | 秒（00-59）                                    |
| %s   | 秒（00-59）                                    |
| %T   | 时间, 24-小时（hh:mm:ss）                      |
| %U   | 周（00-53）星期日是一周的第一天                |
| %u   | 周（00-53）星期一是一周的第一天                |
| %V   | 周（01-53）星期日是一周的第一天，与 %X 使用    |
| %v   | 周（01-53）星期一是一周的第一天，与 %x 使用    |
| %W   | 星期名                                         |
| %w   | 周的天（0=星期日, 6=星期六）                   |
| %X   | 年，其中的星期日是周的第一天，4 位，与 %V 使用 |
| %x   | 年，其中的星期一是周的第一天，4 位，与 %v 使用 |
| %Y   | 年，4 位                                       |
| %y   | 年，2 位                                       |

**示例**

```sql
select  
DATE_FORMAT(NOW(),'%b %d %Y %h:%i %p'),
DATE_FORMAT(NOW(),'%m-%d-%Y'),
DATE_FORMAT(NOW(),'%d %b %y'),
DATE_FORMAT(NOW(),'%d %b %Y %T:%f')
```

结果如下所示：

```sql
 Nov 04 2008 11:45 PM
 11-04-2008
 04 Nov 08
 04 Nov 2008 11:45:34:243 
```

#### `now()、curdate()、curtime()`

NOW() 返回当前的日期和时间。

| NOW()               | CURDATE()  | CURTIME() |
| :------------------ | :--------- | :-------- |
| 2008-11-11 12:45:34 | 2008-11-11 | 12:45:34  |

#### `Date()`

DATE() 函数提取日期或日期/时间表达式的日期部分。

`2008-11-11 13:23:44.657` → `2008-11-11`

#### `extract()`

EXTRACT() 函数用于返回日期/时间的单独部分，比如年、月、日、小时、分钟等等。

```sql
 EXTRACT(unit FROM date)
```

date 参数是合法的日期表达式。unit 参数可以是下列的值：

| Unit 值            |
| :----------------- |
| MICROSECOND        |
| SECOND             |
| MINUTE             |
| HOUR               |
| DAY                |
| WEEK               |
| MONTH              |
| QUARTER            |
| YEAR               |
| SECOND_MICROSECOND |
| MINUTE_MICROSECOND |
| MINUTE_SECOND      |
| HOUR_MICROSECOND   |
| HOUR_SECOND        |
| HOUR_MINUTE        |
| DAY_MICROSECOND    |
| DAY_SECOND         |
| DAY_MINUTE         |
| DAY_HOUR           |
| YEAR_MONTH         |

### 字符串连接函数

#### `Concat()、concat_ws()、group_concat()`

字符串连接函数 

使用方法：
CONCAT(str1,str2,…) 

CONCAT_WS(separator,str1,str2,...)

group_concat([DISTINCT] 要连接的字段 [Order BY ASC/DESC 排序字段] [Separator '分隔符'])

**CONCAT()**返回结果为连接参数产生的字符串。如有任何一个参数为NULL ，则返回值为 NULL。

> 注意：
> 如果所有参数均为非二进制字符串，则结果为非二进制字符串。 
> 如果自变量中含有任一二进制字符串，则结果为一个二进制字符串。
> 一个数字参数被转化为与之相等的二进制字符串格式；若要避免这种情况，可使用显式类型 cast

**CONCAT_WS()** 代表 CONCAT With Separator ，是CONCAT()的特殊形式。第一个参数是其它参数的分隔符。分隔符的位置放在要连接的两个字符串之间。分隔符可以是一个字符串，也可以是其它参数。

> 和MySQL中concat函数不同的是, concat_ws函数在执行的时候,不会因为NULL值而返回NULL 。
>
> 如果分隔符为 NULL，则结果为 NULL。函数会忽略任何分隔符参数后的 NULL 值。



**Group_concat()：**

```sql
# 以id分组，把name字段的值打印在一行，逗号分隔(默认)
select id,group_concat(name) from aa group by id;

# 以id分组，把name字段的值打印在一行，分号分隔
select id,group_concat(name separator ';') from aa group by id;

# 以id分组，把去冗余的name字段的值打印在一行
select id,group_concat(distinct name) from aa group by id;

# 以id分组，把name字段的值打印在一行，逗号分隔，以name排倒序
select id,group_concat(name order by name desc) from aa group by id;
```

#### `repeat()`

  用来复制字符串,如下'ab'表示要复制的字符串，2表示复制的份数

```sql
select repeat('ab',2);
```



### 字符串截取函数

#### **1、从左开始截取字符串** 

left（str, length） 
说明：left（被截取字段，截取长度） 
例：

```sql
select left(content,200) as abstract from my_content_t 
```

#### **2、从右开始截取字符串** 

right（str, length） 
说明：right（被截取字段，截取长度） 
例：

```sql
select right(content,200) as abstract from my_content_t 
```

#### **3、截取字符串 **

substring（str, pos） 
substring（str, pos, length） 
说明：substring（被截取字段，从第几位开始截取，截取长度） 

例：

```sql
select substring(content,5) as abstract from my_content_t 
select substring(content,5,200) as abstract from my_content_t 
```

> （注：如果位数是负数 如-5 则是从后倒数位数，到字符串结束或截取的长度） 



#### **4、按关键字截取字符串** 

substring_index（str,delim,count） 
说明：substring_index（被截取字段，分隔符，关键字出现的次数） 

例：

```sql
select substring_index("www.baidu.com",".",2) as abstract from wiki_user 
# （注：如果关键字出现的次数是负数 如-2 则是从后倒数，到字符串结束） 
# 结果 www.baidu
```



### 数学函数

#### ABS()——绝对值

`ABS(X)`返回 `X` 的绝对值：

```sql
mysql> SELECT ABS(2); -> 2 mysql> SELECT ABS(-32); -> 32
```

s这个函数可安全地使用于 `BIGINT` 值。

#### sign()——取正负

`SIGN(X)`以 `-1`、`0` 或 `1` 方式返回参数的符号，它取决于参数 `X` 是负数、0 或正数。

```sql
mysql> SELECT SIGN(-32); -> -1 mysql> SELECT SIGN(0); -> 0 mysql> SELECT SIGN(234); -> 1
```

#### MOD()——取模

`MOD(N,M)`% 取模 (就如 C 中的 `%` 操作符)。返回 `N` 被 `M` 除后的余数：

```sql
mysql> SELECT MOD(234, 10); -> 4 mysql> SELECT 253 % 7; -> 1 mysql> SELECT MOD(29,9); -> 2 mysql> SELECT 29 MOD 9; -> 2
```

这个函数可安全地使用于 `BIGINT` 值。最后一个示例可在 MySQL 4.1 中工作。

#### Floor()、ceiling()、round()、truncate() ——取整

`FLOOR(X)`返回不大于 `X` 的最大整数值：

```sql
mysql> SELECT FLOOR(1.23); -> 1 mysql> SELECT FLOOR(-1.23); -> -2
```

> 注意，返回值被转换为一个 `BIGINT`！
>
> 可用DIV()替换该函数

`CEILING(X)`返回不小于 `X` 的最小整数：

```sql
mysql> SELECT CEILING(1.23); -> 2 mysql> SELECT CEILING(-1.23); -> -1
```

注意，返回值被转换为一个 `BIGINT`！

`ROUND(X,D)`将参数 `X` 四舍五入到最近的整数，然后返回。两个参数的形式是将一个数字四舍五入到 `D` 个小数后返回。

```sql
SELECT ROUND(-1.23，2);
```

> 注意，当参数在两个整数之间时， `ROUND()` 的行为取决于 C 库的实现。某些取整到最近的偶数，总是向下取，总是向上取，也可能总是接近于零。如果你需要某种取整类型，应该使用一个明确定义的函数比如 `TRUNCATE()` 或 `FLOOR()` 代替。

`TRUNCATE(X,D)`将数值 `X` 截到 `D` 个小数，然后返回。如果 `D` 为 `0`，结果将不包含小数点和小数部分

直接截取取数。

#### 计算符

- `EXP(X)`返回值 `e` (自然对数的底) 的 `X` 次方：

  `mysql> SELECT EXP(2); -> 7.389056 mysql> SELECT EXP(-2); -> 0.135335`

- `LN(X)`返回 `X` 的自然对数：

  `mysql> SELECT LN(2); -> 0.693147 mysql> SELECT LN(-2); -> NULL`这个函数在 MySQL 4.0.3 被新加入。在 MySQL 中，它是 `LOG(X)` 的同义词。

- `LOG(X)`

- `LOG(B,X)`如果以一个参数调用，它返回 `X` 的自然对数：

  `mysql> SELECT LOG(2); -> 0.693147 mysql> SELECT LOG(-2); -> NULL`如果以两个参数调用，这个函数返回 `X` 任意底 `B` 的对数：`mysql> SELECT LOG(2,65536); -> 16.000000 mysql> SELECT LOG(1,100); -> NULL`任意底选项在 MySQL 4.0.3 中被加入。`LOG(B,X)` 等价于 `LOG(X)/LOG(B)`。

- `LOG2(X)`返回 `X` 的以 2 为底的对数：

  `mysql> SELECT LOG2(65536); -> 16.000000 mysql> SELECT LOG2(-100); -> NULL``LOG2()` 通常可以用于计数出一个数字需要多少个比特位用于存储它。这个函数在 MySQL 4.0.3 中被添加。在更早的版本中，可以使用 `LOG(X)/LOG(2)` 来代替它。

- `LOG10(X)`返回 `X` 以 10 为底的对数：

  `mysql> SELECT LOG10(2); -> 0.301030 mysql> SELECT LOG10(100); -> 2.000000 mysql> SELECT LOG10(-100); -> NULL`

- `POW(X,Y)`

- `POWER(X,Y)`返回 `X` 的 `Y` 幂：

  `mysql> SELECT POW(2,2); -> 4.000000 mysql> SELECT POW(2,-2); -> 0.250000`

- `SQRT(X)`返回 `X` 的非否平方根：

  `mysql> SELECT SQRT(4); -> 2.000000 mysql> SELECT SQRT(20); -> 4.472136`

- `PI()`返回 PI 值(圆周率)。缺少显示 5 位小数，但是在 MySQL 内部，为 PI 使用全部的双精度。

  `mysql> SELECT PI(); -> 3.141593 mysql> SELECT PI()+0.000000000000000000; -> 3.141592653589793116`

- `COS(X)`返回 `X` 的余弦，在这里，`X` 以弧度给出：

  `mysql> SELECT COS(PI()); -> -1.000000`

- `SIN(X)`返回 `X` 的正弦，在这里，`X` 以弧度给出：

  `mysql> SELECT SIN(PI()); -> 0.000000`

- `TAN(X)`返回 `X` 的正切，在这里，`X` 以弧度给出：

  `mysql> SELECT TAN(PI()+1); -> 1.557408`

- `ACOS(X)`返回 `X` 的反余弦，更确切地说，返回余弦值为 `X` 的值。如果 `X` 不在 `-1` 到 `1` 之间的范围内，返回 `NULL`：

  `mysql> SELECT ACOS(1); -> 0.000000 mysql> SELECT ACOS(1.0001); -> NULL mysql> SELECT ACOS(0); -> 1.570796`

- `ASIN(X)`返回 `X` 的反正弦，更确切地说，返回正弦值为 `X` 的值。如果 `X` 不在 `-1` 到 `1` 之间的范围内，返回 `NULL`：

  `mysql> SELECT ASIN(0.2); -> 0.201358 mysql> SELECT ASIN('foo'); -> 0.000000`

- `ATAN(X)`返回 `X` 的反正切，更确切地说，返回正切值为 `X` 的值：

  `mysql> SELECT ATAN(2); -> 1.107149 mysql> SELECT ATAN(-2); -> -1.107149`

- `ATAN(Y,X)`

- `ATAN2(Y,X)`返回两个变量 `X` 和 `Y` 的反正切。它类似于计算 `Y / X` 的反正切，除了两个参数的符号用于决定结果的象限：

  `mysql> SELECT ATAN(-2,2); -> -0.785398 mysql> SELECT ATAN2(PI(),0); -> 1.570796`

- `COT(X)`返回 `X` 的余切：

  `mysql> SELECT COT(12); -> -1.57267341 mysql> SELECT COT(0); -> NULL`

#### rand() ——取随机

`RAND(N)`返回一个范围在 `0` 到 `1.0` 之间的随机浮点值。如果一个整数参数 `N` 被指定，它被当做种子值使用(用于产生一个可重复的数值)：

