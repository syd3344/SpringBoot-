## 单表查询

### 题1--员工部门

#### 表及数据

```sql
 create table salTab(
   eid int primary key auto_increment COMMENT '员工id', 
   dept_id varchar(10) COMMENT '部门id',
   salary double COMMENT '工资',
   offer_time char(6) COMMENT '入职时间',
   level int   COMMENT '员工等级'
 )

insert into salTab  values
(null,'1001',300000.00,'201009',0),
(null,'1002',15000.00,'201303',1),
(null,'1002',25000.00,'201208',1),
(null,'1002',19000.00,'201111',1),
(null,'1002',30000.00,'200904',1),
(null,'1003',5000.00,'201409',2),
(null,'1003',6000.00,'201509',2),
(null,'1003',6500.00,'201408',2),
(null,'1003',6500.00,'201409',2),
(null,'1003',7500.00,'201409',2),
(null,'1003',7500.00,'201601',2),
(null,'1003',5500.00,'201509',2),
(null,'1003',8500.00,'201409',2),
(null,'1003',9000.00,'201309',2),
(null,'1003',10000.00,'201209',2),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4000.00,'201609',3),
(null,'1004',4500.00,'201509',3),
(null,'1004',4500.00,'201509',3),
(null,'1004',4500.00,'201509',3),
(null,'1004',4500.00,'201509',3),
(null,'1004',4500.00,'201509',3),
(null,'1004',4500.00,'201509',3),
(null,'1004',3000.00,'201509',3),
(null,'1004',3000.00,'201509',3),
(null,'1004',3000.00,'201509',3),
(null,'1004',3000.00,'201509',3),
(null,'1004',2500.00,'201612',3),
(null,'1004',2500.00,'201612',3),
(null,'1004',2500.00,'201612',3);
```



#### 题

> -- 1、查询员工部门id为1001的所有员工
>
>
> -- 2、查询员工工资大于10000的所有员工
>
>
> -- 3、查询员工工资大于15000且员工等级在(0、1、2)之中的所有员工
>
> -- 4、查询在201409之前入职的所有员工
>
>
> -- 5、查询所有的员工，并按照员工的入职时间排序，先入职的在前
>
> -- 6、统计公司所有员工的平均工资
>
>
> -- 7、统计公司所有员工的工资总和
>
>
> -- 8、统计各个部门的员工平均工资
>
>
> -- 9、找出员工平均工资高于20000.00的部门
>
>
> -- 10、找出公司的最低工资是多少
>
>
> -- 11、找出公司的最低工资的员工是谁
>
>
> -- 12、找出公司的最高工资是多少
>
>
> -- 13、找出公司的最高工资的员工是谁
>
>
> -- 14、找出各个部门的最高工资是多少
>
>
> ​	 
>



### 题2--水果

#### 表及数据

> - A：对应着每种水果；B：对应着每种水果的价格

```sql
create table A(
  A_ID int primary key auto_increment,
  A_NAME varchar(20) not null
);
insert into A values(1,'苹果');
insert into A values(2,'橘子');
insert into A values(3,'香蕉');

create table B( 
   A_ID int primary key auto_increment,
   B_PRICE double
);
insert into B values(1,2.30);
insert into B values(2,3.50);
insert into B values(4,null);

```



#### 题

> - 1、 查询价格最贵的水果名称
> - 2、 显示最贵的水果 以及 最贵水果的价格



### 题3--教师学生

#### 表及数据

> - teacher 教师表
> - student 学生表
> - cource 课程表
> - studentcource 选课表  学生和课程的关系表

```sql
CREATE TABLE teacher (
  id int(11) NOT NULL primary key auto_increment,
  name varchar(20) not null unique
 );
CREATE TABLE student (
  id int(11) NOT NULL primary key auto_increment,
  name varchar(20) NOT NULL unique,
  city varchar(40) NOT NULL,
  age int 
) ;
CREATE TABLE course(
  id int(11) NOT NULL primary key auto_increment,
  name varchar(20) NOT NULL unique,
  teacher_id int(11) NOT NULL,
  FOREIGN KEY (teacher_id) REFERENCES teacher (id)
);

CREATE TABLE studentcourse (
   student_id int NOT NULL,
   course_id int NOT NULL,
   score double NOT NULL,
   FOREIGN KEY (student_id) REFERENCES student (id),
   FOREIGN KEY (course_id) REFERENCES course (id)
);

insert into teacher values(null,'关羽');
insert into teacher values(null,'张飞');
insert into teacher values(null,'赵云');

insert into student values(null,'小王','北京',20);
insert into student values(null,'小李','上海',18);
insert into student values(null,'小周','北京',22);
insert into student values(null,'小刘','北京',21);
insert into student values(null,'小张','上海',22);
insert into student values(null,'小赵','北京',17);
insert into student values(null,'小蒋','上海',23);
insert into student values(null,'小韩','北京',25);
insert into student values(null,'小魏','上海',18);
insert into student values(null,'小明','北京',20);

insert into course values(null,'语文',1);
insert into course values(null,'数学',1);
insert into course values(null,'生物',2);
insert into course values(null,'化学',2);
insert into course values(null,'物理',2);
insert into course values(null,'英语',3);

insert into studentcourse values(1,1,80);
insert into studentcourse values(1,2,90);
insert into studentcourse values(1,3,85);
insert into studentcourse values(1,4,78);
insert into studentcourse values(2,2,53);
insert into studentcourse values(2,3,77);
insert into studentcourse values(2,5,80);
insert into studentcourse values(3,1,71);
insert into studentcourse values(3,2,70);
insert into studentcourse values(3,4,80);
insert into studentcourse values(3,5,65);
insert into studentcourse values(3,6,75);
insert into studentcourse values(4,2,90);
insert into studentcourse values(4,3,80);
insert into studentcourse values(4,4,70);
insert into studentcourse values(4,6,95);
insert into studentcourse values(5,1,60);
insert into studentcourse values(5,2,70);
insert into studentcourse values(5,5,80);
insert into studentcourse values(5,6,69);
insert into studentcourse values(6,1,76);
insert into studentcourse values(6,2,88);
insert into studentcourse values(6,3,87);
insert into studentcourse values(7,4,80);
insert into studentcourse values(8,2,71);
insert into studentcourse values(8,3,58);
insert into studentcourse values(8,5,68);
insert into studentcourse values(9,2,88);
insert into studentcourse values(10,1,77);
insert into studentcourse values(10,2,76);
insert into studentcourse values(10,3,80);
insert into studentcourse values(10,4,85);
insert into studentcourse values(10,5,83);

```

#### 题目

> - 1、查询不及格的学生
> - 2、查询获得最高分的学生信息
> - 3、查询编号2的课程比编号1的课程的最高成绩高的学生信息
> - 4、查询平均成绩大于70分的同学的学号和平均成绩
> - 5、查询所有同学的学号、姓名、选课数、总成绩
> - 6、查询学过赵云老师所教课的同学的学号、姓名
> - 7、查询没学过关羽老师课的同学的学号、姓名
> - 8、查询没有学三门课以上的同学的学号、姓名
> - 9、查询各科成绩最高和最低的分
> - 10、查询学生信息和平均成绩
> - 11、查询上海和北京学生数量
> - 12、查询不及格的学生信息和课程信息
> - 13、统计每门课程的学生选修人数（超过四人的进行统计）



### 题4--部门员工

#### 表及数据

```sql
-- 部门表

create table dept(

​       deptno int primary key auto_increment, -- 部门编号

​       dname varchar(14) ,       -- 部门名字

​       loc varchar(13)   -- 地址

) ;

-- 员工表

create table emp(

​       empno int primary key auto_increment,-- 员工编号

​       ename varchar(10), -- 员工姓名                                                               -

​       job varchar(9),      -- 岗位

​       mgr int,   -- 直接领导编号

​       hiredate date, -- 雇佣日期，入职日期

​       sal int, -- 薪水

​       comm int,  -- 提成

​       deptno int not null, -- 部门编号

​       foreign key (deptno) references dept(deptno)

);

insert into dept values(10,'财务部','北京');

insert into dept values(20,'研发部','上海');

insert into dept values(30,'销售部','广州');

insert into dept values(40,'行政部','深圳');

insert into emp values(7369,'刘一','职员',7902,'1980-12-17',800,null,20);

insert into emp values(7499,'陈二','推销员',7698,'1981-02-20',1600,300,30);

insert into emp values(7521,'张三','推销员',7698,'1981-02-22',1250,500,30);

insert into emp values(7566,'李四','经理',7839,'1981-04-02',2975,null,20);

insert into emp values(7654,'王五','推销员',7698,'1981-09-28',1250,1400,30);

insert into emp values(7698,'赵六','经理',7839,'1981-05-01',2850,null,30);

insert into emp values(7782,'孙七','经理',7839,'1981-06-09',2450,null,10);

insert into emp values(7788,'周八','分析师',7566,'1987-06-13',3000,null,20);

insert into emp values(7839,'吴九','总裁',null,'1981-11-17',5000,null,10);

insert into emp values(7844,'郑十','推销员',7698,'1981-09-08',1500,0,30);

insert into emp values(7876,'郭靖','职员',7788,'1987-06-13',1100,null,20);

insert into emp values(7900,'令狐冲','职员',7698,'1981-12-03',950,null,30);

insert into emp values(7902,'张无忌','分析师',7566,'1981-12-03',3000,null,20);

insert into emp values(7934,'杨过','职员',7782,'1983-01-23',1300,null,10);

 
```



#### 题目

> -- 1．列出至少有一个员工的所有部门。
>
>  
>
> -- 2．列出薪金比"刘一"多的所有员工。
>
>  
>
> -- 3．***** 列出所有员工的姓名及其直接上级的姓名。
>
>  
>
> -- 4．列出受雇日期早于其直接上级的所有员工。
>
>  
>
> -- 5．列出部门名称和这些部门的员工信息，同时列出那些没有员工的部门。
>
>  
>
> -- 6．列出所有job为“职员”的姓名及其部门名称。
>
>  
>
> -- 7．列出最低薪金大于1500的各种工作。
>
>  
>
> -- 8．列出在部门 "销售部" 工作的员工的姓名，假定不知道销售部的部门编号。
>
>  
>
> -- 9．列出薪金高于公司平均薪金的所有员工。
>
>  
>
> -- 10．列出与"周八"从事相同工作的所有员工。
>
>  
>
> -- 11．列出薪金等于部门30中员工的薪金的所有员工的姓名和薪金。(只要和部门30中任意一个员工的薪资相等即可)
>
>  
>
> -- 12．列出薪金高于在部门30工作的所有员工的薪金的员工姓名和薪金。
>
>  
>
> -- 13．列出在每个部门工作的员工数量、平均工资。
>
>  
>
> -- 14．列出所有员工的姓名、部门名称和工资。
>
>  
>
> -- 15．列出所有部门的详细信息和部门人数。
>
>  
>
> -- 16．列出各种工作的最低工资。
>
>  
>
> -- 17．列出各个部门的 经理 的最低薪金。
>
>  
>
> -- 18．列出所有员工的年工资,按年薪从低到高排序。 
>
>  
>
> -- 19.查出emp表中薪水在3000以上（包括3000）的所有员工的员工号、姓名、薪水。
>
>  
>
> -- 20.查询出所有薪水在'陈二'之上的所有人员信息。
>
>  
>
> -- 21.查询出emp表中部门编号为20，薪水在2000以上（不包括2000）的所有员工，显示他们的员工号，姓名以及薪水，以如下列名显示：员工编号 员工名字 薪水
>
>  
>
> -- 22.查询出emp表中所有的工作种类（无重复）
>
>  
>
> -- 23.查询出所有奖金（comm）字段不为空的人员的所有信息。
>
>  
>
> -- 24.查询出薪水在800到2500之间（闭区间）所有员工的信息。
>
>  
>
> -- 25.查询出员工号为7521，7900，7782的所有员工的信息。
>
>  
>
> -- 26.查询出名字中有“张”字符，并且薪水在1000以上（不包括1000）的所有员工信息。
>
>  
>
> -- 27.查询出名字第三个汉字是“忌”的所有员工信息。
>
>  
>
> -- 28.将所有员工按薪水升序排序，薪水相同的按照入职时间降序排序。
>
>  
>
> -- 29.将所有员工按照名字首字母升序排序，首字母相同的按照薪水降序排序。 order by convert(name using gbk) asc; 
>
>  
>
> -- 30.查询出最早工作的那个人的名字、入职时间和薪水。
>
>  
>
> -- 31.显示所有员工的名字、薪水、奖金，如果没有奖金，暂时显示100.
>
>  
>
> -- 32.显示出薪水最高人的职位。
>
>  
>
> -- 33.查出emp表中所有部门的最高薪水和最低薪水，部门编号为10的部门不显示。
>
>  
>
> -- 34.删除10号部门薪水最高的员工。
>
>  
>
> -- 35.将薪水最高的员工的薪水降30%。
>
>  
>
> -- 36.查询员工姓名，工资和 工资级别(工资>=3000 为3级，工资>2000 为2级，工资<=2000 为1级)
>
> 语法：case when ... then ... when ... then ... else ... end



## 多表查询

### 题1--学生薪资

#### 表及数据

```sql
CREATE TABLE students (
    stu_no      CHAR(4)             PRIMARY KEY     COMMENT '学员id',  
    birth_date  DATE            NOT NULL         	COMMENT '学员生日',
    name  VARCHAR(14)           NOT NULL 			COMMENT '学员的名字',
    gender      ENUM ('M','F')  NOT NULL 			COMMENT '学员性别',    
    enter_date   DATE            NOT NULL 			COMMENT '入学日期'
) COMMENT '学生表';

CREATE TABLE salaries (
    stu_no      CHAR(4)         NOT NULL COMMENT '学生ID',
    salary      double          NOT NULL COMMENT '工资',
    month       INT             NOT NULL COMMENT '发工资月份',
    level       INT             NOT NULL COMMENT '工资等级',
    FOREIGN KEY (stu_no) REFERENCES students (stu_no) ON DELETE CASCADE
) COMMENT '薪资表';

insert into students values
('100','1990-08-19','JACK','M','20160811'),
('101','1970-08-12','TOM','M','20100606'),
('102','1996-03-19','JAMES','M','20140101'),
('103','1987-04-28','KETTY','F','20130910'),
('104','1983-05-19','JIM','F','20160418');

insert  into `salaries`(`stu_no`,`salary`,`month`,`level`) 
values (100,12000,201601,2),
(101,9000,201601,1),
(102,13000,201601,3),
(103,8300,201601,1),
(104,9500,201601,1),
(100,12200,201602,2),
(101,9200,201602,1),
(102,13200,201602,3),
(103,8500,201602,1),
(104,9700,201602,1),
(100,12400,201603,2),
(101,9400,201603,1),
(102,13400,201603,3),
(103,8700,201603,1),
(104,9900,201603,1),
(100,12600,201604,2),
(101,9600,201604,1),
(102,13600,201604,3),
(103,8900,201604,1),
(104,10100,201604,1),
(100,12800,201605,2),
(101,9800,201605,1),
(102,13800,201605,3),
(103,9100,201605,1),
(104,10300,201605,1),
(100,12800,201606,2),
(101,9800,201606,1),
(102,13800,201606,3),
(103,9100,201606,1),
(104,10300,201606,1),
(100,13000,201607,2),
(101,10000,201607,1),
(102,14000,201607,3),
(103,9300,201607,1),
(104,10500,201607,1),
(100,13200,201608,2),
(101,10200,201608,1),
(102,14200,201608,3),
(103,9500,201608,1),
(104,10700,201608,1),
(100,13400,201609,2),
(101,10400,201609,1),
(102,14400,201609,3),
(103,9700,201609,1),
(104,10900,201609,1),
(100,13600,201610,2),
(101,10600,201610,1),
(102,14600,201610,3),
(103,9900,201610,1),
(104,11100,201610,1),
(100,13800,201611,2),
(101,10800,201611,1),
(102,14800,201611,3),
(103,10100,201611,1),
(104,11300,201611,1),
(100,14000,201612,2),
(101,11000,201612,1),
(102,15000,201612,3),
(103,10300,201612,1),
(104,11500,201612,1);
```



#### 题

> -- 1、查看学生总数(students)
>
> -- 2、查询学员JAMES的每个月都发了多少工资(students、salaries)
>
> -- 3、查询学员JAMES在201602月发了多少工资(students、salaries)
>
> -- 4、查询学员JAMES的2016年年薪(students、salaries)
>
> -- 5、查询学员JAMES的月平均工资(students、salaries)



### 题2--员工薪资

#### 表及数据

```sql
-- 部门表
CREATE TABLE dept (
  id INT PRIMARY KEY PRIMARY KEY, -- 部门id
  dname VARCHAR(50), -- 部门名称
  loc VARCHAR(50) -- 部门所在地
);

-- 添加4个部门
INSERT INTO dept(id,dname,loc) VALUES 
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');



-- 职务表，职务名称，职务描述
CREATE TABLE job (
  id INT PRIMARY KEY,
  jname VARCHAR(20),
  description VARCHAR(50)
);

-- 添加4个职务
INSERT INTO job (id, jname, description) VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');



-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id
  ename VARCHAR(50), -- 员工姓名
  job_id INT, -- 职务id
  mgr INT , -- 上级领导
  joindate DATE, -- 入职日期
  salary DECIMAL(7,2), -- 工资
  bonus DECIMAL(7,2), -- 奖金
  dept_id INT, -- 所在部门编号
  CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
  CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
);

-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);



-- 工资等级表
CREATE TABLE salarygrade (
  grade INT PRIMARY KEY,   -- 级别
  losalary INT,  -- 最低工资
  hisalary INT -- 最高工资
);

-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);
```



#### 题

> -- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
>
> -- 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
>
> -- 3.查询员工姓名，工资，工资等级
>
> -- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
>
> -- 5.查询出部门编号、部门名称、部门位置、部门人数
>
> -- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询