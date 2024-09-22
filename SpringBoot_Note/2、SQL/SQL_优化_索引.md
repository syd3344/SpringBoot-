## 概述

> - 类似于一本书的目录。
>   - 书中所有内容通过页码存储在不同的页上
>   - 目录提供了快速找到某个内容的路径
>   - 数据库的索引用于提高数据检索的效率
> - 允许数据库快速找到指定列的值，而不用遍历整个表。
> - 它通过为特定列或多列创建一个排序的结构，帮助数据库引擎更快地找到所需的数据。
> - 本质就是一个数据结构



## 种类

> - 单列索引
>   - 在一列上创建索引
> - 复合索引
>   - 在多列上创建索引
> - 唯一索引
>   - 索引列中的所有制唯一
> - 全文索引
>   - 用于文本搜索

## 基本使用

> 表如下

```mysql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    department VARCHAR(100),
    salary DECIMAL(10, 2)
);

```

### 单列索引

#### 概述

> - 只能加速针对该列的查询。其他列的查询无法受益。

#### 创建索引

> - 创建了 `name` 列的索引后，查询name的效率将显著提高。
> - 数据库会首先在 `idx_name` 索引中查找相关记录，再根据找到的记录指针返回具体的数据行。

```mysql
create index idx_name on employees(name)
```

#### 查看表中索引

```mysql
show index from employees
```

#### 删除表中索引

```mysql
drop index idx_name on employees;
```



### 复合索引

#### 概述

> - 包含多个条件的查询，可以创建复合索引
> - 可以**加速多个列的查询**，但受最左前缀匹配原则的限制
> - 复合索引类似于字典的**多层排序**。
>   - 比如你要查找姓氏为 "Smith" 且名字为 "John" 的人，字典会先根据姓氏排序，然后在所有姓氏为 "Smith" 的人中，再根据名字排序。同理，复合索引将两个或更多列按顺序存储，查询时会优先使用第一个列的索引，如果第一个列的值相同，才会使用第二个列的值。

#### 创建

```mysql
create index_idx_department on employees(name,department) 
```

> - 该复合索引是在 `name` 和 `department` 两列上创建的，因此可以加速如下的多列查询

```my
select * from employees where name = `Alice` and department = `Engineering`;
```

#### 左缀原则

> - 复合索引按列的顺序存储，查询时至少需要用到索引中的第一个列，才能发挥索引的作用。
>   - 例如：CREATE INDEX idx_name_department ON employees(name, department);
>     - 查询 `name` 列时索引有效，因为 `name` 是第一个列
>     - 查询 `name` 和 `department` 时索引也有效
>     - 但只查询 `department` 列时，索引不会生效，因为它不是最左的列



## 底层

> 一般采用**B+树**或**哈希表**等数据结构来实现，以提高查询的效率

### B+树

> - 多分支
> - 数据在叶子节点
> - **非叶子节点**的主要作用是充当目录或索引，帮助快速定位叶子节点中的数据。
> - 它们存储的是索引键（即索引列的值）和指向子节点的指针，而不存储实际的数据。



## 优缺点

### 优

> - 查询
> - 排序

### 缺

> - 花费空间
> - 写操作会慢



## 使用场景

> - 数据量大且少更新
> - 常用作Where或group by 或order 的查询条件

































