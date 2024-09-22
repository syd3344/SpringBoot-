## 概述

> - DDL（Data Definition Language）:数据库定义语言

## 数据类型

### 数值

> ![1726412196377](SQL_DDL.assets/1726412196377.png)

### 字符串

> - cahr
>   - 定长字符串
>   - 若只存一个字符，其他位置用空格代替
>   - 性能好
> - varchar
>   - 变长字符串
>   - 若只存一个字符，则只占一个字符
>   - 性能差

### 日期类型

> - date       日期值      年-月-日
> - time       时间值      时-分-秒
> - year       年
> - datetime       日期+时间值
> - timestamp    最多到2038年



## 库操作

> - show：查询
>   - 查询所有数据库
>     - show databases;		
>   - 查询当前数据库
>     - show database( );
> - create：创建
>   - craete database +库名 ;
> - use：使用
>   - use + 数据库名；
> - drop：删除
>   - drop database [if exists] +库名;
>   - if exists，执行后续代码，不会停止



## 表操作

### 查询

> - 查询当前库所有表
>   - show tables;
> - 查询表结构
>   - desc +表名;
> - 查询指定表的建表语句
>   - show create table + 表名;

### 创建

```mysql
create table tb_dept(
   id int unsigned primary key auto_increment comment '主键ID',
   name varchar(10) not null unique comment '部门名称',
   create_time datetime not null comment '创建时间',
    update_time datetime not null comment '修改时间'
) comment '部门表';
```

### 增加列

```java
ALTER TABLE employees ADD COLUMN department VARCHAR(50);
向 employees 表添加了一个 department 列。
```

### 修改列类型

```
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12, 2);
将 `salary` 列的类型修改为精度更高的 `DECIMAL(12, 2)`。
```

### 删除列

```
ALTER TABLE employees DROP COLUMN age;
删除了 employees 表中的 age 列。
```

### 修改表名

```
ALTER TABLE employees RENAME TO staff;
将表 employees 重命名为 staff
```

### 删除表

```
DROP TABLE employees;
删除 employees 表，所有数据和表结构将一同删除
```

删除表中数据，但不删除结构

```
TRUNCATE TABLE employees;
清空 employees 表中的所有数据，但保留表结构
```





































