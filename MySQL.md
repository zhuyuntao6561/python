# python

### 数据库知识点

* 数据库
* RDBMS
* SQL
* MySQL

#### 传统数据记录的缺点

* 不易保存
* 备份困难
* 查找不便

#### 现代化手段--数据库

* 持久化存储
* 读写速度极高
* 保证数据的有效性
* 对程序支持性非常好，容易扩展

#### 数据库就是一种特殊的文件，其中存储着需要的数据

##### 关系型数据库核心元素

* 数据行（记录）
* 数据列（字段）
* 数据表（数据行的集合）
* 数据库（数据表的集合）

##### RDBMS

* 两种类型：关系型数据库、非关系型数据库
* 定义：所谓的关系型数据库RDBMS，是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据
* 主要产品
```
    - oracle:在以前的大型醒目中使用，银行，电信等项目
    - mysql:web时代使用最广泛的关系型数据库
    - ms sql server: 在微软的项目中使用
    - sqlite: 轻量级数据库，主要应用在移动平台
```

#### SQL

* SQL语句主要分为：

```

 - DQL：数据查询语言，用于对数据进行查询，如select
 - DML: 数据库操作语言，对数据进行增加、修改、删除，如insert、udpate、delete
 - TPL: 事务处理语言，对事务进行处理，包括begin transaction、commit、rollback
 - DCL: 数据库控制语言，进行授权与权限收回，如grant、revoke
 - DDL: 数据定义语言，进行数据库、表的管理等，如create、drop
 - CCL: 指针控制语言，通过控制指针文成表的操作，如declare cursor

```
* 对于web程序员来讲，重点是数据的crud（增删改查），必须熟练编写DQL、DML能够编写DDL完成数据库、表操作 
* SQL是一门特殊的语言，专门用来操作关系数据库
* 不区分大小写

#### MySql特点

* 使用C和C++编写，并使用了多种编译器进行测试，保证源代码的可移植性
* 支持多种操作系统
* 为多种编程语言提供了API
* 主持多线程，充分利用CPU资源
* 优化的SQL查询算法，有效地提高查询速度
* 提供多语言支持，常见的编码如GB2312、BIG5、UTF8
* 提供TCP/IP、ODBC和JDBC等多种数据库连接途径
* 提供用于管理、检查、优化数据库操作的管理工具
* 大型的数据库。可以处理拥有上千万条记录的大型数据库
* 支持多种存储引擎
* MySQL软件采用了双授权政策，它分为这区版和商业版，由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发都选择MySQL作为网站数据库
* MySQL使用标准的SQL数据库语言形式
* Mysql是可以定制的，采用GPL协议，你可以修改源码来开发自己的Mysql系统
* 在线DDL更改功能
* 复制全局事务标识
* 复制无崩溃从机
* 复制多线程从机

#### 数据的完整性

* 一个数据库就是一个完整的业务单元，可以包含多张表，数据被存储在表中
* 在表中为了更加准确的存储数据，保证数据的正确有效，可以在创建表的时候，为表添加一些强制性的验证，包括数据字段的类型、约束

##### 数据类型

* 可以通过查看帮助文档查阅所有支持的数据类型
* 使用数据类型的原则是：够用就行，尽量使用取值范围小的，而不用大的，这样可以更多的节省存储空间

##### 常用数据类型

```

 - 整数：int, bit
 - 小数：decimal
 - 字符串：varchar,char
 - 日期时间：data, time, datetime
 - 枚举类型（enum）

```
##### 特别说明的类型

-  decimal 表示浮点数，如decimal(5,2)表示共存5位数，小数占2位
- char表示固定长度的字符串，如char(3),如果填充"ab"时会补一个空格为"ab "
- varchar表示可变长度的字符串，如varchar(3),填充"ab"时就会存储"ab"
- 字符串text表示存储大文本，当字符串大于4000时推荐使用
- 对于图片、音频、视频等文件，不存储在数据库中，而是上传到某个服务器上，然后在表中存储这个文件的保存路径

##### 约束

* 主键primary key: 物理上存储的顺序
* 非空not null : 此字段不允许填写空值
* 唯一unique : 此字段的值不允许重复
* 默认default : 当不填写此值时会使用默认值，如果填写时以填写为准
* 外键foreign key: 对关系字段进行约束，当为关系字段填写时，会到关联的表中查询此值是否存在，如果存在则填写成功，如果不存在则填写失败并抛出异常

#### 数据库操作

```

* 退出 ：exit ;quit; ctrl+d
* 显示时间： select now();
* 查看所有数据库：show databases;
* 创建数据库： create database 数据库名 charset=utf8;
* 查看数据库的结构：show create database 数据库名;
* 删除数据库（慎慎慎用）：drop database 数据库名；
* 使用数据库：use 数据库名；
* 查看使用的数据库：select database();

```

#### 数据表操作

* 查看当前数据库中所有表：show tables;
* 创建表的基本用法：
```
create table 表名(
id int unsigned primary key auto_increment not null,
name varchar(30) not null,
age tinyint unsigned default 0,
high decimal(5,2),
gender enum("男", "女", "保密") default "保密"
)
```
* 查看表的创建语句：show create table 表名；
* 查看表结构：desc 表名；
* 修改表-添加字段：alter table 表名 add 字段名 类型；
* 修改表-修改字段（不重命名）：alter table 表名 modify 字段名 类型及约束；
* 修改表-修改字段（重命名）：alter table 表名 change 原字段名 新字段名 类型及约束；
* 修改表-删除字段：alter table 表名 drop 字段名；
* 修改表-删除表： drop table 表名；

#### 数据库数据操作

* 查询数据：select * from 表名；select 列1，列2，...from  表名；
  - 使用as给字段起别名 ；也可以给表起别名
  - 在select后面列前面使用distinct可以消除重复行
* 插入数据：insert into 表名 values(...)
* 修改数据：update 表名 set 列1=值1，列2=值2... where 条件;
* 删除数据：delete from 表名 where 条件；
```
    - 逻辑删除：alter table 表名 add 新字段名 bit(1)  default 0;
                        update 表名 set 新字段名=1 where 条件；
                        select * from 表名 where 新字段名=0；
```
* 修改数据库编码:

```

# 应用于之后创建的表
alter database 数据库名 charset=utf8;
# 应用之后马上可以使用
ALTER TABLE 表名 CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;

```
    
