# python

### 条件

* 语法

```
select * from 表名 where 条件
例：select * from students where id = 1;

```
* where后面支持多种运算符，进行条件的处理

```
1.比较运算符（=, >, >=, <, <=, != 或 <>）
2.逻辑运算符 (and, or, not)
3.模糊查询
   - like
   - %表示任意多个字符
   - _表示一个任意字符
4.范围查询
    - in表示在一个非连继续的范围内
    - between...and...表示在连续范围内
5.空判断
    - 注意：null与""是不同的
    - 判空is null
6. 优先级由高到低的顺序为：小括号，not, 比较运算符，逻辑运算符
7.and比or先运算，如果同时出现并希望先算or，需要结合()使用
``` 
### 排序

* 为了方便查看数据，可以对数据进行排序

```
# 语法
select * from 表名 order by 列1 asc | desc [,列2 asc | desc,...]
# 说明
- 将行数据按照列1进行排序，如果某些行列1的值相同时，则按照列2排序，以此类推
- 默认按照列值从小到大排列（asc）
- asc从小到大排列，即升序
- desc从大到小排序，即降序
```
### 聚合函数
* 为了快速得到统计数据，经常会用到如下5个聚合函数
```
- count(*)表示计算总行数，括号中写星与列名，结果时相同的
- max(列)表示求此列的最大值
- min(列)表示求此列的最小值
- sum(列)表示求此列的和
- avg(列)表示求此列的平均值
- round(123.23, 1)保留几位小数，四舍五入
```
### 分组
##### group by 
* group by的含义：将查询结果按照1个或多个字段进行分组，字段值相同的为一组
* group by可用于单个字段分组，也可用于多个字段返祖
##### group by + group_concat()
* group_concat(字段名)可以作为一个输出字段来使用
* 表示分组后，根据分组结果，使用group_concat()来放置每一组的某字段的值的集合
##### group by + 集合函数
* 通过group_concat()的启发，我们既然可以统计出每个分组的某字段的值的集合，那么我们也可以通过集合函数来对这个值的集合做一些操作
##### group by + having
* having 条件表达式：用来分组查询后指定一些条件来输出查询结果
* having 作用和where一样，但having只能用于group by 
##### group by + with rollup
* with rollup 的作用是:在最后新赠一行，来记录当前列里所有的记录的总和
### 分页
* 当数据量过大时，在一页中查看数据是一件非常麻烦的事情
```
# 语法
select * from 表名 limit start, count
# 说明
从start开始，获取count条数据
```
### 连接查询
* 当查询结果的列来源多张表时，需要将多张表连接成一个大的数据集，再选择合适的列返回
```
1.内连接查询：查询结果为两个表匹配到的数据
 - inner join...on
2.右连接查询：查询的结果为两个表匹配到的数据，右表特有的数据，对于左表中不存在的数据使用null填充
 - right join...on
3.左连接查询：查询的结果为两个匹配到的数据，左表特有的数据，对于右表中不存在的数据使用null填充
 - left join...on
```
### 子查询
* 在一个select语句中，嵌入了另外一个select 语句，那么被嵌入的select语句称之为子查询语句
* 如果一个查询语句查出来的数据是有多行多列组成，那么称之为表级子查询
* 如果一个查询语句中有多个表，那么在select后面指定字段时，要说明表名，即表名，字段名
##### 主查询和子查询的关系
* 子查询是嵌入到主查询中
* 子查询时辅助主查询的，要么充当条件，要么充当数据源
* 子查询是可以独立存在的语句，是一条完整的select 语句
##### 子查询分类
* 标量子查询：子查询返回的结果是一个数据（一行一列）
* 列子查询：返回的结果是一列（一列多行）
* 行子查询：发挥的结果是一行（一行多列）
* 多行多列
### 查询完整格式
```
SELECT select_expr [,select_expr,...] [
                FROM tb_name
                [WHERE 条件判断]
                [GROUP BY {col_name | postion} [ASC | DESC], ...]
                [HAVING WHERE 条件判断]
                [ORDER BY {col_name | expr | postion} [ASC | DESC], ...]
                [  LIMIT {[offset,]rowcount | row_count OFFSET offset}
]
# 完整的select语句
select distinct * 
from 表名
where ....
group by ... having ...
order by ...
limit start,count
```

