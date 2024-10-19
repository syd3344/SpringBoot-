## 概述

> - 动态 SQL 是 MyBatis 和 MyBatis-Plus 提供的一种灵活构建 SQL 查询的机制，允许根据条件动态生成不同的 SQL 语句。



## 概览

> - `<if>`：条件语句，用于判断某条件是否为 `true`。
> - `<choose>`：类似于 Java 的 `switch-case` 语句。
> - `<foreach>`：循环遍历集合生成 SQL。
> - `<trim>`：自动添加或删除多余的符号，比如 `AND`、`OR`。
> - `<where>`：自动处理条件前面的 `WHERE` 关键字，避免拼接多余的 `AND` 或 `OR`。
> - `<set>`：用于更新语句中的 `SET` 子句，自动处理逗号。



## < if >

> - 标签用于根据给定条件动态生成 SQL 片段。只有当条件为 `true` 时，相关 SQL 语句才会被添加。

> - 根据 `name`、`age` 和 `gender` 的值动态生成 `WHERE` 子句。
> - 如果对应的值不为空，则相应的条件会被添加到查询中。

```xml
<mapper namespace="com.example.mapper.UserMapper">

    <select id="selectUsersByCondition" resultType="com.example.entity.User">
        SELECT * FROM user
        <where>
            <if test="name != null and name != ''">
                AND name = #{name}
            </if>
            <if test="age != null">
                AND age = #{age}
            </if>
            <if test="gender != null">
                AND gender = #{gender}
            </if>
        </where>
    </select>

</mapper>
```



## < choose >

> - 用于根据条件选择性地执行 SQL 语句，类似于 Java 的 `switch` 语句。

> - SQL 查询会根据不同的条件选择执行。
>   - 如果 `name` 不为空，则使用 `name` 进行过滤；
>   - 如果 `age` 不为空，则使用 `age` 进行过滤；
>   - 如果两者都为空，则使用默认的性别为 `'unknown'`

```xml
<mapper namespace="com.example.mapper.UserMapper">

    <select id="selectUserByCondition" resultType="com.example.entity.User">
        SELECT * FROM user
        <where>
            <choose>
                <when test="name != null and name != ''">
                    AND name = #{name}
                </when>
                <when test="age != null">
                    AND age = #{age}
                </when>
                <otherwise>
                    AND gender = 'unknown'
                </otherwise>
            </choose>
        </where>
    </select>

</mapper>
```



## < foreach >

> - 用于遍历集合，并动态生成 `IN` 查询等 SQL 片段

> - `idList` 是一个传入的集合。`<foreach>` 标签会遍历这个集合，并生成一个包含多个 `id` 的 `IN` 查询。

> - 例如，如果传入的 `idList` 为 `[1, 2, 3]`，生成的 SQL 为：
> - ![1727981547699](Mybatis_动态Sql.assets/1727981547699.png)

```xml
<mapper namespace="com.example.mapper.UserMapper">

    <select id="selectUsersByIds" resultType="com.example.entity.User">
        SELECT * FROM user WHERE id IN
        <foreach item="id" collection="idList" open="(" separator="," close=")">
            #{id}
        </foreach>
    </select>

</mapper>
```



## < trim >

> - 用于处理 SQL 的前缀和后缀，自动删除多余的符号（如 `AND`、`OR`）

> - `<trim>` 标签可以处理前缀和后缀的添加。`prefix="WHERE"` 指定在生成的 SQL 前添加 `WHERE` 关键字
> -  `prefixOverrides` 则自动删除多余的 `AND` 或 `OR`。如果没有条件成立，`WHERE` 关键字不会出现在 SQL 中

```xml
<mapper namespace="com.example.mapper.UserMapper">

    <select id="selectUsersWithTrim" resultType="com.example.entity.User">
        SELECT * FROM user
        <trim prefix="WHERE" prefixOverrides="AND | OR">
            <if test="name != null and name != ''">
                AND name = #{name}
            </if>
            <if test="age != null">
                AND age = #{age}
            </if>
        </trim>
    </select>

</mapper>
```



## < where >

> - 它会自动添加 `WHERE` 关键字并去除多余的 `AND` 或 `OR`。

```xml
<mapper namespace="com.example.mapper.UserMapper">

    <select id="selectUsersWithWhere" resultType="com.example.entity.User">
        SELECT * FROM user
        <where>
            <if test="name != null and name != ''">
                AND name = #{name}
            </if>
            <if test="age != null">
                AND age = #{age}
            </if>
        </where>
    </select>

</mapper>
```



## < set >

> - 动态生成 SQL 语句中的 `SET` 子句，通常在执行更新操作时使用。

> - 更新用户的 `name`、`age` 和 `email` 字段，具体取决于提供的参数。

```xml
<mapper namespace="com.example.mapper.UserMapper">

    <update id="updateUser">
        UPDATE user
        <set>
            <if test="name != null and name != ''">
                name = #{name},
            </if>
            <if test="age != null">
                age = #{age},
            </if>
            <if test="email != null and email != ''">
                email = #{email},
            </if>
        </set>
        WHERE id = #{id}
    </update>

</mapper>
```





