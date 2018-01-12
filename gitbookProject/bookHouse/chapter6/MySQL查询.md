# MySQL查询

查询所有字段
```
select * from 表名;
例：
select * from students;
```

给列或者表起别名
```
select id as ID编号,name as 姓名 from students as 学生表;
```

消除重复行
```
create table temp(
id int not null,
name varchar(10),
isdelete bit default 0
);
insert into temp(id,name) values(1,'xx1');
insert into temp(id,name) values(1,'xx1');
insert into temp(id,name) values(1,'xx2');
insert into temp(id,name) values(1,'xx2');
insert into temp(id,name) values(1,'xx3');
insert into temp(id,name) values(1,'xx4');

insert into temp(id,name) values(2017121401,'01');
insert into temp(id,name) values(2017121402,'01');
insert into temp(id,name) values(2017121403,'01');
insert into temp(id,name) values(2017121404,'01');

select distinct id from temp;
select distinct name from temp;
select distinct name,id from temp;
```
```
例1：查询编号大于3的学生
select * from students where id>3;

例2：查询编号不大于4的科目
select * from subjects where id<=4;

例3：查询姓名不是“黄蓉”的学生
select * from students where sname!='黄蓉';

例4：查询没被删除的学生
select * from students where isdelete=0;

例5：查询编号大于3的女同学
select * from students where id>3 and gender=0;

例6：查询编号小于4或没被删除的学生
select * from students where id<4 or isdelete=0;

例7：查询姓黄的学生
select * from students where sname like '黄%';

例8：查询姓黄并且名字是一个字的学生
select * from students where sname like '黄_';

例9：查询姓黄或叫靖的学生
select * from students where sname like '黄%' or sname like '%靖';

例10：查询编号是1或3或8的学生
select * from students where id in(1,3,8);

例11：查询学生是3至8的学生
select * from students where id between 3 and 8;

例12：查询学生是3至8的男生
select * from students where id between 3 and 8 and gender=1;

例13：查询没有填写地址的学生
select * from students where hometown is null;

例14：查询填写了地址的学生
select * from students where hometown is not null;

例15：查询填写了地址的女生
select * from students where hometown is not null and gender=0;

```

## 优先级

- 优先级由高到低的顺序为：小括号，not，比较运算符，逻辑运算符
- and比or先运算，如果同时出现并希望先算or，需要结合()使用

## 聚合函数

- 为了快速得到统计数据，经常会用到如下5个聚合函数:
- count(), max(), min(), sum(), avg()
```
例1：查询学生总数
select count(*) from students;

例2：查询女生的编号最大值
select max(id) from students where gender=0;

例3：查询未删除的学生最小编号
select min(id) from students where isdelete=0;

例4：查询男生的编号之和
select sum(id) from students where gender=1;

例5：查询未删除女生的编号平均值
select avg(id) from students where isdelete=0 and gender=0;
```

## 分组
- 按照字段分组，表示此字段相同的数据会被放到一个组中
- 语法：
select 列1,列2,聚合... from 表名 group by 列1,列2...
```
例1：查询id号总数
select id as ID编号,count(*)  from temp group by id;
```