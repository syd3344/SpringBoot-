## 参数为list

### 基础SQL -- In子句

> - 假设你的 `List` 是一组 `id` 值，SQL 语句可以写成：

```mysql
SELECT * FROM table_name WHERE id IN (1, 2, 3);
```



### Mybatis -- 动态Sql

> - MyBatis 支持通过 `foreach` 标签来迭代一个列表。

```xml
<select id="selectByIds" resultType="yourType">
    SELECT * FROM table_name WHERE id IN
    <foreach collection="idList" item="id" open="(" separator="," close=")">
        #{id}
    </foreach>
</select>
```

#### 解析

> - `collection`: 这里的 `idList` 是你在 Java 代码中传递的 `List`。
> - `item`: `foreach` 会迭代 `idList` 中的每个 `id`，并将其拼接到 `IN` 子句中。
> - `open`, `separator`, `close`: 分别代表左括号、逗号分隔符和右括号。



###  传递 -- ==**@Param**==

> - 在 Java 代码中，通常需要使用 `@Param` 注解来传递 `List`

```java
@Mapper
public interface YourMapper {
    List<YourType> selectByIds(@Param("idList") List<Integer> idList);
}
```





## 输出为list

### 场景

> - 通常出现在一种**一对多**关系中，比如查询一个部门，它有多个员工，或查询一个订单，它有多个商品。
> - 要实现这种情况，通常涉及**嵌套查询**或**关联查询**，并且需要在 MyBatis 中进行特殊处理。



### 具体场景

> - 假设你有两个表：
>   - `order` 表，表示订单信息。
>   - `order_item` 表，表示每个订单包含的商品。
> - 你希望查询一个订单时，`OrderVO` 中包含一个字段 `items`，它是一个 `List<OrderItemVO>`，对应订单的所有商品。



### Vo

```java
public class OrderVO {
    private Integer orderId;
    private String orderName;
    private List<OrderItemVO> items;  // 对应订单中的商品
    // getter and setter
}

public class OrderItemVO {
    private Integer itemId;
    private String itemName;
    // getter and setter
}

```



### 基础SQL写法

```sql
SELECT 
    o.id as orderId, 
    o.name as orderName, 
    (SELECT GROUP_CONCAT(i.id, ':', i.name) FROM order_item i WHERE i.order_id = o.id) AS items 
FROM 
    order o
WHERE 
    o.id = #{orderId};
```

#### 解析

> - 这里使用了 `GROUP_CONCAT` 函数将多个 `order_item` 的结果拼接为一个字符串
> - 然后你可以在 Java 代码中进行拆分处理，转化为 `List<OrderItemVO>`。



### 关联写法

```sql
SELECT 
    o.id as orderId, 
    o.name as orderName, 
    i.id as itemId, 
    i.name as itemName 
FROM 
    order o
LEFT JOIN 
    order_item i ON o.id = i.order_id
WHERE 
    o.id = #{orderId};
```

#### 解析

> - 它返回的是一个**扁平结果集**，并不能直接映射到 `List`
> - 也就是无法直接将 `order_item` 映射为 `OrderVO` 中的 `items` 字段（即 `List<OrderItemVO>`）

这条 SQL 查询会返回如下类似的结果：

| orderId | orderName | itemId | itemName |
| ------- | --------- | ------ | -------- |
| 1       | Order A   | 101    | Item A1  |
| 1       | Order A   | 102    | Item A2  |
| 2       | Order B   | 201    | Item B1  |



### 关联写法的扁平化处理

#### 概述

> - 如果你使用的是 MyBatis 或类似的 ORM 工具，要把查询结果中的订单项 `itemId` 和 `itemName` 转换为 `OrderVO` 中的 `items`（`List<OrderItemVO>`），需要通过 **resultMap** 和 **collection** 标签来处理一对多的结果集。
> - 你需要在 MyBatis 中使用 `resultMap` 来定义这种一对多的映射关系。



```xml
<resultMap id="OrderResultMap" type="OrderVO">
    <id property="orderId" column="orderId" />
    <result property="orderName" column="orderName" />
    <collection property="items" ofType="OrderItemVO">
        <id property="itemId" column="itemId" />
        <result property="itemName" column="itemName" />
    </collection>
</resultMap>

<select id="selectOrderById" resultMap="OrderResultMap">
    SELECT 
        o.id as orderId, 
        o.name as orderName, 
        i.id as itemId, 
        i.name as itemName 
    FROM 
        order o
    LEFT JOIN 
        order_item i ON o.id = i.order_id
    WHERE 
        o.id = #{orderId};
</select>
```



#### 解析

> - **<resultMap>**: 
>   - 定义如何将 SQL 查询结果映射到 `OrderVO` 和 `OrderItemVO` 对象。
> - **<id>**: 
>   - **==映射主键字段==**，这里是 `orderId` 和 `itemId`。
> - **<collection>**: 
>   - 定义 `items` 字段
>   - 它是 `OrderItemVO` 的一个 `List`，每个 `OrderVO` 对应的多个 `OrderItemVO` 会通过这个标签映射
> - **<result>**
>   -  用来定义如何将 SQL 查询结果的某一列映射到 Java 对象中的某个字段
>   - 这里是 `orderName` 和 `itemName`
> - **property**
>   - Java **==对象==**中的字段名称。
> - **column**
>   -  数据库查询**==结果集==**中的列名称。









