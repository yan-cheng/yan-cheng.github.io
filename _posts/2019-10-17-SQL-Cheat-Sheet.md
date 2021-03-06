---
title: SQL Cheat Sheet
description: SQL Cheat Sheet
categories:
 - Cheat Sheet
tags:
 - SQL
---

SQL(结构化查询语言)分为两个部分：数据操作语言 (DML) 和 数据定义语言 (DDL)。

查询和更新指令构成了 SQL 的 DML 部分：

- `SELECT` - 从数据库表中获取数据
- `UPDATE` - 更新数据库表中的数据
- `DELETE` - 从数据库表中删除数据
- `INSERT INTO` - 向数据库表中插入数据

SQL 的 DDL 部分使我们有能力创建或删除表格：

- `CREATE DATABASE` - 创建新数据库
- `ALTER DATABASE` - 修改数据库
- `CREATE TABLE` - 创建新表
- `ALTER TABLE` - 变更（改变）数据库表
- `DROP TABLE` - 删除表
- `CREATE INDEX` - 创建索引（搜索键）
- `DROP INDEX` - 删除索引

# SQL Manipulate Statement

## SELECT

`SELECT` 语句用于从表中选取数据。结果被存储在一个结果集中。关键词 `DISTINCT` 用于返回唯一不同的值。

```sql
SELECT  DISTINCT column_name FROM table_name
```

示例：

```sql
SELECT LastName,FirstName FROM Persons;

SELECT * FROM Persons;

SELECT DISTINCT Company FROM Orders;
```

## SELECT TOP

`TOP` 子句用于规定要返回的记录的数目。对于拥有数千条记录的大型表来说，`TOP` 子句是非常有用的。并非所有的数据库系统都支持 `TOP` 子句。

```sql
SELECT TOP number|percent column_name(s) FROM table_name
```

示例：

```sql
SELECT TOP 2 * FROM Persons;

SELECT TOP 50 PERCENT * FROM Persons;
```

- MySQL 和 Oracle 中与 SQL 中 `SELECT TOP` 等价的语句是：

  MySQL：`SELECT column_name(s) FROM table_name LIMIT number`

  Oracle：`SELECT column_name(s) FROM table_name WHERE rownum <= number`

## ORDER BY

`ORDER BY` 语句用于根据指定的列对结果集进行排序。`ORDER BY` 语句默认按照升序 `ASC` 对记录进行排序。如果您希望按照降序对记录进行排序，可以使用 `DESC` 关键字。

示例：
```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber;

SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC;

SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC;
```

## WHERE

如需有条件地从表中选取数据，可将 `WHERE` 子句添加到 `SELECT` 语句。

```sql
SELECT column_name(s) FROM table_name WHERE column_name operator value
```

下面的运算符可在 `WHERE` 子句中使用：

| 操作符            | 描述              |
|:--:|:-:|
| =                 | 等于              |
| <>                | 等于              |
| >                 | 大于              |
| <                 | 小于              |
| >=                | 大于等于          |
| <=                | 小于等于          |
| BETWEEN           | 在某个范围内      |
| LIKE              | 搜索某种模式      |

- 在某些版本的 SQL 中，操作符 `<>` 可以写为 `!=` 。
- 我们在例子中的条件值周围使用的是单引号。SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。如果是数值，请不要使用引号。

示例：

```sql
SELECT * FROM Persons WHERE City='Beijing';

SELECT * FROM Persons WHERE Year>1965;
```

## AND，OR 和 NOT 

`AND`，`OR` 和 `NOT` 可在 `WHERE` 子语句中把两个或多个条件结合起来。

示例：

```sql
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter';

SELECT * FROM Persons WHERE (FirstName='Thomas' OR FirstName='William') AND NOT LastName='Carter';
```

## LIKE

`LIKE` 操作符用于在 `WHERE` 子句中搜索列中的指定模式。SQL 通配符可以替代一个或多个字符。SQL 通配符必须与 `LIKE` 运算符一起使用。

```sql
SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern
```

在 SQL 中，可使用以下通配符：

| 通配符                     | 描述                       |
|:-:|:-:|
| %                         | 替代一个或多个字符            |
| _                         | 仅替代一个字符               |
|[charlist]               | 字符列中的任何单一字符        |
|[^charlist]              | 不在字符列中的任何单一字符     |
|[!charlist]              | 不在字符列中的任何单一字符     |


示例：

```sql
-- 从 "Persons" 表中选取出不居住在包含 "lond" 的城市里的人
SELECT * FROM Persons WHERE City NOT LIKE '%lon%'

-- 从 "Persons" 表中选取姓氏以 "C" 开头，然后是任意字符，然后是 "er"：
SELECT * FROM Persons WHERE LastName LIKE 'C_er'

-- 从 "Persons" 表中选取居住的城市不以 "A" 或 "L" 或 "N" 开头的人：
SELECT * FROM Persons WHERE City LIKE '[!ALN]%'
```

## INSERT INTO 

`INSERT INTO` 语句用于向表格中插入新的行。

```sql
INSERT INTO  table_name (column_name1, column_name2,…) VALUES (value1, value2,…)
```

示例：

```sql
INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing');

INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees');
```

## INSERT INTO SELECT

`INSERT INTO SELECT` 语句从一个表中拷贝数据并插入到另外一张表中。选取的源表和目的表的列必须数目相同，并且列的数据类型一致。

```sql
INSERT INTO table2 (column1, column2, column3, ...) 
    SELECT column_name(s) FROM table1 WHERE condition;
```

示例：
```sql
-- 将 "Suppliers" 表中的各列插入到 "Customers" 表对应的列中
INSERT INTO Customers (CustomerName, ContactName, Address, City, Country)
    SELECT SupplierName, ContactName, Address, City, Country FROM Suppliers;
```

## Update

`Update` 语句用于修改表中的数据。

```sql
UPDATE table_name SET column_name = value1 WHERE column_name = value2
```


示例：

```sql
UPDATE Person SET FirstName='Fred' WHERE LastName='Wilson';
UPDATE Person SET Address='Zhongshan', City='Nanjing' WHERE LastName ='Wilson';
```

## DELETE

`DELETE` 语句用于删除表中的行。

```sql
DELETE FROM table_name WHERE column_name = value
```

示例：

```sql
DELETE FROM Person WHERE LastName='Wilson';

DELETE * FROM table_name;
```

## IN

`IN` 操作符允许我们在 `WHERE` 子句中规定多个值。

```sql
SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...)
```

示例：

```sql
SELECT * FROM Persons WHERE LastName IN ('Adams','Carter')
```

## BETWEEN

操作符 `BETWEEN ... AND` 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

```sql
SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2
```

示例：

```sql
SELECT * FROM Persons WHERE LastName BETWEEN 'Adams' AND 'Carter';

SELECT * FROM Persons WHERE LastName NOT BETWEEN 'Adams' AND 'Carter';
```

不同的数据库对 `BETWEEN...AND` 操作符的处理方式是有差异的。某些数据库会列出介于 "Adams" 和 "Carter" 之间的人，但不包括 "Adams" 和 "Carter" ；某些数据库会列出介于 "Adams" 和 "Carter" 之间并包括 "Adams" 和 "Carter" 的人；而另一些数据库会列出介于 "Adams" 和 "Carter" 之间的人，包括 "Adams" ，但不包括 "Carter" 。所以，请检查你的数据库是如何处理 `BETWEEN....AND` 操作符的！

## AS

表的 SQL Alias 语法

```sql
SELECT column_name(s) FROM table_name AS alias_name
``` 

列的 SQL Alias 语法

```sql
SELECT column_name AS alias_name FROM table_name
```

示例：

```sql
SELECT po.OrderID, p.LastName, p.FirstName FROM Persons AS p, Product_Orders
    AS po WHERE p.LastName='Adams' AND p.FirstName='John'

SELECT LastName AS Family, FirstName AS Name FROM Persons
```

## JOIN

`JOIN` 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。下面列出了您可以使用的 `JOIN` 类型，以及它们之间的差异。

- `JOIN`: 即 `INNER` `JOIN`，如果表中有至少一个匹配，则返回行

 ```sql
SELECT column_name(s) FROM table_name1 INNER JOIN table_name2 
    ON table_name1.column_name=table_name2.column_name
```

- `LEFT JOIN`: 即使右表中没有匹配，也从左表返回所有的行

```sql
SELECT column_name(s) FROM table_name1 LEFT JOIN table_name2 
    ON table_name1.column_name=table_name2.column_name
```

- `RIGHT JOIN`: 即使左表中没有匹配，也从右表返回所有的行

```sql
SELECT column_name(s) FROM table_name1 RIGHT JOIN table_name2 
    ON table_name1.column_name=table_name2.column_name
```

- `FULL JOIN`: 只要其中一个表中存在匹配，就返回行

```sql
SELECT column_name(s) FROM table_name1 FULL JOIN table_name2 
    ON table_name1.column_name=table_name2.column_name
```

示例：

```sql
SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo FROM Persons
    INNER JOIN Orders ON Persons.Id_P = Orders.Id_P ORDER BY Persons.LastName
```

## UNION

`UNION` 操作符用于合并两个或多个 `SELECT` 语句的结果集。请注意，`UNION` 内部的 `SELECT` 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 `SELECT` 语句中的列的顺序必须相同。另外，`UNION` 结果集中的列名总是等于 `UNION` 中第一个 `SELECT` 语句中的列名。

```sql
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) `FROM` table_name2
```

默认地，`UNION` 操作符选取不同的值。如果允许重复的值，请使用 `UNION ALL`。

```sql
SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
```

示例：

```sql
SELECT E_Name FROM Employees_China
UNION
SELECT E_Name FROM Employees_USA

SELECT 'Customer' As Type, ContactName, City, Country
FROM Customers
UNION
SELECT 'Supplier', ContactName, City, Country
FROM Suppliers
```

## SELECT INTO

`SELECT INTO` 语句从一个表中选取数据，然后把数据插入另一个表中。`SELECT INTO` 语句常用于创建表的备份复件或者用于对记录进行存档。

```sql
SELECT column_name(s) INTO new_table_name [IN externaldatabase] FROM old_tablename
```

示例：

```sql
-- 制作 "Persons" 表的备份表 "Persons_backup"：
SELECT * INTO Persons_backup FROM Persons

-- IN 子句可用于向另一个数据库中拷贝表：
SELECT * INTO Persons IN 'Backup.mdb' FROM Persons

-- 创建一个名为 "Persons_Order_Backup" 的新表，其中包含了从 "Persons" 和 "Orders" 两个表中取得的信息：
SELECT Persons.LastName,Orders.OrderNo
    INTO Persons_Order_Backup
    FROM Persons
    INNER JOIN Orders
    ON Persons.Id_P=Orders.Id_P
```

## GROUP BY

`GROUP BY` 语句用于结合合计函数（比如 `SUM` 函数），根据一个或多个列对结果集进行分组。

```sql
SELECT column_name(s), aggregate_function(column_name)
FROM table_name
WHERE condition
GROUP BY column_name(s)
```

示例：

```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country

SELECT Customer,OrderDate,SUM(OrderPrice) FROM Orders
GROUP BY Customer,OrderDate

SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders
FROM Orders
LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName;
```

## HAVING

在 SQL 中增加 `HAVING` 子句原因是，`WHERE` 关键字无法与合计函数一起使用。

```sql
SELECT column_name(s), aggregate_function(column_name)
    FROM table_name
    WHERE condition
    GROUP BY column_name(s)
    HAVING condition
```

示例：

```sql
-- 列出每个国家的顾客数量，只选择顾客数量大于5的：
SELECT COUNT(CustomerID), Country
    FROM Customers
    GROUP BY Country
    HAVING COUNT(CustomerID) > 5;

-- 列出 "Davolio" 和 "Fuller" 两位雇员的订单数量，只选择订单数量大于25的：
SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
    FROM Orders
    INNER JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
    WHERE LastName = 'Davolio' OR LastName = 'Fuller'
    GROUP BY LastName
    HAVING COUNT(Orders.OrderID) > 25;
```

## EXISTS

`EXISTS` 操作符用来测试一个子查询里某个记录是否存在。如果子查询返回一个或多个记录，`EXISTS` 操作符则返回真。

```sql
SELECT column_name(s) FROM table_name
    WHERE EXISTS (SELECT column_name FROM table_name WHERE condition);
```

示例：

```sql
-- 下面的 SQL 返回 TRUE，并列出产品价格小于20的供应商
SELECT SupplierName
    FROM Suppliers
    WHERE EXISTS (SELECT ProductName FROM Products WHERE SupplierId=
        Suppliers.supplierId AND Price < 20)
```

## ANY ALL

`ANY` 和 `ALL` 操作符与 `WHERE` 子句或 `HAVING` 子句一起使用。当一个子查询中任何一条记录满足条件时，`ANY` 操作符返回真；当一个子查询中所有记录满足条件时，`ALL` 操作符返回真。

`ANY`:

```sql
SELECT column_name(s) FROM table_name
    WHERE column_name operator ANY
    (SELECT column_name FROM table_name WHERE condition);
```

`ALL`:

```sql
SELECT column_name(s) FROM table_name
    WHERE column_name operator ALL
    (SELECT column_name FROM table_name WHERE condition);
```

- 这里的 operator 必须是标准比较操作符 (`=`， `<>`， `!=`， `>`， `>=`， `<`， 或 `<=`)。

示例：
```sql
SELECT ProductName FROM Products
    WHERE ProductID = ANY (SELECT ProductID FROM OrderDetails WHERE Quantity = 10);

SELECT ProductName FROM Products
    HAVING COUNT(ProductNum) > ALL (SELECT COUNT(ProductNum)
    FROM OrderDetails WHERE Quantity = 10);
```

## NULL

`NULL` 值代表遗漏的未知数据。在默认的情况下，表的列接受 `NULL` 值。`NOT NULL` 约束强制列不接受 `NULL` 值。`NOT NULL` 约束强制字段始终包含值。

示例：

```sql
-- 下面的 SQL 强制 "P_Id" 列和 "LastName" 列不接受 NULL 值
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)

SELECT LastName, FirstName, Address FROM Persons WHERE Address IS NULL;
```

# SQL Database Statement

## SQL Data Types

数据类型定义列中存放的值的种类。

### MySQL Data Types

在 MySQL 中，有三种主要数据类型：文本 (text)，数字 (number) 和时间 (date)。

**文本**：

| Data type | Description |
|:-|:-|
|CHAR(size)       |(8) Holds a fixed length string (can contain letters, numbers, and special characters). The fixed size is specified in parenthesis. Can store up to 255 characters                                                                                                                                     |
|VARCHAR(size)    |(8) Holds a variable length string (can contain letters, numbers, and special characters). The maximum size is specified in parenthesis. Can store up to 255 characters. Note: If you put a greater value than 255 it will be converted to a TEXT type                                                 |
|TINYTEXT         |(8) Holds a string with a maximum length of 255 characters                                                                                                                                                                                                                                      |
|TEXT             |(16) Holds a string with a maximum length of 65,535 characters                                                                                                                                                                                                                                  |
|MEDIUMTEXT       |(24) Holds a string with a maximum length of 16,777,215 characters                                                                                                                                                                                                                              |
|LONGTEXT         |(32) Holds a string with a maximum length of 4,294,967,295 characters                                                                                                                                                                                                                           |
|BLOB             |(16) For BLOBs (Binary Large OBjects). Holds up to 65,535 bytes of data                                                                                                                                                                                                                         |
|MEDIUMBLOB       |(24) For BLOBs (Binary Large OBjects). Holds up to 16,777,215 bytes of data                                                                                                                                                                                                                     |
|LONGBLOB         |(32) For BLOBs (Binary Large OBjects). Holds up to 4,294,967,295 bytes of data                                                                                                                                                                                                                  |
|ENUM(x,y,z,...)  |Let you enter a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the list, a blank value will be inserted. Note: The values are sorted in the order you enter them. You enter the possible values in this format: ENUM('X','Y','Z').|
|SET              |Similar to ENUM except that SET may contain up to 64 list items and can store more than one choice                                                                                                                                                                                                 

**数字**：

| Data type | Description |
|:-|:-|
|TINYINT(size)   |(8) -128 to 127 normal. 0 to 255 UNSIGNED. The maximum number of digits may be specified in parenthesis                                                                                                                                 |
|SMALLINT(size)  |(16) -32768 to 32767 normal. 0 to 65535 UNSIGNED. The maximum number of digits may be specified in parenthesis                                                                                                                           |
|MEDIUMINT(size) |(32) -8388608 to 8388607 normal. 0 to 16777215 UNSIGNED. The maximum number of digits may be specified in parenthesis                                                                                                                    |
|INT(size)       |(32) -2147483648 to 2147483647 normal. 0 to 4294967295 UNSIGNED. The maximum number of digits may be specified in parenthesis                                                                                                            |
|BIGINT(size)    |(64) -9223372036854775808 to 9223372036854775807 normal. 0 to 18446744073709551615 UNSIGNED*. The maximum number of digits may be specified in parenthesis                                                                                |
|FLOAT(size,d)   |A small number with a floating decimal point. The maximum number of digits may be specified in the size parameter. The maximum number of digits to the right of the decimal point is specified in the d parameter                    |
|DOUBLE(size,d)  |A large number with a floating decimal point. The maximum number of digits may be specified in the size parameter. The maximum number of digits to the right of the decimal point is specified in the d parameter                    |
|DECIMAL(size,d) |A DOUBLE stored as a string , allowing for a fixed decimal point. The maximum number of digits may be specified in the size parameter. The maximum number of digits to the right of the decimal point is specified in the d parameter|

**时间**：

| Data type | Description |
|:-|:-|
|DATE()      |A date. Format: YYYY-MM-DD. The supported range is from '1000-01-01' to '9999-12-31'                                                                                                                                                                                                                                               |
|DATETIME()  |A date and time combination. Format: YYYY-MM-DD HH:MI:SS. The supported range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'                                                                                                                                                                                               |
|TIMESTAMP() |A timestamp. TIMESTAMP values are stored as the number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC). Format: YYYY-MM-DD HH:MI:SS. TIMESTAMP also accepts various formats, like YYYYMMDDHHMISS, YYMMDDHHMISS, YYYYMMDD, or YYMMDD. The supported range is from '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07' UTC|
|TIME()      |A time. Format: HH:MI:SS. The supported range is from '-838:59:59' to '838:59:59'                                                                                                                                                                                                                                                  |
|YEAR()      |A year in two-digit or four-digit format. Values allowed in four-digit format: 1901 to 2155. Values allowed in two-digit format: 70 to 69, representing years from 1970 to 2069                                                                                                                                                    |

### SQL Server Data Types

**字符串类型**：

| Data type     | Description                    | Max size                | Storage                 |
|:--------------|:-------------------------------|:------------------------|:------------------------|
|char(n)        |Fixed width character string    |8,000 characters         |Defined width            |
|varchar(n)     |Variable width character string |8,000 characters         |2 bytes + number of chars|
|varchar(max)   |Variable width character string |1,073,741,824 characters |2 bytes + number of chars|
|text           |Variable width character string |2GB of text data         |4 bytes + number of chars|
|nchar          |Fixed width Unicode string      |4,000 characters         |Defined width x 2        |
|nvarchar       |Variable width Unicode string   |4,000 characters                                  ||
|nvarchar(max)  |Variable width Unicode string   |536,870,912 characters                            ||
|ntext          |Variable width Unicode string   |2GB of text data                                  ||
|binary(n)      |Fixed width binary string       |8,000 bytes                                       ||
|varbinary      |Variable width binary string    |8,000 bytes                                       ||
|varbinary(max) |Variable width binary string    |2GB                                               ||
|image          |Variable width binary string    |2GB                                               ||

**数字类型**：

| Data type | Description | Storage |
|:-|:-|:-|
|bit          |Integer that can be 0, 1, or NULL  |             |
|tinyint      |Allows whole numbers from 0 to 255  |1 byte       |
|smallint     |Allows whole numbers between -32,768 and 32,767 |2 bytes      |
|int          |Allows whole numbers between -2,147,483,648 and 2,147,483,647 |4 bytes      |
|bigint       |Allows whole numbers between -9,223,372,036,854,775,808 and 9,223,372,036,854,775,807 |8 bytes      |
|decimal(p,s) |Fixed precision and scale numbers. Allows numbers from -10^38^ +1 to 10^38^ –1. The p parameter indicates the maximum total number of digits that can be stored (both to the left and to the right of the decimal point). p must be a value from 1 to 38. Default is 18. The s parameter indicates the maximum number of digits stored to the right of the decimal point. s must be a value from 0 to p. Default value is 0 |5-17 bytes   |
|numeric(p,s) |Fixed precision and scale numbers. Allows numbers from -10^38^ +1 to 10^38^ –1. The p parameter indicates the maximum total number of digits that can be stored (both to the left and to the right of the decimal point). p must be a value from 1 to 38. Default is 18. The s parameter indicates the maximum number of digits stored to the right of the decimal point. s must be a value from 0 to p. Default value is 0 | 5-17 bytes  |
|smallmoney   |Monetary data from -214,748.3648 to 214,748.3647 |4 bytes      |
|money        |Monetary data from -922,337,203,685,477.5808 to 922,337,203,685,477.5807 |8 bytes      |
|float(n)     |Floating precision number data from -1.79E + 308 to 1.79E + 308. The n parameter indicates whether the field should hold 4 or 8 bytes. float(24) holds a 4-byte field and float(53) holds an 8-byte field. Default value of n is 53. |4 or 8 bytes |
|real         |Floating precision number data from -3.40E + 38 to 3.40E + 38 |4 bytes      |

**时间类型**：

| Data type   | Description  | Storage  |
|:-|:-|:-|
|datetime       |From January 1, 1753 to December 31, 9999 with an accuracy of 3.33 milliseconds|8 bytes|
|datetime2      |From January 1, 0001 to December 31, 9999 with an accuracy of 100 nanoseconds|6-8 bytes|
|smalldatetime  |From January 1, 1900 to June 6, 2079 with an accuracy of 1 minute|4 bytes|
|date           |Store a date only. From January 1, 0001 to December 31, 9999 |3 bytes|
|time           |Store a time only to an accuracy of 100 nanoseconds|3-5 bytes|
|datetimeoffset |The same as datetime2 with the addition of a time zone offset|8-10 bytes|
|timestamp      |Stores a unique number that gets updated every time a row gets created or modified. The timestamp value is based upon an internal clock and does not correspond to real time. Each table may have only one timestamp variable|        |

**其他类型**：

| Data type       | Description  |
|:-|:-|
|sql_variant      |Stores up to 8,000 bytes of data of various data types, except text, ntext, and timestamp|
|uniqueidentifier |Stores a globally unique identifier (GUID) |
|xml              |Stores XML formatted data. Maximum 2GB |
|cursor           |Stores a reference to a cursor used for database operations |
|table            |Stores a result-set for later processing |



## SQL Operators

### SQL Arithmetic Operators

| Operator          | Description       |
|:-:|:-:|
|+                  |Add                |
|-                  |Subtract           |
|*                  |Multiply           |
|%                  |Modulo             |
|/                  |Divide             |

### SQL Bitwise Operators

| Operator          | Description        |
|:-:|:-:|
|&                  |Bitwise AND         |
|\|                 |Bitwise OR          |
|^                  |Bitwise exclusive OR|


### SQL Comparison Operators

| Operator          | Description            |
|:-:|:-:|
|=                  |Equal to                |
|>                  |Greater than            |
|<                  |Less than               |
|>=                 |Greater than or equal to|
|<=                 |Less than or equal to   |
|<>                 |Not equal to            |

### SQL Compound Operators

| Operator          | Description       |
|:-:|:-:|
|+=	|Add equals|
|-=	|Subtract equals|
|*=	|Multiply equals|
|/=	|Divide equals|
|%=	|Modulo equals|
|&=	|Bitwise AND equals|
|^-=	|Bitwise exclusive equals|
|\|*=	|Bitwise OR equals|

### SQL Logical Operators

| Operator          | Description                                                |
|:-:|:-|
|ALL                |TRUE if all of the subquery values meet the condition       |
|AND                |TRUE if all the conditions separated by AND is TRUE         |
|OR                 |TRUE if any of the conditions separated by OR is TRUE       |
|NOT                |Displays a record if the condition(s) is NOT TRUE           |
|ANY                |TRUE if any of the subquery values meet the condition       |
|SOME               |TRUE if any of the subquery values meet the condition       |
|BETWEEN            |TRUE if the operand is within the range of comparisons      |
|IN                 |TRUE if the operand is equal to one of a list of expressions|
|LIKE               |TRUE if the operand matches a pattern                       |
|EXISTS             |TRUE if the subquery returns one or more records            |


## CREATE DATABASE

`CREATE DATABASE` 语句用于创建数据库。

```sql
CREATE DATABASE dbname;
```

示例：

```sql
CREATE DATABASE my_db;
```

## DROP DATABASE

`DROP DATABASE` 语句用于删除数据库。

```
DROP DATABASE database_name
```

示例：

```sql
DROP DATABASE my_db;
```

## CREATE TABLE
`CREATE TABLE` 语句用于创建数据库中的表。表由行和列组成，每个表都必须有个表名。

```sql
CREATE TABLE table_name
(
    column_name1 data_type(size),
    column_name2 data_type(size),
    column_name3 data_type(size),
    ....
);
```

column_name 参数规定表中列的名称。data_type 参数规定列的数据类型（例如 varchar、integer、decimal、date 等等）。size 参数规定表中列的最大长度。

示例：

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
## DROP TABLE

`DROP TABLE` 语句用于删除表。

```sql
DROP TABLE table_name
```

## TRUNCATE TABLE

如果我们仅仅需要删除表内的数据，但并不删除表本身，使用 `TRUNCATE TABLE` 语句：

```sql
TRUNCATE TABLE table_name
```

## ALTER TABLE

`ALTER TABLE` 语句用于在已有的表中添加、删除或修改列。

如需在表中添加列，请使用下面的语法:

```sql
ALTER TABLE table_name ADD column_name datatype;
```

如需删除表中的列，请使用下面的语法（请注意，某些数据库系统不允许这种在数据库表中删除列的方式）：

```sql
ALTER TABLE table_name DROP COLUMN column_name
```

要改变表中列的数据类型，请使用下面的语法：

SQL Server / MS Access：

```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype;
```

My SQL / Oracle：

```sql
ALTER TABLE table_name
MODIFY COLUMN column_name datatype;
```

Oracle 10G 之后版本:

```sql
ALTER TABLE table_name
MODIFY column_name datatype;
```

示例：

```sql
-- 在 "Persons" 表中添加一个类型是 date 的 "DateOfBirth" 列
ALTER TABLE Persons ADD DateOfBirth date

-- 删除 "Person" 表中的 "DateOfBirth" 列
ALTER TABLE Persons DROP COLUMN DateOfBirth

-- 把 "Persons" 表中 "DateOfBirth" 列的数据类型改为 year，可以存放 2 位或 4 位格式的年份
ALTER TABLE Persons ALTER COLUMN DateOfBirth year
```
## Constraints

SQL 约束用于规定表中的数据规则。如果存在违反约束的数据行为，行为会被约束终止。约束可以在创建表时规定（通过 `CREATE TABLE` 语句），或者在表创建之后规定（通过 `ALTER TABLE` 语句）。

```sql
CREATE TABLE table_name
(
    column_name1 data_type(size) constraint_name,
    column_name2 data_type(size) constraint_name,
    column_name3 data_type(size) constraint_name,
    ....
);
```

在 SQL 中，我们有如下约束：

- `NOT NULL` - 指示某列不能存储 `NULL` 值。
- `UNIQUE` - 保证某列的每行必须有唯一的值。
- `PRIMARY KEY` - `NOT NULL` 和 `UNIQUE` 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- `FOREIGN KEY` - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- `CHECK` - 保证列中的值符合指定的条件。
- `DEFAULT` - 规定没有给列赋值时的默认值。

### NOT NULL

`NOT NULL` 约束强制列不接受 `NULL` 值。`NOT NULL` 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

示例：

```sql
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)
```

### UNIQUE

`UNIQUE` 约束唯一标识数据库表中的每条记录。`UNIQUE` 和 `PRIMARY KEY` 约束均为列或列集合提供了唯一性的保证。`PRIMARY KEY` 约束拥有自动定义的 UNIQUE 约束。
请注意，每个表可以有多个 `UNIQUE` 约束，但是每个表只能有一个 `PRIMARY KEY` 约束。

- `CREATE TABLE` 时的 `SQL UNIQUE` 约束

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 `UNIQUE` 约束：

```sql

-- MySQL：
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    UNIQUE (P_Id)
)

-- SQL Server / Oracle / MS Access：
CREATE TABLE Persons
(
    P_Id int NOT NULL UNIQUE,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)
```

如需命名 `UNIQUE` 约束，并定义多个列的 `UNIQUE` 约束，请使用下面的 SQL 语法：

```sql
# MySQL / SQL Server / Oracle / MS Access：
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

- `ALTER TABLE` 时的 SQL `UNIQUE` 约束

当表已被创建时，如需在 "P_Id" 列创建 UNIQUE 约束，请使用下面的 SQL：

```sql
-- MySQL / SQL Server / Oracle / MS Access：
ALTER TABLE Persons ADD UNIQUE (P_Id)
```

如需命名 `UNIQUE` 约束，并定义多个列的 `UNIQUE` 约束，请使用下面的 SQL 语法：

```sql
-- MySQL / SQL Server / Oracle / MS Access：
ALTER TABLE Persons ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
```

- 撤销 `UNIQUE` 约束：

```sql
-- MySQL：
ALTER TABLE Persons DROP INDEX uc_PersonID

-- SQL Server / Oracle / MS Access：
ALTER TABLE Persons DROP CONSTRAINT uc_PersonID
```

### PRIMARY KEY

主键 (`PRIMARY KEY`) 唯一标识数据库表中的每条记录。主键必须包含唯一的值。主键列不能包含 `NULL` 值。每个表只能有一个主键，一个主键可能由单个或多个字段组成。

- `CREATE TABLE` 时的 SQL `PRIMARY KEY` 约束

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 `PRIMARY KEY` 约束：

```sql
-- MySQL：
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    PRIMARY KEY (P_Id)
)

-- SQL Server / Oracle / MS Access：
CREATE TABLE Persons
(
    P_Id int NOT NULL PRIMARY KEY,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)
```

如需命名 `PRIMARY KEY` 约束，并定义多个列的 `PRIMARY KEY` 约束，请使用下面的 SQL 语法：

```sql
-- MySQL / SQL Server / Oracle / MS Access：
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
)
```

在上面的实例中，只有一个主键 `PRIMARY KEY`（"pk_PersonID"）。然而，"pk_PersonID" 的值是由两个列（"P_Id"" 和 "LastName"）组成的。

### FOREIGN KEY

一个表中的 `FOREIGN KEY` 指向另一个表中的 `PRIMARY KEY`。`FOREIGN KEY` 约束用于预防破坏表之间连接的行为。`FOREIGN KEY` 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

- `CREATE TABLE` 时的 SQL `FOREIGN KEY` 约束

下面的 SQL 在 "Orders" 表创建时在 "P_Id" 列上创建 `FOREIGN KEY` 约束：

```sql
-- MySQL：
CREATE TABLE Orders
(
    O_Id int NOT NULL,
    OrderNo int NOT NULL,
    P_Id int,
    PRIMARY KEY (O_Id),
    FOREIGN KEY (P_Id) REFERENCES Persons(P_Id)
)

-- SQL Server / Oracle / MS Access：
CREATE TABLE Orders
(
    O_Id int NOT NULL PRIMARY KEY,
    OrderNo int NOT NULL,
    P_Id int FOREIGN KEY REFERENCES Persons(P_Id)
)
```

如需命名 `FOREIGN KEY` 约束，并定义多个列的 `FOREIGN KEY` 约束，请使用下面的 SQL 语法：

```sql
-- MySQL / SQL Server / Oracle / MS Access：
CREATE TABLE Orders
(
    O_Id int NOT NULL,
    OrderNo int NOT NULL,
    P_Id int,
    PRIMARY KEY (O_Id),
    CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id)
    REFERENCES Persons(P_Id)
)
```

- `ALTER TABLE` 时的 SQL `FOREIGN KEY` 约束

当 "Orders" 表已被创建时，如需在 "P_Id" 列创建 `FOREIGN KEY` 约束，请使用下面的 SQL：

```sql
-- MySQL / SQL Server / Oracle / MS Access：
ALTER TABLE Orders
    ADD FOREIGN KEY (P_Id)
    REFERENCES Persons(P_Id)

-- 如需命名 FOREIGN KEY 约束，并定义多个列的 FOREIGN KEY 约束，请使用下面的 SQL 语法：
-- MySQL / SQL Server / Oracle / MS Access：

ALTER TABLE Orders
    ADD CONSTRAINT fk_PerOrders
    FOREIGN KEY (P_Id)
    REFERENCES Persons(P_Id)
```

- 撤销 `FOREIGN KEY` 约束

如需撤销 `FOREIGN KEY` 约束，请使用下面的 SQL：

```sql
-- MySQL：
ALTER TABLE Orders
    DROP FOREIGN KEY fk_PerOrders

-- SQL Server / Oracle / MS Access：
ALTER TABLE Orders
    DROP CONSTRAINT fk_PerOrders
```

### CHECK

`CHECK` 约束用于限制列中的值的范围。如果对单个列定义 `CHECK` 约束，那么该列只允许特定的值。如果对一个表定义 `CHECK` 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。

- `CREATE TABLE` 时的 SQL `CHECK` 约束

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 `CHECK` 约束。`CHECK` 约束规定 "P_Id" 列必须只包含大于 0 的整数。

```sql
-- MySQL：
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    CHECK (P_Id>0)
)

-- SQL Server / Oracle / MS Access：
CREATE TABLE Persons
(
    P_Id int NOT NULL CHECK (P_Id>0),
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)
```

如需命名 `CHECK` 约束，并定义多个列的 `CHECK` 约束，请使用下面的 SQL 语法：

```sql
-- MySQL / SQL Server / Oracle / MS Access：
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
)
```

- `ALTER TABLE` 时的 SQL `CHECK` 约束

当表已被创建时，如需在 "P_Id" 列创建 `CHECK` 约束，请使用下面的 SQL：

```sql
-- MySQL / SQL Server / Oracle / MS Access:
ALTER TABLE Persons
    ADD CHECK (P_Id>0)
```

如需命名 CHECK 约束，并定义多个列的 CHECK 约束，请使用下面的 SQL 语法：

```sql
-- MySQL / SQL Server / Oracle / MS Access：
ALTER TABLE Persons
    ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
```

- 撤销 CHECK 约束

如需撤销 CHECK 约束，请使用下面的 SQL：

```sql
-- SQL Server / Oracle / MS Access：
ALTER TABLE Persons
    DROP CONSTRAINT chk_Person

-- MySQL：
ALTER TABLE Persons
    DROP CHECK chk_Person
```

### DEFAULT

`DEFAULT` 约束用于向列中插入默认值。如果没有规定其他的值，那么会将默认值添加到所有的新记录。

- `CREATE TABLE` 时的 SQL `DEFAULT` 约束

下面的 SQL 在 "Persons" 表创建时在 "City" 列上创建 `DEFAULT` 约束：

```sql
-- My SQL / SQL Server / Oracle / MS Access：
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
)
```

通过使用类似 `GETDATE()` 这样的函数，`DEFAULT 约束也可以用于插入系统值：

```sql
CREATE TABLE Orders
(
    O_Id int NOT NULL,
    OrderNo int NOT NULL,
    P_Id int,
    OrderDate date DEFAULT GETDATE()
)
```

- `ALTER TABLE` 时的 SQL `DEFAULT` 约束

当表已被创建时，如需在 "City" 列创建 `DEFAULT` 约束，请使用下面的 SQL：

```sql
-- MySQL：
ALTER TABLE Persons
    ALTER City SET DEFAULT 'SANDNES'

-- SQL Server / MS Access：
ALTER TABLE Persons
    ALTER COLUMN City SET DEFAULT 'SANDNES'

-- Oracle：
ALTER TABLE Persons
    MODIFY City DEFAULT 'SANDNES'
```

- 撤销 `DEFAULT` 约束

如需撤销 `DEFAULT` 约束，请使用下面的 SQL：

```sql
# MySQL：
ALTER TABLE Persons
    ALTER City DROP DEFAULT

# SQL Server / Oracle / MS Access：
ALTER TABLE Persons
    ALTER COLUMN City DROP DEFAULT
```

## CREATE INDEX

`CREATE INDEX` 语句用于在表中创建索引。在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

`CREATE INDEX` 语法

在表上创建一个简单的索引。允许使用重复的值：

```sql
CREATE INDEX index_name ON table_name (column_name)
```

`CREATE UNIQUE INDEX` 语法

在表上创建一个唯一的索引。不允许使用重复的值：唯一的索引意味着两个行不能拥有相同的索引值：

```sql
CREATE UNIQUE INDEX index_name ON table_name (column_name)
```

注释：用于创建索引的语法在不同的数据库中不一样。因此，检查您的数据库中创建索引的语法。

```sql
-- 在 "Persons" 表的 "LastName" 列上创建一个名为 "PIndex" 的索引：
CREATE INDEX PIndex
    ON Persons (LastName)

-- 如果您希望索引不止一个列，您可以在括号中列出这些列的名称，用逗号隔开：
CREATE INDEX PIndex
    ON Persons (LastName, FirstName)
```

## DROP INDEX

`DROP INDEX` 语句用于删除表中的索引。

用于 MS Access 的 `DROP INDEX` 语法：

```sql
DROP INDEX index_name ON table_name
```

用于 MS SQL Server 的 `DROP INDEX` 语法：

```sql
DROP INDEX table_name.index_name
```

用于 DB2/Oracle 的 `DROP INDEX` 语法：

```sql
DROP INDEX index_name
```

用于 MySQL 的 `DROP INDEX` 语法：

```sql
ALTER TABLE table_name DROP INDEX index_name
```

## AUTO INCREMENT

`AUTO INCREMENT` 会在新记录插入表中时生成一个唯一的数字。我们通常希望在每次插入新记录时，自动地创建主键字段的值。我们可以在表中创建一个 auto-increment 字段。

- 用于 MySQL 的语法

MySQL 使用 `AUTO_INCREMENT` 关键字来执行 auto-increment 任务。默认地，`AUTO_INCREMENT` 的开始值是 1，每条新记录递增 1。

```sql
-- 把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段：
CREATE TABLE Persons
(
    ID int NOT NULL AUTO_INCREMENT,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255),
    PRIMARY KEY (ID)
)

-- 在 "Persons" 表中插入一条新记录。"ID" 列会被赋予一个唯一的值。"FirstName" 列
-- 会被设置为 "Lars"，"LastName" 列会被设置为 "Monsen"。
INSERT INTO Persons (FirstName,LastName) VALUES ('Lars','Monsen')
```

要让 `AUTO_INCREMENT` 序列以其他的值起始，请使用下面的 SQL 语法：

```sql
ALTER TABLE Persons AUTO_INCREMENT=100
```

- 用于 SQL Server 的语法

MS SQL Server 使用 `IDENTITY` 关键字来执行 auto-increment 任务。

```sql
-- 把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段，并
-- 规定 "ID" 列以 10 起始且递增 5：
CREATE TABLE Persons
(
    ID int IDENTITY(10,5) PRIMARY KEY,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)

-- 在 "Persons" 表中插入一条新记录。"ID" 列会被赋予一个唯一的值。"FirstName" 列
-- 会被设置为 "Lars"，"LastName" 列会被设置为 "Monsen"。
INSERT INTO Persons (FirstName,LastName) VALUES ('Lars','Monsen')
```

- 用于 Access 的语法

MS Access 使用 `AUTOINCREMENT` 关键字来执行 auto-increment 任务。默认地，`AUTOINCREMENT` 的开始值是 1，每条新记录递增 1。

```sql
-- 把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段，
-- 并规定 "ID" 列以 10 起始且递增 5：
CREATE TABLE Persons
(
    ID Integer PRIMARY KEY AUTOINCREMENT(10,5),
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)

-- 在 "Persons" 表中插入一条新记录。"ID" 列会被赋予一个唯一的值。"FirstName" 列
-- 会被设置为 "Lars"，"LastName" 列会被设置为 "Monsen"。
INSERT INTO Persons (FirstName,LastName) VALUES ('Lars','Monsen')
```

- Oracle 语法

在 Oracle 中，您必须通过 `sequence` 对象（该对象生成数字序列）创建 auto-increment 字段。

```sql
-- 创建一个名为 seq_person 的 sequence 对象，它以 1 起始且以 1 递增。
-- 该对象缓存 10 个值以提高性能。cache 选项规定了为了提高访问速度要存储多少个序列值。

CREATE SEQUENCE seq_person
    MINVALUE 1
    START WITH 1
    INCREMENT BY 1
    CACHE 10
```

要在 "Persons" 表中插入新记录，我们必须使用 `nextval` 函数（该函数从 seq_person 序列中取回下一个值）：

```sql
-- 在 "Persons" 表中插入一条新记录。"ID" 列会被赋值为来自 seq_person 序列的下一个数字。
-- "FirstName"列 会被设置为 "Lars"，"LastName" 列会被设置为 "Monsen"。
INSERT INTO Persons (ID,FirstName,LastName) VALUES (seq_person.nextval,'Lars','Monsen')
```

## SQL Views

视图是可视化的表。

### SQL CREATE VIEW
在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。
视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。
您可以向视图添加 SQL 函数、`WHERE` 以及 `JOIN` 语句，也可以呈现数据，就像这些数据来自于某个单一的表一样。

```sql
CREATE VIEW view_name AS
    SELECT column_name(s)
    FROM table_name
    WHERE condition
```

示例：
```sql
-- 一个视图会选取 "Products" 表中所有单位价格高于平均单位价格的产品：
CREATE VIEW [Products Above Average Price] AS
    SELECT ProductName,UnitPrice
    FROM Products
    WHERE UnitPrice>(SELECT AVG(UnitPrice) FROM Products)

-- 我们可以像这样查询上面这个视图：
SELECT * FROM [Products Above Average Price]

-- 另一个视图会计算在 1997 年每个种类的销售总数。请注意，这个视图会从另一个名为
-- "Product Sales for 1997" 的视图那里选取数据：
CREATE VIEW [Category Sales For 1997] AS
    SELECT DISTINCT CategoryName,Sum(ProductSales) AS CategorySales
    FROM [Product Sales for 1997]
    GROUP BY CategoryName

-- 我们也可以向查询添加条件。现在，我们仅仅需要查看 "Beverages" 类的销售总数：
SELECT * FROM [Category Sales For 1997]
    WHERE CategoryName='Beverages'
```

### SQL DROP VIEW

您可以通过 `DROP VIEW` 命令来删除视图。
```sql
DROP VIEW view_name
```

## Dates

只要您的数据包含的只是日期部分，运行查询就不会出问题。但是，如果涉及时间部分，情况就有点复杂了。
在讨论日期查询的复杂性之前，我们先来看看最重要的内建日期处理函数。

### MySQL Date Functions

下面的表格列出了 MySQL 中最重要的内建日期函数：


|函数               | 描述                              |
|:-:|:-:|
|NOW()              |返回当前的日期和时间               |
|CURDATE()          |返回当前的日期                     |
|CURTIME()          |返回当前的时间                     |
|DATE()             |提取日期或日期/时间表达式的日期部分|
|EXTRACT()          |返回日期/时间的单独部分            |
|DATE_ADD()         |向日期添加指定的时间间隔           |
|DATE_SUB()         |从日期减去指定的时间间隔           |
|DATEDIFF()         |返回两个日期之间的天数             |
|DATE_FORMAT()      |用不同的格式显示日期/时间          |


### SQL Server Date Functions

下面的表格列出了 SQL Server 中最重要的内建日期函数：

|函数               | 描述                           |
|:-:|:-:|
|GETDATE()          |返回当前的日期和时间            |
|DATEPART()         |返回日期/时间的单独部分         |
|DATEADD()          |在日期中添加或减去指定的时间间隔|
|DATEDIFF()         |返回两个日期之间的时间          |
|CONVERT()          |用不同的格式显示日期/时间       |

### SQL Date Data Types

MySQL 使用下列数据类型在数据库中存储日期或日期/时间值：

| 时间              | 格式              |
|:-:|:-:|
|DATE               |YYYY-MM-DD         |
|DATETIME           |YYYY-MM-DD HH:MM:SS|
|TIMESTAMP          |YYYY-MM-DD HH:MM:SS|
|YEAR               |YYYY 或 YY         |


### SQL Server Data Types

SQL Server 使用下列数据类型在数据库中存储日期或日期/时间值：

| 时间              | 格式              |
|:-:|:-:|
|DATE               |YYYY-MM-DD         |
|DATETIME           |YYYY-MM-DD HH:MM:SS|
|SMALLDATETIME      |YYYY-MM-DD HH:MM:SS|
|TIMESTAMP          |唯一的数字         |

# SQL Functions

## MIN() and MAX()

`MIN()` 函数返回指定列的最小值。`MAX()` 函数返回指定列的最大值。

```sql
SELECT MIN(column_name) FROM table_name WHERE condition;

SELECT MAX(column_name) FROM table_name WHERE condition;
```


## COUNT(), AVG() and SUM() 

`COUNT()` 函数返回匹配指定条件的行数。`AVG()` 函数返回数值列的平均值。`SUM()` 函数返回数值列的和。

```sql
SELECT COUNT(column_name) FROM table_name WHERE condition;

SELECT AVG(column_name) FROM table_name WHERE condition;

SELECT SUM(column_name) FROM table_name WHERE condition;
```

## NULL Functions

`NULL` 函数用于规定如何处理 `NULL` 值。

SQL Server / MS Access：使用 `ISNULL()` 函数
Oracle：使用 `NVL()` 函数
MySQL：使用 `IFNULL()` 和 `COALESCE()` 函数

示例：
```sql
-- MySQL
SELECT ProductName, UnitPrice * (UnitsInStock + IFNULL(UnitsOnOrder, 0)) FROM Products

SELECT ProductName, UnitPrice * (UnitsInStock + COALESCE(UnitsOnOrder, 0)) FROM Products

-- SQL Server / MS Access
SELECT ProductName, UnitPrice * (UnitsInStock + ISNULL(UnitsOnOrder, 0)) FROM Products

-- Oracle
SELECT ProductName, UnitPrice * (UnitsInStock + NVL(UnitsOnOrder, 0)) FROM Products
```

## UCASE() and LCASE()

`UCASE()` 函数把字段的值转换为大写。`LCASE()` 函数把字段的值转换为小写。

```sql
SELECT UCASE(column_name) FROM table_name;

SELECT LCASE(column_name) FROM table_name;
```

## MID()
`MID()` 函数用于从文本字段中提取字符。

```sql
SELECT MID(column_name,start[,length]) FROM table_name;
```

示例：

```sql
SELECT MID(name,1,4) AS ShortTitle FROM Websites;
```

## LEN()

`LEN()` 函数返回文本字段中值的长度。MySQL 中函数为 `LENGTH()`。

```sq
SELECT LEN(column_name) FROM table_name;
```

## ROUND()

`ROUND()` 函数用于把数值字段舍入为指定的小数位数。

```sql
SELECT ROUND(column_name,decimals) FROM table_name;
```

示例：

```sql
SELECT ROUND(1.58);
-> 2

SELECT ROUND(1.298, 1);
-> 1.3
```

## NOW()

`NOW()` 函数返回当前系统的日期和时间。

```sql
SELECT NOW() FROM table_name;
```

示例：

```sql
SELECT name, url, Now() AS date FROM Websites;
```

## FORMAT()

`FORMAT()` 函数用于对字段的显示进行格式化。

```sql
SELECT FORMAT(column_name,format) FROM table_name;
```

示例：

```sql
SELECT name, url, DATE_FORMAT(Now(),'%Y-%m-%d') AS date FROM Websites;
```

# SQL Functions References 

## MySQL Functions

## SQL Server Functions

## MS Access Functions

## Oracle Functions

# SQL Injection
