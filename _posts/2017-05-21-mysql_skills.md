---
layout: post
title:  "mysql_skills"
date:   2017-05-21 21:08:08 +0800
tag: mysql
---
# SQL开发技巧


### SQL语言
```
graph LR
SQL-->关系型数据库
SQL-->非关系型数据库
```

#### 常见的SQL语句类型

```
graph LR
SQL-->DDL:数据定义语言
SQL-->TPL:事务处理语言
SQL-->DCL:数据控制语言
SQL-->DML:数据操作语言
DML:数据操作语言-->SELECT
DML:数据操作语言-->INSERT
DML:数据操作语言-->UPDATE
DML:数据操作语言-->DELETE
```

#### 正确使用sql的重要性
很多开发人员不了解sql语言，大多数人只是通过一些底层架构或者是框架，生成对象，操作对象来达到操作数据库数据，但相对操作效率也是比较低下的

而正确使用SQL
*   增加数据库处理效率，减少应用响应时间
*   减少数据库服务器负载，增加服务器稳定性
*   减少服务器间通讯的网络流量


### 如何正确使用join从句

SQL标准中join的类型
> * 内连接'INNER'
> * 全外连接'FULL_OUTER'
> * 左外连接'LEFT_OUTER'
> * 右外连接'RIGHT_OUTER'
> * 交叉连接'CROSS'

#### 现有表
```
mysql> select * from join_test1;
+----+-----------+
| id | user_name |
+----+-----------+
|  1 | 唐僧      |
|  2 | 猪八戒    |
|  3 | 孙悟空    |
|  4 | 沙僧      |
+----+-----------+
4 rows in set (0.00 sec)

mysql> select * from join_test2;
+----+-----------+
| id | user_name |
+----+-----------+
|  1 | 孙悟空    |
|  2 | 猪八戒    |
|  3 | 蛟魔王    |
|  4 | 鹏魔王    |
|  5 | 狮驼王    |
+----+-----------+
5 rows in set (0.00 sec)
```

#### join操作的类型-Inner join
内连接Inner join基于连接谓词将两张表（如 A和B）的列组合在一起，产生新的结果表
![]('../images/posts/mysql/Inner_join.png')
