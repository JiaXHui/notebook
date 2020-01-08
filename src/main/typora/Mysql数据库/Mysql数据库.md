# 数据库Mysql



## 数据库基本介绍

```text
基本数据存储到数据库中。（导航、图片地址、商品信息...）
图片信息存储到硬盘上。（图片服务器）
极少的特别重要的信息会存储到浏览器cookie中。
高级应用：会将常用的数据存储到电脑缓存中。
数据库全称database简称DB，数据库是存储数据的仓库。其本质就是本地一些特有的格式的存储数据的文件。

数据库管理系统。指用来维护、操作这本数据文件的系统，我们叫数据库管理系统。
其中有名气且常见的一些数据库管理系统MySQL、Oracle、SQLserver、DB2...

MySQL（人气王）	MySQL/Oracle		开源免费 适用于中小型项目，有收费版。
Oracle（土豪专享）	oracle				收费版、适用于所有项目。
9i(internet) 互联网兴起，数据量变大，适用的一个版本。
10G/11G(grid) 数据越来越大，推出一个网格化管理。
12C(cloud) 适用于云系列(云平台、云计算、云存储)

SQLserver			微软				收费/免费版  全能。
DB2					IBM					收费/免费版  全能。

作用：保存信息。
持久化：
大白话：将信息保存进数据库，以防止数据丢失，我们叫持久化。
专业：数据从瞬时状态转变为持久状态的过程叫做持久化。
瞬时状态：内存中的数据、new的数据，随时可能丢失。
持久状态：硬盘中（数据库中）。
```



## [MySQL](www.mysql.com)

```test
关系型数据库管理系统
上述讲的所有的数据都是关系型数据库，其特点是在数据库中表与表、列与列都存在关系，所以我们叫关系型数据库。

非关系型数据库关系系统
类似于redis 这种数据库中的数据是以key-value的形式进行存储，数据与数据之间不存在任何关联，所以叫非关系型数据库。
```



## 卸载MySQL文件

```
MySQL为了保存数据不丢失，所以直接控制面板卸载的时候数据和配置是不会清理的。这会导致下次安装总是不成功，所以卸载的时候一定记得清理配置。

第一步：找到MySQL的安装目录 - 查找 my.ini的配置文件 备份
第二步：控制面板直接卸载
第三步：打开文件找到两个目录  basedir（软件目录） | datadir（数据目录）分别删除。
```

$\textcolor{red}{1:  C:\ProgramData\MySQL}$

$\textcolor{red}{2:  C:\Program Files\MySQL}$

```text
删除这两个目录
```



## 配置环境变量

```text
C:\Program Files\MySQL\MySQL Server 8.0\bin
```



## MySQL基本操作

```test
通过本机自带的dos系统启动mysql
mysql -hlocalhost -uroot -p密码
如果是调用本机的mysql数据库可以省略localhost连接地址
mysql  -u root -p密码

mysql: 声明当前命令作用于mysql数据库
-h:    输入访问数据库的地址,远程就是IP地址如果是本机localhost就可以省略
-u:    用户名称
-p:    用户密码

退出命令 
exit 
quit
\q
```



## SQL介绍和分类(六种分类)

```text
SQL Structured Query Language（结构化查询语言） 简称SQL
作用：用来在数据库管理系统上操作库、表、数据。

结构化查询语言(Structured Query Language)简称SQL，是一种特殊目的的编程语言，是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统。

SQL是一门通用语言，所有的关系型数据库都能用，极个别不同的数据库会存在极小的差异
SQL一共分为6类。 其中我们学习的有三类。
```

![图片1](E:\typora图片\Mysql数据库\图片1.png)

```text
CRUD（create read update delete） 增删改查的意思 

需要学习的三个
DDL(数据库定义语言)
负责创建、修改、删除、查询库和表（和数据无关）
DML（数据库操作语言）
负责数据的增加、修改、删除操作。
DQL（数据库查询语言）
负责数据的查询操作

需要了解的三个（DBA（数据库管理员）必学）
TCL（事务控制语言）
数据的提交、回滚操作。
DCL（权限控制语言）
负责控制数据库权限
CCL（指针控制语言）
控制指针
```



### 数据库及建表概念

```text
数据库（database）
一个数据库管理系统中可以创建多个数据库，小项目一般一个项目使用一个数据库就可以。大型的项目可能有多个数据库。
数据表（table）
	数据库下存放的是数据表，数据表的结构和table是一样的。分表头、行、列。表中存放内容。一般一行代表一条数据。
```



### 数据库类型

```mysql
MySQL常用的数据类型

字符串类型:
  char   固定长度的字符串类型
  例如: char 的长度为 char(100),存储2个字符,但是会占用100个字符的空间
  varchar 字符串,长度可变的字符串类型
  例如: varchar 的长度为 varchar(100),存储2个字符,但是会占用100个字符的空间
  
数字类型:
  浮点型(两种):
     float 正常
     double 正常(不可改变长度)
  整形(四种):  
     tinyint 微小的 int 占用一个字符空间
     mediumint 中型的 int 占用两个字符空间
     int  普通类型占用四个字符空间
     bigint 大型的 int 占用八个字符空间
  布尔:
     数据库是没有 boolean 类型的.即使有一些能够生成 boolean 类型,自定会帮你转为 tinyint 使用 0,1 来表示
     true 与 false
  日期:
      date  2019-10-30
      time  19:40:29
      datetime  	2019-10-30 15:03:58
      timestamp 2019-10-30 15:03:58
      
      关于datetime和timestamp 区别【面试题】
      datetime 的默认值是null。最大的日期支持到9999:12:31 23:59:59.999
      timestamp的默认值是当前系统时间。最大支持的时间是
      2037年:12:31 23:59:59.999【根本的原因是32位操作系统最大计算的时间就是2037年】 现在已经64位操作系统，只要是64位没有这个限制。
      计算时间元年  1970年1月1日（计算计算机诞生）
      时间类型、double、float这是三个不需要声明长度。括号也不要。
```



### DDL库操作

```mysql
库(database)
创建(create) 修改(alter) 删除(drop) 查询(show)

语法结构:
操作 database ...

#查看所有数据库
show databases;'

#创建一个数据库
create database 数据库名;

#删除数据库
drop database 数据库名;

#创建一个带指定字符集数据库(编码格式)
create database 数据库名 character set 编码格式;

#查询单个数据库的信息
show create database 数据库名;

#改变已经创建好的数据库的编码格式
alter database 数据库名 character set 编码格式;
```



### DDL表操作

```mysql
表(table)
创建(create)  修改(alter) 删除(drop) 查询(show|desc)

#创建一个表
create table 表名(
  列名 数据类型(长度) 权限,
  列名 数据类型(长度) 权限,
  ....
  );

#查看所有的表
show tables;

#查看某一个表的信息  description[详情、描述]
desc 表名;

#删除一个表
drop table 表名;

#修改 - 一个表名
alter table 表名 rename 新名字;

#修改 - 创建一个表后在后面追加一列
alter table 表名 add 列名 数据类型(长度) 权限;

#修改 - 删除一个表的列名
alter table 表名 drop 列名;

#修改 - 创建一个表后修改一个列名
alter table 表名 change 旧列名 新列名 类型(长度) 权限;
```



### DML操作(表增、删、改)

```mysql
负责数据的增加、修改、删除.

增加 insert into 表名
修改 update 表名 set
删除 delete 表名 from

#增加一条数据
语法：insert into 表名(列名1,列名2...) value (值1,值2...);

#修改一条数据
语法: update 表名 set 列名=值,列名=值... where 条件;

#删除数据
语法: delete from 表名 where 条件;

#群不删除的两种方法
delete from 表名;
truncate 表名;
两种删除的异同:
相同点:都是清空数据
不同点:
    delete from这种事一条条的删除数据,效率极慢
    truncate 表名这种事现将表删除,留下表的数据结构,在生成一个新表.极快.
```



### DQL操作(表查询)

```mysql
查询
语法: select * from 表名
```



## MySQL约束

```mysql
主键约束
外键约束
唯一约束
非空约束
默认约束
```

 

### 主键约束 primary key

```mysql
主键(primary key) 简称 PK   自动增长 auto_increment 
是一条数据的唯一表示.一般主键会单独设置一个列,命名为id
特性:一般当当前列设置为主键,当前列的值就不能重复且不能为空 (确保唯一性)

一般手动添加的数据容易重复所以编办将主键类型设置为 int 类型
设置为自动增长  auto_increment 但数据类型必须为 int 类型
```



### 唯一约束  unique

```mysql
唯一约束(unique) 一般设置在一些特殊字段上面,为了确保数据的唯一性,例如注册邮箱,身份证号码,手机号...
没有主键约束强,但区别是值可以为null
```



### 非空约束 not null

```mysql
非空约束(not null)
特点:被定义的列字段值不可以为空,必须有值
总结:可以使空字符串但是不能为null
```



### 默认约束 default

```mysql
默认约束(default='')
特点:默认约束定义的列字段值为null时可以传入一个默认值
```



## 表关系

 ```mysql
MySQL是关系型数据库(表与表之间,列与列之间) 都有关系.
表与表之间的关系

三种关系分别是:  一对多,多对一 / 多对多 / 一对一
 ```



### 一对多/多对一

```MySQL
实例:班级表对学生表
一个班级对多个学生,多个学生对一个班级

从班级角度来讲:一个班级对应多个学生
从学生角度来讲:多个学生对应一个班级

#外键:和主键作用不同 外键的作用是使表与表之间创建关系.这种对应方式我们称之为外键关系,不同的对应关系,外键的添加方法也是不同的.关键字constraint(别名) foreign key 列名 reference 表名(列名)

#在一对多或多对一中,外键需要添加在多的一方,单独创建一个字段,并且要将关联定义到对应表的唯一字段中
```



### 多对多

```mysql
many to many 多对多
例如:用户对应股票  学生对应课程

#定义:两个表的一条数据分别对应另外一个表的多条数据

需要创建一个表专门保存两个表的数据对代表的字段id
```



### 一对一

```mysql
one to one 

one to one 一对一指的的一张表的一条数据只能对应另一个表的一条数据，一般一对一的时候的我们会分一个主表，一个从表。   主表中的数据不受影响（一对多一样），从表中的数据要么不存在，一旦存在必须强制和主表中的一条数据对应，并且不能重复。

示例： 人 和 身份证号  |  一夫一妻  |  电脑 - SN|MAC  
Wife    Husband
--
一对一效果：（从表主外键重合）
主表中主键自增。从表中可以没有数据
主表中主键在从表中做外键，并且外键指定到从表的主键上。 主表从表ID一致。
#注意：从表中ID不用自增。
```



## 连接查询

```mysql
上述一对多表关系查询的时候，使用的是两条SQL。正常有关系的查询一条SQL足够，可以使用连接查询

连接查询一共有两大类4小类
内连接
隐式内连接
显式内连接
外连接
左外连接
右外连接

在查询的时候可以直接指定多个表

如果在执行多个表查询的时候，如果不指定条件。那么会产生笛卡尔积结果集。
笛卡尔积：给x,y两个集合，如果将xy对应关系，那么在结果中会将两个集合的数据分别一一对应。
如何解决？  通过条件进行筛选。
```



### 隐式内连接

```mysql
#隐式内连接使用的关键字是 where
例如: select * from 表名 where 条件
```



### 显式内连接

```mysql
#显式内连接使用的关键字是 inner join .. on
例如: select s* from 表1 s inner join 表二 t inner join 表三 p
on p.sid=s.sid and p.tid=t.tid; 
```



### 左外连接

```mysql
left ... join on
特点:再多表查询的时候,左外链接的查询结果以左边的表为主,右表如果没有数据则使用 null 代替.(以左为尊)
```



### 右表连接

```mysql
right ... join on
特点:再多表查询的时候,右外链接的查询结果以右边的表为主,左表如果没有数据则使用 null 代替.(以右为尊)
```



## 分组、排序、聚合函数以及SQL函数



### 条件查询



```mysql
#支持的运算符
```

| 运算符        | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| =             | 等于                                                         |
| <> 或者!=     | 不等于                                                       |
| <             | 小于                                                         |
| <=            | 小于等于                                                     |
| >             | 大于                                                         |
| >=            | 大于等于                                                     |
| between...and | 两个值之间                                                   |
| is null       | 为null（is not null 不为null）                               |
| and           | 并且                                                         |
| or            | 或者                                                         |
| in            | 包含(not in 不包含范围中)                                    |
| not           | not否的意思,主要和is 或者in一起使用                          |
| like          | like称为模糊查询,支持%或者下划线 _%匹配任意字符_一个下划线( _ )只匹配一个字符 |



#### 1、等号查询（=）

```mysql
查询薪水为5000的员工
select * from emp where sal = 5000;
查询职位为 MANAGER 的员工的信息
select * from emp where job = 'mAnAger'
```



#### 2、不等号查询（<> 或 !=）

```mysql
查询薪水不等于5000的员工的信息
select * from emp where sal != 5000;
查询职位不是MANAGER 的员工的信息
select * from emp where job <> 'mAnAger';
```



#### 3、区间的条件查询(<,<=,>,>=,between...and..)

```mysql
薪水大于1600的员工信息
select * from emp where sal > 1600;
#薪水在1600和3000之间的员工信息
```



#### 4、或者or

```mysql
#注意：只要不是实在没有办法，就不要用。 SQL注入。可以用in代替
查询部门编号为20或者30的部门信息
select * from dept where deptno = 20 or deptno = 30 
```



#### 5、包含,不包含(in,not in)

```mysql
查询部门编号20或30的信息
select * from dept where depton in(20,30);
查询部门编号不为20或30的信息
select * from dept where depton not in(20,30);
```



#### 6、关于空的查询操作(is null,is not null)

```mysql
查询没有补助的员工信息
select * from dept where comm is null;
查询有补助地员工员工信息
select * from dept where comm is not null;
```



### 模糊查询(like ,% _)

```mysql
#非常常用-条件检索
# % 表示代表n个字符 _代表一个字符

例如: select * from dept where ename like 's%'; 
select * from dept where ename like '%s%';
select * from dept where ename like '_s%';
select * from dept where ename like 's_'; 
```



### 排序查询(order by)

```mysql
根据员工的薪水排序（升序
select * from dept order by sal esc;
根据员工的薪水排序（降序）
select * from dept order by sal desc;
员工入职日期降序查询
select * from dept where  order by HIREDATE esc;
查询职位为MANAGER 的员工信息,并且按照薪资从高到低排序
select * from dept where salgrade='MANAGER' order by sal esc;
```



### 排序查询

```mysql
排序 order【顺序、订单】 by   ..   desc(降序) asc（升序）
#因为排序不影响最后查询结果，所以order by之前不用加任何连接关键字(例如and)，
一般都是结果筛选完成之后在进行排序，所以没有分页就放在最后，如果有分页先排序在分页。

根据员工的薪水排序（升序）
根据员工的薪水排序（降序）
select * from emp order by sal asc;
select * from emp order by sal desc;
员工入职日期降序查询
select * from emp order by hiredate desc;
查询职位为MANAGER 的员工信息,并且按照薪资从高到低排序
select * from emp where job = 'manager' order by sal desc;
```



### 聚合函数

```mysql
SQL中存在一些聚合函数，这些特殊函数是不能直接在where后面当做条件使用的，一般使用方式为 放在返回值项（select  *）

#聚合函数在编辑器中是关键词并且是粉色。

平均数 	avg
总数量	count
总和	sum
最大值	max
最小值	min
```





### 去重复distinct

```mysql
#去重复(忽略人的情况下，查询公司一共有几个部门)
```





### 分组查询group by & having

```mysql
#分组总量不变，前面依然不加连接词（where、and、or...）
找出不同工作类别中的最高薪资

#max分组之后求出每个组中的最大值
select job,max(sal) from emp group by job;

找出不同工作类别中的最高薪资，显示的时候要求按照薪资从高到低显示

#排序SQL别名任何地方都能使用
select job,max(sal) as mm from emp group by job order by mm desc;

求每个部门的平均薪资
select deptno,avg(sal) from emp group by deptno;

求每个岗位的最高薪资 ，除MANAGER之外  
select job,max(sal) from emp where job <> "MANAGER" group by job;

找出每个工作岗位的平均薪水 ，要求显示平均薪水大于2000的 （having）
select job,avg(sal) as t from emp group by job having t>2000;
```



### Limit关键字

```mysql
limit: 极限；
在SQL中limit 一共有两种用法。并且绝对后置的条件。 前面也不用连接关键词。

第一：取前几条数据
语法：limit 条数;
#取公司取公司三个员工
select * from emp limit 3;
#取公司工资最高的三个员工
select * from emp order by sal desc limit 3;
#根据工作分组求平均薪资,并且不要经历,根据平均薪水降序排序,求第一个
select job,avg(sql) as av from emp where job!="MANAGER"
group by job
order by av desc
limit 1;

第二：分页查询
#为什么要分页？
1、为了用体验，不可能一次返回几万条数据给用户。
2、性能考虑，一次返回数据查询慢并且内存消耗极大。

语法：limit 起始索引,返回条数;
```



### 子查询

```mysql
一条SQL成为另一个SQL的条件。（嵌套查询）
找出薪水比公司平均薪水高的员工,要求显示员工的名字和薪水

select *  from emp where sal>(select avg(sal) from emp);
```





## 数据库事务

​        **事务(transaction)，一般是指要做的或已经做的事情。在计算机术语中是指访问并可能更新数据库中各种数据项的一个程序执行单元。事务通常由高级数据库操作语言或编辑语言（C++，SQL或Java）书写的用户程序所引起，并使用形如begin transaction和end transaction语句（或函数调用）来界定。事务由事物开始（begin transaction）和事务结束（end transaction）之间的执行操作所组成。**



**解释:由Java发起的对数据库的操作。这个操作我们叫做事务，这个操作是一条SQL，或者是多条SQL，取决于当前的事务**。

案例：

1、孙浩楠去银行存100W RMB .

分析：修改账号增加100W 一个SQL。 这个一个业务也是一个事务。

2、孙浩楠去银行支出100W借给蟹老板。

分析：浩南哥-100W一条SQL ，蟹老板+100W又是一条SQL。

只要是一件事，或者一系列操作。我们就可以理解为一个事务。

​        某一天银行故障，浩南哥给蟹老板转账的时候当钱从浩南哥转出操作ok，当给蟹老板转入的时候发生故障。 如果没有对应的处理方案，钱会丢失（数据丢失）。 此时如果有事务介入。那么整个流程捆绑成一个事务，一个事务如果执行过程中出现问题，那么可以将信息回滚到起点。



### 事物四个特性☆☆☆☆☆

**事务ACID四个特性，也叫四个原则。**

**原子性：事务不可分割，操作要么都成功，要么都失败。**

**一致性：事务前后操作综合一致。**

**隔离性：多个事务之间互不干扰。**

**持久性：事务提交之后将数据就直接保存在本地数据表中，数据不会应为故障而丢失。**



### [事物的隔离级别](https://www.cnblogs.com/ubuntu1/p/8999403.html)☆☆☆☆☆



**数据库事物的隔离级别有四种,由低到高分别为:**

**1、读未提交(Read uncommitted)**

**2、读提交(Read committed)**

**3、重复读(Repeatable read):MySQL默认**

**4、序列化(Serializable)**

一般使用2，3级别。1执行效率极高但是基本不用，4不会出现任何问题但是效率极慢一般银行使用



#### 读未提交(Read uncommitted)

```text
读未提交，顾名思义，就是一个事务可以读取另一个未提交事务的数据。

事例：老板要给程序员发工资，程序员的工资是3.6万/月。但是发工资时老板不小心按错了数字，按成3.9万/月，该钱已经打到程序员的户口，但是事务还没有提交，就在这时，程序员去查看自己这个月的工资，发现比往常多了3千元，以为涨工资了非常高兴。但是老板及时发现了不对，马上回滚差点就提交了的事务，将数字改成3.6万再提交。

分析：实际程序员这个月的工资还是3.6万，但是程序员看到的是3.9万。他看到的是老板还没提交事务时的数据。这就是脏读。
```

**读未提交(脏读现象):一个事务读取到了另一个没有提交的事务的数据.**



#### 读提交(Read committed)

```text
读提交，顾名思义，就是一个事务要等另一个事务提交后才能读取数据。

事例：程序员拿着信用卡去享受生活（卡里当然是只有3.6万），当他埋单时（程序员事务开启），收费系统事先检测到他的卡里有3.6万，就在这个时候！！程序员的妻子要把钱全部转出充当家用，并提交。当收费系统准备扣款时，再检测卡里的金额，发现已经没钱了（第二次检测金额当然要等待妻子转出金额事务提交完）。程序员就会很郁闷，明明卡里是有钱的…

分析：这就是读提交，若有事务对数据进行更新（UPDATE）操作时，读操作事务要等待这个更新操作事务提交后才能读取数据，可以解决脏读问题。但在这个事例中，出现了一个事务范围内两个相同的查询却返回了不同数据，这就是不可重复读。
```



**读提交解决了脏读的问题,但是事务开始后,查询操作需要等修改操作执行完成之后才能再次查询.出现了不可重复读现象**



#### 重复读(Repeatable read)

```mysql
重复读，就是在开始读取数据（事务开启）时，不再允许修改操作

事例：程序员拿着信用卡去享受生活（卡里当然是只有3.6万），当他埋单时（事务开启，不允许其他事务的UPDATE修改操作），收费系统事先检测到他的卡里有3.6万。这个时候他的妻子不能转出金额了。接下来收费系统就可以扣款了。

分析：重复读可以解决不可重复读问题。写到这里，应该明白的一点就是，不可重复读对应的是修改，即UPDATE操作。但是可能还会有幻读问题。因为幻读问题对应的是插入INSERT操作，而不是UPDATE
```

**重复读(效率更高,更为严谨):重复读解决了不可重复读的问题,但是会出现幻读的问题.不可从复读对应的修改操作,幻读对应的增加操作.**



#### 序列化(Serializable)

```text
Serializable 是最高的事务隔离级别，在该级别下，事务串行化顺序执行，可以避免脏读、不可重复读与幻读。但是这种事务隔离级别效率低下，比较耗数据库性能，一般不使用。
```

**序列化:不会出现任何异常,但是效率极其低下   -- 一般银行使用.**



















