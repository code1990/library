##（一）MySQL 术语

* 数据库: 数据库是一些关联表的集合。
* 数据表: 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
* 列: 一列(数据元素) 包含了相同的数据, 例如邮政编码的数据。
* 行：一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
* 冗余：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
* 主键：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
* 外键：外键用于关联两个表。
* 复合键：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
* 索引：使用索引可快速访问数据库表中的特定信息。类似于书籍的目录。
* 参照完整性: 参照的完整性要求关系中不允许引用不存在的实体。，目的是保证数据的一致性。

**数据库三范式**

* 第一范式:确保每列的原子性.
* 第二范式:在第一范式的基础上更进一层,目标是确保表中的每列都和主键相关.
* 第三范式:在第二范式的基础上更进一层,目标是确保每列都和主键列直接相关,而不是间接相关.

**数据库ACID**

* A (Atomicity) 原子性 事务里的所有操作要么全部做完，要么都不做
* C (Consistency) 一致性 数据库要一直处于一致的状态
* I (Isolation) 独立性 所谓的独立性是指并发的事务之间不会互相影响
* D (Durability) 持久性 持久性是指一旦事务提交后，它所做的修改将会永久的保存在数据库上
##（二）数据库
* CREATE DATABASE 数据库名;
* drop database <数据库名>;
* use  <数据库名>
##（三）数据类型
* 数值数据类型(INTEGER、SMALLINT、DECIMAL和NUMERIC)，
* 时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。
* 字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。 

##（四）数据表

	CREATE TABLE IF NOT EXISTS `runoob_tbl`(
	   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
	   `runoob_title` VARCHAR(100) NOT NULL,
	   `runoob_author` VARCHAR(40) NOT NULL,
	   `submission_date` DATE,
	   PRIMARY KEY ( `runoob_id` )
	)ENGINE=InnoDB DEFAULT CHARSET=utf8;

	DROP TABLE table_name ;

	INSERT INTO runoob_tbl  
	(runoob_title, runoob_author, submission_date)
	VALUES
	("学习 PHP", "菜鸟教程", NOW());

SELECT column_name,column_name
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]

    查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。
    SELECT 命令可以读取一条或者多条记录。
    你可以使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据
    你可以使用 WHERE 语句来包含任何条件。
    你可以使用 LIMIT 属性来设定返回的记录数。
    你可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。


##（五）UPDATE 查询 
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]
##（六） DELETE 语句 
删除 MySQL 数据表中的记录。
DELETE FROM table_name [WHERE Clause]
##（七）LIKE 子句
LIKE 通常与 % 一同使用，类似于一个元字符的搜索。
##（八）UNION 操作符
DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。

ALL: 可选，返回所有结果集，包含重复数据。
##（九）MySQL 排序
你可以使用 ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。 默认情况下，它是按升序排列。
##（十）MySQL GROUP BY 语句 

GROUP BY 语句根据一个或多个列对结果集进行分组。

在分组的列上我们可以使用 COUNT, SUM, AVG,等函数。 

##（十一）Mysql 连接的使用

JOIN 按照功能大致分为如下三类：

    INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。
    LEFT JOIN（左连接）：获取左表所有记录，即使右表没有对应匹配的记录。
    RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

##（十二）MySQL NULL 值处理

为了处理这种情况，MySQL提供了三大运算符:

    IS NULL: 当列的值是 NULL,此运算符返回 true。
    IS NOT NULL: 当列的值不为 NULL, 运算符返回 true。
##（十三）MySQL 正则表达式

##（十四）MySQL 事务
事务是必须满足4个条件（ACID）：：原子性（Atomicity，或称不可分割性）、一致性（Consistency）、隔离性（Isolation，又称独立性）、持久性（Durability）。

    原子性：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成

    一致性：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。

    隔离性：数据库允许多个并发事务同时对其数据进行读写和修改的能力

    持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

MYSQL 事务处理主要有两种方法：

1、用 BEGIN, ROLLBACK, COMMIT来实现

    BEGIN 开始一个事务
    ROLLBACK 事务回滚
    COMMIT 事务确认

2、直接用 SET 来改变 MySQL 的自动提交模式:

    SET AUTOCOMMIT=0 禁止自动提交
    SET AUTOCOMMIT=1 开启自动提交
##（十五）MySQL ALTER命令


当我们需要修改数据表名或者修改数据表字段时，就需要使用到MySQL ALTER命令。

* ALTER TABLE testalter_tbl DROP i;
* ALTER TABLE testalter_tbl ADD i INT FIRST;
* ALTER TABLE testalter_tbl DROP i;
* ALTER TABLE testalter_tbl ADD i INT AFTER c;
* ALTER TABLE testalter_tbl MODIFY c CHAR(10);
* ALTER TABLE testalter_tbl CHANGE i j BIGINT;
* ALTER TABLE testalter_tbl CHANGE j j INT;
##（十六）MySQL 索引


MySQL索引的建立对于MySQL的高效运行是很重要的，索引可以大大提高MySQL的检索速度。

打个比方，如果合理的设计且使用索引的MySQL是一辆兰博基尼的话，那么没有设计和使用索引的MySQL就是一个人力三轮车。

索引分单列索引和组合索引。单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。组合索引，即一个索引包含多个列。
创建索引

这是最基本的索引，它没有任何限制。它有以下几种创建方式：

CREATE INDEX indexName ON mytable(username(length)); 

如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length。
修改表结构(添加索引)

ALTER table tableName ADD INDEX indexName(columnName)

创建表的时候直接指定

CREATE TABLE mytable(  
 
ID INT NOT NULL,   
 
username VARCHAR(16) NOT NULL,  
 
INDEX [indexName] (username(length))  
 
);  

删除索引的语法

DROP INDEX [indexName] ON mytable; 

唯一索引

它与前面的普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。它有以下几种创建方式：
创建索引

CREATE UNIQUE INDEX indexName ON mytable(username(length)) 

修改表结构

ALTER table mytable ADD UNIQUE [indexName] (username(length))
##（十七）MySQL 临时表


MySQL 临时表在我们需要保存一些临时数据时是非常有用的。临时表只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。 
mysql> CREATE TEMPORARY TABLE SalesSummary ()

##（十八）MySQL 复制表


如果我们需要完全的复制MySQL的数据表，包括表的结构，索引，默认值等。 如果仅仅使用CREATE TABLE ... SELECT 命令，是无法实现的。

本章节将为大家介绍如何完整的复制MySQL数据表，步骤如下：

    使用 SHOW CREATE TABLE 命令获取创建数据表(CREATE TABLE) 语句，该语句包含了原数据表的结构，索引等。
    复制以下命令显示的SQL语句，修改数据表名，并执行SQL语句，通过以上命令 将完全的复制数据表结构。
    如果你想复制表的内容，你就可以使用 INSERT INTO ... SELECT 语句来实现。

实例
##（十九）MySQL 元数据


你可能想知道MySQL以下三种信息：

    查询结果信息： SELECT, UPDATE 或 DELETE语句影响的记录数。
    数据库和数据表的信息： 包含了数据库及数据表的结构信息。
    MySQL服务器信息： 包含了数据库服务器的当前状态，版本号等。

##（二十）MySQL 序列使用


MySQL序列是一组整数：1, 2, 3, ...，由于一张数据表只能有一个字段自增主键， 如果你想实现其他字段也实现自动增加，就可以使用MySQL序列来实现。

本章我们将介绍如何使用MySQL的序列。

##MySQL 处理重复数据


有些 MySQL 数据表中可能存在重复的记录，有些情况我们允许重复数据的存在，但有时候我们也需要删除这些重复的数据。

本章节我们将为大家介绍如何防止数据表出现重复数据及如何删除数据表中的重复数据。
防止表中出现重复数据
你可以在MySQL数据表中设置指定的字段为 PRIMARY KEY（主键） 或者 UNIQUE（唯一） 索引来保证数据的唯一性。

让我们尝试一个实例：下表中无索引及主键，所以该表允许出现多条重复记录。
##MySQL 及 SQL 注入


如果您通过网页获取用户输入的数据并将其插入一个MySQL数据库，那么就有可能发生SQL注入安全的问题。

本章节将为大家介绍如何防止SQL注入，并通过脚本来过滤SQL中注入的字符。
 防止SQL注入

在脚本语言，如Perl和PHP你可以对用户输入的数据进行转义从而来防止SQL注入。

PHP的MySQL扩展提供了mysqli_real_escape_string()函数来转义特殊的输入字符。
##MySQL 导出数据

MySQL中你可以使用SELECT...INTO OUTFILE语句来简单的导出数据到文本文件上。 
mysql> SELECT * FROM runoob_tbl 
    -> INTO OUTFILE '/tmp/runoob.txt';
##、source 命令导入

source 命令导入数据库需要先登录到数库终端：

mysql> create database abc;      # 创建数据库
mysql> use abc;                  # 使用已创建的数据库 
mysql> set names utf8;           # 设置编码
mysql> source /home/abc/abc.sql  # 导入备份数据库

3、使用 LOAD DATA 导入数据

MySQL 中提供了LOAD DATA INFILE语句来插入数据。 以下实例中将从当前目录中读取文件 dump.txt ，将该文件中的数据插入到当前数据库的 mytbl 表中。

mysql> LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl;

##

##

##

##

