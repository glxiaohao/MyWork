# mysql 基本操作

安装mysql 服务端/客户的

```
apt-get install mysql-server
apt-get install mysql-client
```

查看mysql版本(检验mysql是否安装成功)

```linux
mysql -V
```

连接mysql数据库

```
mysql -u root -p mysql
```

密码:guoling

退出数据库
```
按ctrl+d或输入quit命令退出
```


数据库
```
查看所有存在的数据库
show databases;
使用数据库
use 数据库名;
查看当前选择的数据库
select database();
创建数据库
create database 数据库名 charset=utf8;
例：
create database python_gl charset=utf8;
删除数据库
drop database 数据库名;
例：
create database python_gl;
```

数据表

```
查看当前数据库中所有表
show tables;
查看表结构
desc 表名;
创建表
auto_increment表示自动增长
create table 表名(列 类型 约束,...);
例：创建班级表
create table classes(
id int unsigned auto_increment primary key not null,
name varchar(10),
isdelete bit default 0
);
例：创建学生表
create table students(
id int unsigned auto_increment primary key not null,
name varchar(10) not null,
gender bit default 1,
hometown varchar(20),
clsid int unsigned,
isdelete bit default 0,
foreign key(clsid) references classes(id)
);
修改表-添加字段
alter table 表名 add 列名 类型;
例：
alter table students add birthday datetime;
修改表-修改字段：重命名版
alter table 表名 change 原名 新名 类型及约束;
例：
alter table subjects change name name1 varchar(20) not null;
修改表-修改字段：不重命名版
alter table 表名 modify 列名 类型及约束;
例：
alter table subjects modify name1 varchar(10) not null;
修改表-删除字段
alter table 表名 drop 列名;
例：
alter table students drop birthday;
删除表
drop table 表名;
例：
drop table students;
查看表的创建语句
show create table 表名;
例：
show create table subjects;
```
实例
```
创建数据库 
create database python_gl charset=utf8;

例：创建班级表
create table classes(
id int unsigned auto_increment primary key not null,
name varchar(10),
isdelete bit default 0
);

例：创建学生表
create table students(
id int unsigned auto_increment primary key not null,
name varchar(10) not null,
gender bit default 1,
hometown varchar(20),
clsid int unsigned,
isdelete bit default 0,
foreign key(clsid) references classes(id) -- 将clsid设为外键,引用classes表的id字段
);

desc table表名
例：desc students

给students表添加一个birthday的字段(问题)
结合准一的数据库存取时间,取出时间
alter table students add birthday datetime;

将classes表中的name改为name2
alter table classes change name name2 varchar(10);
alter table classes change name2 name varchar(20);
alter table classes change name name varchar(10);

将classes表中的birthday列删除
alter table classes drop birthday;

向classes表插入数据
insert into classes values(1,'python',0);
insert into classes values(2,'lua',0);
insert into classes values(3,'go',1);
insert into classes values(4,'Java',1);
insert into classes values(5,'Android',0);

向students表插入数据(给全部列写数据) 0：男	1：女
insert into students values(2017001,'guoling',1,'HuBei HuangGang',1,0);
insert into students values(2017002,'dengzhongqiang',0,'HuBei XianTao',2,0);
insert into students values(2017003,'guohao',0,'HuBei HuangGang',1,0);
insert into students values(2017004,'chensheng',0,'HuBei qichun',5,0);


alter table students change name name varchar(20);
insert into students values(2017002,'dengzhongqiang',0,'HuBei XianTao',2,0);

向students表插入数据(给部分列写数据)
insert into students(id,name) values(2017005,'xx1');
insert into students(id,name) values(2017006,'xx2');
insert into students(id,name) values(2017007,'xx3');
insert into students(id,name) values(2017008,'xx4');

一次向students表插入多条数据(给部分列写数据)
insert into students(id,name) values(2017009,'A'),(2017010,'B'),(2017011,'C');

修改students表中A的性别为女,家乡为'古墓'
update students set gender=1,hometown='gumu' where id='2017009';
update students set gender=1,hometown='古墓' where id='2017009';

alter database python_gl character set utf8; 


删除一条记录
delete from subject where id='2017008';

逻辑删除，本质就是修改操作
update students set isdelete=1 where id='2017005';

备份(出错了)
运行mysqldump命令
mysqldump -uroot -p python_gl > ~/Desktop/python_gl.sql;
mysqldump –uroot –p python_gl > ~/Desktop/py.sql;

```