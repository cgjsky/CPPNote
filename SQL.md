# 基础

## 查询

```sql
select * from table
select distinct column_name from table 

//增加条件
select column_name from table
where column_name operator value
eg:
SELECT * FROM Websites WHERE id=1;
```

SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。

![image-20220324132507179](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324132507179.png)

![image-20220324132650423](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324132650423.png)

## 排序

```sql
select * from table
order by column_name //默认升序
order by column_name desc //降序
order by column_name,column_name //可以多个列排序
```

## 插入

```sql
//1.不指定列名
INSERT INTO table_name
VALUES (value1,value2,value3,...);
没有指定要插入数据的列名的形式需要列出插入行的每一列数据

//2.指定列名
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);

注意到，没有向 id 字段插入任何数字？
id 列是自动更新的，表中的每条记录都有一个唯一的数字。

```

## 更新

```sql
//更新哪个表，set更新的值，选出被更新的语句
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE some_column=some_value;
```

## 删除

```sql
DELETE FROM table_name
WHERE some_column=some_value;
```

# 高阶

## 选取记录

```sql
SELECT * FROM Websites LIMIT 2;
SELECT TOP 50 PERCENT * FROM Websites;
select top 5 * from table order by id desc 
```

## 通配符

![image-20220324134206603](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324134206603.png)

## 别名

```sql
SELECT name AS n, country AS c
FROM Websites;
//我们把三个列（url、alexa 和 country）结合在一起，并创建一个名为 "site_info" 的别名：
select name ,contat('url',',','alexa',',','country') as site_info
from websites
```

在下面的情况下，使用别名很有用：

- 在查询中涉及超过一个表
- 在查询中使用了函数
- 列名称很长或者可读性差
- 需要把两个列或者多个列结合在一起

```sql
SELECT w.name, w.url, a.count, a.date 
FROM Websites AS w, access_log AS a 
WHERE a.site_id=w.id and w.name="菜鸟教程";
```

![image-20220324135904621](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324135904621.png)

## UNION

连接两个select

```sql
无重复
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
```

```sql
有重复
SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country;
```

## 复制

```sql
//插入新表，newtable原本不存在
select column
into newtable
from oldtable

//插入老表,table1,2都存在
INSERT INTO table2
SELECT * FROM table1;
```

## 创建database

```sql
CREATE DATABASE dbname;
```

## 创建table

```sql
CREATE TABLE Persons
(
PersonID int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);
```

## 约束

![image-20220324142912499](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324142912499.png)

```sql
CREATE TABLE Persons
(
    Id_P int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    PRIMARY KEY (Id_P)  //PRIMARY KEY约束
)
CREATE TABLE Persons
(
    Id_P int NOT NULL PRIMARY KEY,   //PRIMARY KEY约束
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)
```

需要定义多个约束时

```sql
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
)

```

当表已被创建时，如需在 "P_Id" 列创建 UNIQUE 约束，请使用下面的 SQL：

```sql
ALTER TABLE Persons
ADD UNIQUE (P_Id)
```

```sql
ALTER TABLE Persons
ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
```

## 撤销约束

```sql
//mysql
ALTER TABLE Persons
DROP INDEX uc_PersonID

//SQL Server / Oracle / MS Access：
ALTER TABLE Persons
DROP CONSTRAINT uc_PersonID
```

## 外键

```
一个表中的 FOREIGN KEY 指向另一个表中的 UNIQUE KEY(唯一约束的键)。

```

## check

```sql

CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (P_Id>0)
)
```

# 函数

```sql
//AVG
SELECT AVG(column_name) FROM table_name

//COUNT，NULL不计入
SELECT COUNT(column_name) FROM table_name;
//count(*)返回记录数
SELECT COUNT(*) FROM table_name;
SELECT COUNT(DISTINCT column_name) FROM table_name;

//MAX
SELECT MAX(column_name) FROM table_name;

//MIN
SELECT MIN(column_name) FROM table_name;

//SUM
SELECT SUM(column_name) FROM table_name;

//Groupby
SELECT column_name, function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;

//HAVING，功能与where类似，但是where不能筛选聚合函数
SELECT Websites.name, Websites.url, SUM(access_log.count) AS nums FROM (access_log
INNER JOIN Websites
ON access_log.site_id=Websites.id)
GROUP BY Websites.name
HAVING SUM(access_log.count) > 200;

```

![image-20220324152030275](/Users/chenguanjin/Library/Application Support/typora-user-images/image-20220324152030275.png)

```sql
//大小写转换
//mysql
SELECT UCASE(name) AS site_title, url
FROM Websites;
SELECT LCASE(name) AS site_title, url
FROM Websites;
//sqlsever
SELECT UPPER(name) AS site_title, url FROM Websites;
SELECT LOWER (name) AS site_title, url FROM Websites;
```

```sql
//MID,从文本中提取值，起始值必须，默认从1开始
SELECT MID(column_name,start[,length]) FROM table_name;

//LEN
SELECT name, LENGTH(url) as LengthOfURL
FROM Websites;

//ROUND，保留小数位数
SELECT ROUND(column_name,decimals) FROM TABLE_NAME;
```

