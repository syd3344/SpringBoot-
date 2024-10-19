## 概述

> - Data Manipulation Language -- 数据操作语言
>- ==**是用于操作数据库中数据的语言，用的最多，最重要**==
> - DML 的操作对象是数据库中的  **表数据**  ，而非表的结构或定义。

> - **INSERT**：向表中插入数据。
> - **UPDATE**：更新表中的已有数据。
> - **DELETE**：从表中删除数据。
> - **SELECT**：查询表中的数据（尽管 `SELECT` 不直接修改数据，但它是 DML 的一部分）。



## Insert

> - `INSERT` 语句用于向表中添加新数据。

### 插入单行

```mysql
INSERT INTO employees (id, name, age, position, salary) 
VALUES (1, 'John Doe', 30, 'Software Engineer', 75000);
```

### 插入多行数据

```mysql
INSERT INTO employees (id, name, age, position, salary) 
VALUES 
    (2, 'Jane Smith', 28, 'Product Manager', 80000),
    (3, 'Sam Wilson', 35, 'HR Manager', 68000);
```

### 插入部分

```mysql
INSERT INTO employees (name, position) 
VALUES ('Alice', 'Data Scientist');
```

> - ==此语句只向  name  和  position  列插入数据，其他列会使用默认值==



## Update

> - `UPDATE` 语句用于修改表中已有数据。

### ==Set==

> - 用于更新数据库表中现有记录的一个或多个字段的值。它通常与 `UPDATE` 语句一起使用，允许用户通过指定新值来修改特定列。

```sql
UPDATE table_name
SET column1 = value1, 
    column2 = value2, 
    ...
WHERE condition;
```



```sql
UPDATE users
SET name = '李四', 
    age = 28
WHERE id = 1;
```



### 更新单行

> - 将 `id` 为 1 的员工的 `salary` 更新为 85000。

```mysql
UPDATE employees 
SET salary = 85000 
WHERE id = 1;
```



### 更新多行

> - 将所有 `position` 为 `Software Engineer` 的员工职位更新为 `Senior Software Engineer`，并且将他们的 `salary` 提高 5000。

```mysql
UPDATE employees 
SET salary = salary + 5000 
WHERE position = 'Software Engineer';
```



## Delete

> - 用于从表中删除数据

### 删除指定行

> - 删除 `id` 为 3 的员工。

```sql
DELETE FROM employees 
WHERE id = 3;
```

### 删除多行

> - 删除所有 `position` 为 `HR Manager` 的员工。

```sql
DELETE FROM employees 
WHERE position = 'HR Manager';
```

### 删除所有行

> - 删除 `employees` 表中的所有数据，但保留表的结构。

```sql
DELETE FROM employees;
```





## Select

> - 用于从表中查询数据

###  查询所有列

```sql
SELECT * FROM employees;
```

> - 查询 `employees` 表中的所有列和所有行的数据。

###  查询指定列

```sql
SELECT name, position, salary 
FROM employees;
```

> - 只查询 `employees` 表中的 `name`、`position` 和 `salary` 列。

### 使用 Where 条件查询

```sql
SELECT * 
FROM employees 
WHERE age > 30 AND salary >= 70000;
```

> - 查询 `age` 大于 30 且 `salary` 大于等于 70000 的员工

###  **==使用排序查询==**

```sql
SELECT name, salary 
FROM employees 
ORDER BY salary DESC;
```

> - 概述：
>   - **排序查询** 是 SQL 中使用 `ORDER BY` 子句对结果集进行排序的操作。
>   - ==**排序可以根据一个或多个列的值进行**==，并且可以按升序（`ASC`）或降序（`DESC`）排列。
>     - 当第一个列的值相同时，按第二个列的值排序
> - 排序规则
>   - ASC：升序，默认
>   - `DESC`：降序排列

### ==使用分页查询==

#### 基本使用

> - `LIMIT`：用于指定每页查询多少条数据。
> - OFFSET：用于指定从哪一条记录开始返回结果。
> - ![1728037642652](+SQL_DML.assets/1728037642652.png)

```sql
SELECT 列名 FROM 表名
ORDER BY 列名
LIMIT 每页的行数 OFFSET 起始位置;
```



> - 从第 11 行开始（`OFFSET 10`），返回 5 行数据

```sql
SELECT * 
FROM employees 
LIMIT 5 OFFSET 10;
```

#### ==优化==

> - 对于较小的数据集，`LIMIT` 和 `OFFSET` 的分页查询性能较好。然而，对于非常大的数据集，随着页数的增加，`OFFSET` 的性能可能会变差。因为数据库需要扫描跳过前面几页的数据，导致查询变慢
> - 使用主键或索引字段分页











