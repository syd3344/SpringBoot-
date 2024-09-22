## 概述

> - **是用于操作数据库中数据的语言**，用的最多，最重要
>
> - DML 的操作对象是数据库中的  **表数据**  ，而非表的结构或定义。

> - **INSERT**：向表中插入数据。
> - UPDATE**：更新表中的已有数据。**
> - DELETE**：从表中删除数据。**
> - SELECT*：查询表中的数据（尽管 `SELECT` 不直接修改数据，但它是 DML 的一部分）。



## insert

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

> - 此语句只向 `name` 和 `position` 列插入数据，其他列会使用默认值



## update

> - `UPDATE` 语句用于修改表中已有数据。

### 更新单行

```mysql
UPDATE employees 
SET salary = 85000 
WHERE id = 1;
```

> - 将 `id` 为 1 的员工的 `salary` 更新为 85000。

### 更新多行

```mysql
UPDATE employees 
SET position = 'Senior Software Engineer', salary = salary + 5000 
WHERE position = 'Software Engineer';
```

> - 将所有 `position` 为 `Software Engineer` 的员工职位更新为 `Senior Software Engineer`，并且将他们的 `salary` 提高 5000。



## Delete

> - 用于从表中删除数据

### 删除指定行

```
DELETE FROM employees 
WHERE id = 3;
```

> - 删除 `id` 为 3 的员工。

### 删除多行

```
DELETE FROM employees 
WHERE position = 'HR Manager';
```

> - 删除所有 `position` 为 `HR Manager` 的员工。

### 删除所有行

```
DELETE FROM employees;
```

> - 删除 `employees` 表中的所有数据，但保留表的结构。



## Select

> - 用于从表中查询数据

###  查询所有列

```
SELECT * FROM employees;
```

> - 查询 `employees` 表中的所有列和所有行的数据。

###  查询指定列

```
SELECT name, position, salary 
FROM employees;
```

> - 只查询 `employees` 表中的 `name`、`position` 和 `salary` 列。

### 使用 WHERE 条件查询

```
SELECT * 
FROM employees 
WHERE age > 30 AND salary >= 70000;
```

> - 查询 `age` 大于 30 且 `salary` 大于等于 70000 的员工

###  使用排序查询

```
SELECT name, salary 
FROM employees 
ORDER BY salary DESC;
```

> - 概述：
>   - **排序查询** 是 SQL 中使用 `ORDER BY` 子句对结果集进行排序的操作。
>   - 排序可以根据一个或多个列的值进行，并且可以按升序（`ASC`）或降序（`DESC`）排列。
> - 排序规则
>   - ASC：升序，默认
>   - `DESC`：降序排列
> - 多列排序
>   - 当第一个列的值相同时，按第二个列的值排序。

### 使用分页查询

```
SELECT * 
FROM employees 
LIMIT 5 OFFSET 10;
```

> - 从第 11 行开始（`OFFSET 10`），返回 5 行数据

















