# oracle 概念

数据库约束有五种：
```
1. 主键约束（PRIMARY KEY）
    主键约束：主键是唯一确定表中的某一行;
    主键有几个需要注意的点：
        1. 键列必须必须具有唯一性，且不能为空，其实主键约束 相当于 UNIQUE+NOT NULL; 
        2. 一个表只允许有一个主键; 
        3. 主键所在列必须具有索引（主键的唯一约束通过索引来实现），如果不存在，将会在索引添加的时候自动创建;
    
    添加主键（约束的添加可在建表时创建，也可如下所示在建表后添加，一般推荐建表后添加，灵活度更高一些，建表时添加某些约束会有限制）
    SQL> alter table emp add constraint emp_id_pk primary key(id);
2. 唯一性约束（UNIQUE)
    唯一性约束：可作用在单列或多列上，对于这些列或列组合，唯一性约束保证每一行的唯一性;
    UNIQUE需要注意：
        1. 对于UNIQUE约束来讲，索引是必须的。如果不存在，就自动创建一个（UNIQUE的唯一性本质上是通过索引来保证的）;
        2.  UNIQUE允许null值，UNIQUE约束的列可存在多个null。这是因为，Unique唯一性通过btree索引来实现，而btree索引中不包含null。当然，这也造成了在where语句中用null值进行过滤会造成全表扫描;
    添加唯一约束       
        SQL> alter table emp add constraint emp_code_uq unique(code);
3. 非空约束（NOT NULL)   
    非空约束作用的列也叫强制列。强制键列中必须有值，当然建表时候若使用default关键字指定了默认值，则可不输入。
    添加非空约束：
        SQL> alter table emp modify ename not null;
4. 外键约束（FOREIGN KEY)
5. 检查约束（CHECK)
......  参考网址: https://user.qzone.qq.com/1973427872/infocenter
```
```

```

## 完整的select语句格式如下
```
select 字段
from 表名
where …….
group by ……..
having …….
order by ……..
以上语句的执行顺序
1.  首先执行where语句过滤原始数据
2.  执行group by进行分组
3.  执行having对分组数据进行操作
4.  执行select选出数据
5.  执行order by排序
```

## having

```
如果想对分组数据再进行过滤需要使用having子句
取得每个岗位的平均工资大于2000
select job, avg(sal) from emp group by job having avg(sal) >2000;
分组函数的执行顺序：
1、 根据条件查询数据
2、 分组
3、 采用having过滤，取得正确的数据
```
数据处理函数
| 函数名              | 含义           | 
| ---------------    |-------------   | 
| Lower              | 转换小写        | 
| upper              | 转换大写        |
| substr             | 取子串        |
| length             | 取长度        |
| trim               | 去空格        |
| to_date            | 将字符串转换成日期|
| to_char            |将日期或数字转换成字符串|
| to_number          | 将字符串转换成数字|
| nvl                | 可以将null转换成一个具体值|
| case               | 分支语句        |
| decode             | 同case        |
| round              | 四舍五入        |

上面函数使用实例如下:
```
1. lower --> 查询员工，将员工姓名全部转换成小写
   select lower(ename) from emp;   
2. expper --> 查询job为manager的员工
   select * from emp where job=upper('manager');
3. substr --> 查询姓名以M开头所有的员工
   select * from emp where substr(ename, 1, 1)=upper('m');
4. length --> 取得员工姓名长度为5的
5. trim --> 会去首尾空格，不会去除中间的空格 
   select * from emp where job=trim(upper('manager  '));
....
参考网址: https://user.qzone.qq.com/1973427872/infocenter
```
 
