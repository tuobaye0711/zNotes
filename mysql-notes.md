[原文链接](https://github.com/qianlei90/Blog/issues/10)

# 《MySQL必知必会》笔记

------

## 第2章 MySQL简介

```
mysql -u ben -p -h myserver -P 9999
```

`-u`:user
`-p`:need password
`-h`:host
`-P`:port，默认3306

## 第3章 使用MySQL

```
USE yourdb;
```

使用某个数据库

```
SHOW DATABASES/TABLES;
SHOW COLUMNS FROM yourtable; == DESCRIBE yourtable;
```

显示已经存在的数据库/表/某表中的列

`SHOW STATUS`:显示服务器状态信息
`SHOW CREATE DATABASE/TABLE`:显示创建数据库/表的SQL语句
`SHOW GRANTS`:显示授权用户的安全权限
`SHOW ERRORS/WARNINGS`:显示服务器错误或警告信息

## 第4章 检索数据

```
SELECT * FROM yourtable;
SELECT columnA， columnB， columnC FROM yourtable;
```

SQL语句不区分大小写，忽略空格，以分号结尾

```
SELECT DISTINCT colunmA FROM yourtable;
```

返回结果去重

```
SELECT columnA FROM yourtable LIMIT 5;
SELECT colunmA FROM yourtable LIMIT 5，6/LIMIT 4 OFFSET 6;
```

限制返回结果为5行/5～5+6行

```
SELECT tableA.colunmA FROM dbA.tableA
```

## 第5章 排序检索数据

```
SELECT colunmA, colunmB, columnC FROM tableA ORDER BY colunmA DESC, colunmC;
```

从表tableA中选取colunmA,colunmB,colunmC列并按colunmA降序,colunmC升序的顺序排列
如果需要在多个列降序，需要在每个列中指定DESC

## 第6章 过滤数据

WHERE子句的操作符:`=`,`!=`,`<>`,`<`,`<=`,`>`,`>=`,`BETWEEN`

```
SELECT colunmA FROM tableA WHERE colunmA BETWEEN num1 AND num2;
SELECT colunmA FROM tableA WHERE colunmA IS NULL;
```

## 第7章 数据过滤

```
SELECT colunmA, colunmB FROM tableA
WHERE colunmA = num1 OR colunmA =num2 AND colunmB >= num3;
```

`AND`比`OR`有更高的优先级，所以应该用括号

```
SELECT colunmA, colunmB FROM tableA
WHERE (colunmA = num1 OR colunmA =num2) AND colunmB >= num3;
```

```
SELECT colunmA, colunmB FROM tableA
WHERE colunmA IN/NOT IN (num1, num2)
ORDER BY colunmB
```

`IN`与`OR`的功能相同,`NOT`用来取反

## 第8章 用通配符进行过滤

使用LIKE关键字，区分大小写，影响性能少用
`%`:任何字符出现任意次数
`_`:任何字符出现1次

```
SELECT colunmA FROM tableA WHERE colunmA LIKE '%char1_char2';
```

## 第9章 用正则表达式进行搜索

`LIKE`匹配整个列，`REGEXP`匹配部分
MySQL中，正则表达式的前导为`\\`，用来匹配特殊字符，如`\\\`用来匹配`\`

## 第10章 创建计算字段

`Concat()`:字符串拼接
`RTrim()`:去掉右侧空格
`AS`:使用别名

```
SELECT Concat(RTrim(colunmA), ' (', RTrim(colunmB), ' )') AS colunmC
FROM tableA ORDER BY colunmA
```

## 第11章 使用数据处理函数

MySQL的日期格式为`yyyy-mm-dd`

字符串函数：
`Upper()`:转换为大写
`Left()`:返回串左边的字符
`Length()`:返回串的长度
`Locate()`:找出一个串的子串
`Lower()`:将串转换为小写
`LTrim()`:去掉串左边的空格
`Right()`:返回串右边的字符
`RTrim()`:去掉串右边的空格
`Soundex()`:返回串的SOUNDEX值
`SubString()`:返回子串的字符
`Upper()`:将串转换为大写

```
SELECT colunmA, colunmB FROM tableA
WHERE Soundex(colunmA) = Soundex(colunmB)
```

时间函数：
`AddDate()`:增加一个日期（天、周等）
`AddTime()`:增加一个时间（时、分等）
`CurDate()`:返回当前日期
`CurTime()`:返回当前时间
`Date()`:返回日期时间的日期部分
`DateDiff()`:计算两个日期之差
`Date_Add()`:高度灵活的日期运算函数
`Date_Format()`:返回一个格式化的日期或时间串
`Day()`:返回一个日期的天数部分
`DayOfWeek()`:对于一个日期，返回对应的星期几
`Hour()`:返回一个时间的小时部分
`Minute()`:返回一个时间的分钟部分
`Month()`:返回一个日期的月份部分
`Now()`:返回当前日期和时间
`Second()`:返回一个时间的秒部分
`Time()`:返回一个日期时间的时间部分
`Year()`:返回一个日期的年份部分

```
SELECT colunmA, colunmB FROM tableA
WHERE Data(colunmA) BETWEEN '2005-09-01' AND '2005-09-30'
```

数值函数：
`Abs()`:返回一个数的绝对值
`Cos()`:返回一个角度的余弦
`Exp()`:返回一个数的指数值
`Mod()`:返回除操作的余数
`Pi()`:返回圆周率
`Rand()`:返回一个随机数

## 第12章 汇总数据

聚集函数:
`AVG()`:返回某列的平均值,忽略NULL行
`COUNT()`:返回某列的行数
`MAX()`:返回某列的最大值
`MIN()`:返回某列的最小值
`SUM()`:返回某列值之和

```
SELECT AVG(DISTINCT colunmA) AS name1 FROM tableA
WHERE colunmB = numB
```

从tableA中选取colunmB列的值为numB的行，并计算这些行中不同colunmA的平均值

## 第13章 分组数据

分组:把数据分为多个逻辑组，对每个组进行聚集计算
一般在使用`GROUP BY`时也使用`ORDER BY`,保证数据排序的唯一性,不依赖`GROUP BY`的排序

```
SELECT colunmA, COUNT(*) AS name1 FROM tableA
WHERE colunmB >= numB
GROUP BY colunmA
HAVING COUNT(*) >= numC
ORDER BY colunmA
```

`WHERE`在分组前进行,约束来之数据库的数据,在结果返回前起作用,不能使用聚集函数
`HAVING`在分组后进行,在查询返回结果集以后对结果进行过滤,可以使用聚集函数

## 第14章 使用子查询

```
SELECT cust_name,
       cust_state,
       (SELECT COUNT(*) FROM orders WHERE orders.cust_id = customers.cust_id) AS orders
FROM customers
ORDER BY cust_name;
```

```
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                  FROM orders
                  WHERE order_num IN (SELECT order_num
                                      FROM orderitems
                                      WHERE prod_id = 'TNT2'));
```

## 第15章 联结表

2个表：

```
SELECT A.name, B.name, B.price FROM A INNER JOIN B ON A.id = B.id;
```

多个表：

```
SELECT A.name, A.contact FROM A, B, C
WHERE A.id = B.id AND B.num = C.num AND C.id = '102';
```

不要联接太多的表，影响数据库性能

## 第16章 创建高级联接

自联接：

```
SELECT A.id, A.name FROM A AS a, A AS b
WHERE a.id = b.id AND b.id = 'DTNTR';
```

自然联接：

排除多次出现的列，使每个列只返回一次

外部联接：

包括那些没有关联的行，`LEFT OUTER JOIN`左边的表中所有的行都联接右边表中的行

```
SELECT A.id, B.num FROM A LEFT OUTER JOIN B ON A.id = B.id;
```

## 第17章 创建组合查询

```
SELECT id, num, price FROM A WHERE price <=5
UNION
SELECT id, num, price FROM B WHERE id IN (1001, 1002);
```

`UNION`中每个查询必须包含相同的列，表达式或聚集函数，列的数据类型必须兼容
`UNION`自动去除重复的行，如果想返回所有匹配的行，使用`UNION ALL`
`ORDER BY`只能放在最后，只能对整体结果排序

## 第18章 全文本搜索

启用：在建表时加入`FULLTEXT`子句，仅在MyISAM中支持

```
CREATE TABLE A
(
    id      int         NOT NULL AUTO_INCREMENT,
    data    datetime    NOT NULL,
    text    text        NULL,
    PRIMARY KEY(id),
    FULLTEXT(text)
)ENGINE = MyISAM;
```

- 如果一个词在50%以上的行中出现，则MySQL将它作为一个非用词忽略
- 表的行数少于3行，则全文搜索补返回结果
- 忽略词中的单引号
- 没有词分隔符的语言不能恰当的返回结果
- 仅在MyISAM引擎中支持全文本搜索

全文本搜索：

```
SELECT text FROM A WHERE Match(text) Against('keywords');
```

MySql对所有行计算一个等级值，返回等级值非0的行

查询扩展

```
SELECT text FROM A
WHERE Match(text) Against('keywords' WITH QUERY EXPANSION);
```

对关键字所在行的所有单词再进行一次全文本搜索

布尔文本搜索

```
SELECT text FROM A
WHERE Match(text) Against('keywords -rope*' IN BOOLEAN MODE);
```

搜索包含keywords但不包含任意以rope开始的行

## 第19章 插入数据

插入多条数据:

```
INSERT INTO table(colA, colB, colC)
VALUES(1, 'TEXT1', NULL), (2, 'TEXT2', default_value);
```

`VALUE`与表中的列一一对应
对于省略的某些列，有如下条件:

1. 允许为NULL
2. 有默认值
   如果不允许NULL也没有默认值，则在插入时会报错

插入检索出的数据:

```
INSERT INTO tableA(colA, colB, colC) SELECT A, B, C FROM tableB;

```

从tableB中检索A,B,C列的数据，插入到表tableA中去

## 第20章 更新和删除数据

更新某行的制定列，**不要省略WHERE子句**，若省略表示更新所有行

```
UPDATE customers SET cust_name = 'A', cust_email = 'A@B.com' WHERE cust_id = 1005;

```

删除某行，类似UPDATE

```
DELETE FROM tableA WHERE id = 1005;

```

## 第21章 创建和操纵表

创建一个表

```
CREATE TABLE A IF NOT EXISTS
(
    id      int         NOT NULL AUTO_INCREMENT=1,
    name    char(50)    NOT NULL,
    address char(50)    NULL DEFAULT "default value",
    PRIMARY KEY (id, name)
)ENGINE=InnoDB;

```

增加一列

```
ALTER TABLE A ADD phone char(20);

```

删除一列

```
ALTER TABLE A DROP COLUMN phone;

```

重命名表名

```
RENAME TABLE A TO B;
```
