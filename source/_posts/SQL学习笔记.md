---
title: SQL学习笔记
date: 2025-09-11 20:06:01
cover: /images/image7.jpg
tags:
    - SQL
    - 技术
categories:
    - 笔记
feature: true
---


# SQL语言自己的一些学习笔记

### 一、SQL语言概述
SQL 指结构化查询语言，全称是 Structured Query Language

### 二、SQL语言的分类
**1.DDL（Data Definition Languages）语句：** 即数据库定义语句  
常用的语句关键字有：

【create table 创建表】、【alter table 修改表】、【drop table 删除表】、【truncate table 删除表中所有行】、【create index 创建索引】、【drop index 删除索引】、【comment 注释】、【rename table 修改表名】

**02、DML（Data Manipulation Language）语句：** 即用于管理数据库中数据的操作，用来查询（Select）、添加（Insert）、更新（Update）、删除（Delete）等

**03、DCL（Data Control Language）语句：** 即数据控制语句，用于授权/撤销数据库及其字段的权限。
常用的语句关键字有：grant授权、revoke取消授权

**04、DQL:（Data QueryLanguage）语句：** 

数据查询语言:

selet 获取、where、group by、having、ofder by等过滤条件

事务控制语言:

SAVEPOINT 设置保存点、ROLLBACK 回滚、SET TRANSACTION 设置事物属性

### 三、SQL语句详解

**1.操作数据库**
```SQL
CREATE DATABASE TestDB;
DROP DATABASE TestDB;
```

**2.操作表**
```SQL
CREATE TABLE EmployeeTable (
    ID int,
    Name varchar(255),
    Age int
);

DROP TABLE EmployeeTable;

ALTER TABLE EmployeeTable ADD Email varchar(255);
ALTER TABLE EmployeeTable MODIFY COLUMN Age smallint;  // MODIFY修改数据类型和精度
ALTER TABLE EmployeeTable DROP COLUMN Age;  // COLUMN是可以省略的

RENAME TABLE EmployeeTable TO StaffTable;  // 重命名
```

**3.操作数据**
```SQL
INSERT INTO employees (id, name, department_id) VALUES (1, 'Li Ming', 101);    

UPDATE employees SET department_id = 201 WHERE name = 'Li Ming'; 

DELETE FROM employees WHERE id = 1;   
```

**4.基础查询**
```SQL
SELECT * FROM Employee 
WHERE Salary > 5000 
AND Age <= 30;   // OR也可以

SELECT * 
FROM Employee 
WHERE FirstName LIKE 'John%'; 
（查询Employee表中FirstName以John开头的所有记录）

SELECT * 
FROM Employee 
WHERE FirstName LIKE '%John%'; 
（查询Employee表中FirstName包含John的所有记录）

SELECT FirstName, LastName 
FROM Employee 
WHERE LastName LIKE '%son_'; 
（查询Employee表中LastName以son+一个任意字符结束的所有记录）

SELECT * 
FROM Employee   // 多列排序
ORDER BY Salary, Age DESC;  // 正常是ASC 可以省略不写
（首先根据'工资'列的升序对Employee表中的行进行排序，然后在工资相同的情况下，根据'Age'列的降序进行排序。）
```

**5.聚合函数**

```SQL
SELECT COUNT(*) 
FROM Employee;
（返回Employee表的总行数。）

SELECT SUM(Salary) 
FROM Employee;
（返回Employee表中所有员工的薪水总和。）

SELECT MAX(Age) FROM Employee; （返回Employee表中员工的最大年龄。）

SELECT MIN(Age) FROM Employee;（返回Employee表中员工的最小年龄。）

SELECT AVG(Salary) FROM Employee;（返回Employee表中员工的平均薪水。）

```

**6.分组查询**

```SQL
// 单列分组
SELECT Department, COUNT(*) AS EmployeeCount
FROM Employee
GROUP BY Department;

// 多列分组
SELECT Department, JobTitle, COUNT(*) AS EmployeeCount
FROM Employee
GROUP BY Department, JobTitle;

// 结合HAVING
SELECT Department, COUNT(*) AS EmployeeCount
FROM Employee
GROUP BY Department
HAVING COUNT(*) > 1;
```

**7.LIMIT用法**
```SQL
SELECT * FROM Employee
LIMIT 2, 2;  -- 跳过 2 行，返回 2 行
```

**8.各种连接**
```SQL

// 內连接
SELECT column_name(s)
FROM Table1
INNER JOIN Table2 ON Table1.column_name = Table2.column_name;

// 左外连接
SELECT column_name(s)
FROM Table1
LEFT JOIN Table2 ON Table1.column_name = Table2.column_name;

// 右外连接
SELECT column_name(s)
FROM Table1
RIGHT JOIN Table2 ON Table1.column_name = Table2.column_name;

// 全外连接
SELECT column_name(s)
FROM Table1
FULL JOIN Table2 ON Table1.column_name = Table2.column_name;

// 交叉连接 返回笛卡尔积 （例如所有员工和所有部门的组合）
SELECT column_name(s)
FROM Table1
CROSS JOIN Table2;

//自连接
SELECT column_name(s)
FROM Table1 AS T1
INNER JOIN Table1 AS T2 ON T1.column_name = T2.column_name;

```

