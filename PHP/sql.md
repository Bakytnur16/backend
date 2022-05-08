#### SQL structured query language 是用于管理关系数据库管理系统（RDBMS）。 SQL 的范围包括数据插入、查询、更新和删除，数据库模式创建和修改，以及数据访问控制。
- 数据库由若干张表(Table)组成
- 主键的值不修改 不反复使用  
- 注释：# --注释 /* */  

SELECT - 从数据库中提取数据  
UPDATE - 更新数据库中的数据  
DELETE - 从数据库中删除数据  
INSERT INTO - 向数据库中插入新数据  
CREATE DATABASE - 创建新数据库  
ALTER DATABASE - 修改数据库  
CREATE TABLE - 创建新表  
ALTER TABLE - 变更（改变）数据库表  
DROP TABLE - 删除表  
CREATE INDEX - 创建索引（搜索键）  
DROP INDEX - 删除索引  
TRUNCATE TABLE table_name 仅删除表内的数据  


## create 
```
CREATE DATABASE my_db;  
CREATE TABLE table_name  
(  
column_name1 data_type(size), LastName varchar(255) NOT NULL  
column_name2 data_type(size),  
column_name3 data_type(size),  
.... ); 
CREATE INDEX index_name ON table_name (column_name) 
```
## alter
ALTER TABLE table_name ADD column_name datatype  
ALTER TABLE table_name DROP COLUMN column_name  
ALTER TABLE .. ADD col CHAR(20);  


## insert into
```
INSERT INTO mytable(col1,col2) VALUES(vali1,vali2)  
- 第一种形式无需指定要插入数据的列名，只需提供被插入的值即可：  
INSERT INTO table_name VALUES (value1,value2,value3,...);  
- 第二种形式需要指定列名及被插入的值：  
INSERT INTO table_name (column1,column2,column3,...) VALUES (value1,value2,value3,...);  

INSERT INTO mytable1(col1, col2) SELECT col1, col2 FROM mytable2;  

insert into select 和select into from 的区别  

insert into scorebak select * from socre where neza='neza'   --插入一行,要求表scorebak 必须存在  
select *  into scorebak from score  where neza='neza'  --也是插入一行,要求表scorebak 不存在  
```

### select
```
SELECT TOP 子句用于规定要返回的记录的数目。
SELECT column_name,column_name FROM table_name;
SELECT * FROM table_name;
返回第 3 ~ 5 行：SELECT * FROM mytable LIMIT 3, 5;
SELECT * FROM mytable WHERE col IS NULL;
SELECT Websites.name, Websites.url, access_log.count, access_log.date FROM Websites, access_log
WHERE Websites.id=access_log.site_id and Websites.name="菜鸟教程";

查询DISTINCT：相同值只会出现一次。它作用于所有列，也就是说所有列的值都相同才算相同。(在表中，一个列可能会包含多个重复值，有时您也许希望仅仅列出不同（distinct）的值。DISTINCT 关键词用于返回唯一不同的值。)

SELECT DISTINCT col1, col2 FROM mytable;

从一个表复制数据到另一个表: CREATE TABLE newtable AS SELECT * FROM mytable;
从一个表中复制所有的列插入到另一个已存在的表中： INSERT INTO table2 SELECT * FROM table1;
```
### where
```
SELECT column_name,column_name FROM table_name WHERE column_name operator value;
SELECT * FROM Websites WHERE country='CN';
SELECT * FROM Websites WHERE id=1;
```
### and&or
```
如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。
如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。

SELECT * FROM Websites WHERE country='CN' AND alexa > 50; 
SELECT * FROM Websites WHERE country='USA' OR country='CN';
SELECT * FROM Websites WHERE alexa > 15 AND (country='CN' OR country='USA');
```
### order by
```
SELECT column_name,column_name FROM table_name ORDER BY column_name,column_name ASC|DESC;

排练多列 SELECT * FROM Websites ORDER BY country,alexa;
降序SELECT * FROM Websites ORDER BY alexa DESC;
SELECT * FROM mytable ORDER BY col1 DESC, col2 ASC;
```
### update
UPDATE table_name SET column1=value1,column2=value2,... WHERE some_column=some_value;   
UPDATE mytable SET col = val WHERE id = 1;   

### delete
```
DELETE FROM mytable WHERE id = 1;
DROP COLUMN col;
DROP TABLE ..;
```
### like
```
SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern;
SELECT * FROM WebsitesWHERE name LIKE 'G%'; G开头
SELECT * FROM Websites WHERE name LIKE '%k'; k结尾
SELECT * FROM Websites WHERE name LIKE '%oo%'; 选取 name 包含模式 "oo" 的所有客户
SELECT * FROM Websites WHERE name LIKE '_oogle';以一个任意字符开始，然后是 "oogle" 的所有客户
SELECT * FROM Websites WHERE name LIKE 'G_o_le';  "G" 开始，然后任意字符，然后 "o"，然后任意字符，然后是 "le" 的所有网站
SELECT * FROM Websites WHERE name REGEXP '^[GFs]';  name 以 "G"、"F" 或 "s" 开始的所有网站
SELECT * FROM Websites WHERE name REGEXP '^[A-H]'; 以 A 到 H 字母开头的网站
SELECT * FROM Websites WHERE name REGEXP '^[^A-H]';不以A 到 H 字母开头的网站
```
#### 通配符
%替代0和多个字符   
_ 代替一个字符   

### in
```
SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...);
SELECT * FROM Websites WHERE name IN ('Google','菜鸟教程');
```
### between
```
SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;
SELECT * FROM Websites WHERE alexa BETWEEN 1 AND 20; 包括1和20
SELECT * FROM Websites WHERE alexa NOT BETWEEN 1 AND 20;
SELECT * FROM Websites WHERE (alexa BETWEEN 1 AND 20) AND country NOT IN ('USA', 'IND');
SELECT * FROM Websites WHERE name BETWEEN 'A' AND 'H';
```
### as 
```
计算字段通常需要使用 AS 来取别名，否则输出的时候字段名为计算表达式。
SELECT column_name AS alias_name FROM table_name;
SELECT column_name(s) FROM table_name AS alias_name;
SELECT name, CONCAT(url, ', ', alexa, ', ', country) AS site_info FROM Websites;
SELECT name AS n, country AS c FROM Websites;
SELECT col1*col2 AS alias FROM mytable
SELECT w.name, w.url, a.count, a.date FROM Websites AS w, access_log AS a WHERE a.site_id=w.id and w.name="菜鸟教程";
```
### join
连接用于连接多个表，使用 JOIN 关键字，并且条件语句使用 ON 而不是 Where。
![image](https://user-images.githubusercontent.com/64322636/167281142-4a481cfc-81b0-47c7-b0ed-437190f53146.png)
```
内连接又称等值连接，使用 INNER JOIN 关键字。
SELECT column_name(s) FROM table1 INNER JOIN table2
ON table1.column_name=table2.column_name;

select a, b, c from A inner join B on A.key = B.key
SELECT Websites.id, Websites.name, access_log.count, access_log.date FROM Websites
INNER JOIN access_log ON Websites.id=access_log.site_id;

INNER JOIN：如果表中有至少一个匹配，则返回行
LEFT JOIN：即使右表中没有匹配，也从左表返回所有的行
RIGHT JOIN：即使左表中没有匹配，也从右表返回所有的行
FULL JOIN：只要其中一个表中存在匹配，则返回行
```
![image](https://user-images.githubusercontent.com/64322636/167281284-40537162-3b03-4fe7-8dc2-4bffd86d60f3.png)
```
SELECT column_name(s) FROM table1 LEFT JOIN table2
ON table1.column_name=table2.column_name;

select Customers.cust_id, Orders.order_num
   from Customers left outer join Orders
   on Customers.cust_id = Orders.curt_id;
```
### union
```
SQL UNION 操作符合并两个或多个 SELECT 语句的结果。
SELECT column_name(s) FROM table1 UNION SELECT column_name(s) FROM table2;
SELECT column_name(s) FROM table1 UNION ALL SELECT column_name(s) FROM table2;如果允许重复的值，使用 UNION ALL

SELECT country, name FROM Websites WHERE country='CN' UNION ALL
SELECT country, app_name FROM apps WHERE country='CN' ORDER BY country;

使用 UNION 来组合两个查询，如果第一个查询返回 M 行，第二个查询返回 N 行，那么组合查询的结果为 M+N 行。

SELECT col FROM mytable WHERE col = 1 UNION
SELECT col FROM mytable WHERE col =2;

NOT NULL - 不能存储 NULL 值；
UNIQUE - 保证某列每行必须有唯一的值。
PRIMARY KEY - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
FOREIGN KEY - 保证一个表中的数据匹配另一个表中的值的参照完整性。
CHECK - 保证列中的值符合指定的条件。
DEFAULT - 规定没有给列赋值时的默认值。
AUTO_INCREMENT 
```

### 视图
```
CREATE VIEW view_name AS SELECT column_name(s) FROM table_name WHERE condition
- CREATE VIEW [Products Above Average Price] AS SELECT ProductName,UnitPrice FROM Products
WHERE UnitPrice>(SELECT AVG(UnitPrice) FROM Products)

CREATE OR REPLACE VIEW view_name AS SELECT column_name(s) FROM table_name WHERE condition

date
NOW()	返回当前的日期和时间
CURDATE()	返回当前的日期
CURTIME()	返回当前的时间
DATE()	提取日期或日期/时间表达式的日期部分
EXTRACT()	返回日期/时间的单独部分
DATE_ADD()	向日期添加指定的时间间隔
DATE_SUB()	从日期减去指定的时间间隔
DATEDIFF()	返回两个日期之间的天数
DATE_FORMAT()	用不同的格式显示日期/时间
```
### 函数
```
SQL ISNULL()、NVL()、IFNULL() 和 COALESCE() 函数
SELECT AVG(column_name) FROM table_name
SELECT COUNT(*) FROM table_name;
SELECT COUNT(column_name) FROM table_name;
SELECT FIRST(column_name) FROM table_name;
AVG() - 返回平均值
COUNT() - 返回行数
FIRST() - 返回第一个记录的值
LAST() - 返回最后一个记录的值
MAX() - 返回最大值
MIN() - 返回最小值
SUM() - 返回总和
UCASE() - 将某个字段转换为大写
LCASE() - 将某个字段转换为小写
MID() - 从某个文本字段提取字符，MySql 中使用
SubString(字段，1，end) - 从某个文本字段提取字符
LEN() - 返回某个文本字段的长度
ROUND() - 对某个数值字段进行指定小数位数的四舍五入
NOW() - 返回当前的系统日期和时间
FORMAT() - 格式化某个字段的显示方式

SELECT Websites.name, Websites.url FROM Websites   
WHERE EXISTS (SELECT count FROM access_log WHERE Websites.id = access_log.site_id AND count > 200);  
SELECT MID(name,1,4) AS ShortTitle FROM Websites; 从 "Websites" 表的 "name" 列中提取前 4 个字符  
SELECT name, url, DATE_FORMAT(Now(),'%Y-%m-%d') AS date FROM Websites;  
```
