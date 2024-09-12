## 概述

> - MyBatis 是一款简化JDBC操作的持久层框架，它简化了 Java 对数据库的访问过程，提供了灵活的 SQL 映射功能。
>
> - 与全自动的 ORM 框架不同，MyBatis 允许开发者直接编写 SQL 语句，同时通过映射配置或者注解将 SQL 与 Java 对象进行绑定，具有高效、灵活的特点。
>   - ORM（Object-Relational Mapping 对象关系映射）
>   - 是一种用于将面向对象编程语言中的对象与关系数据库中的表进行 **自动映射** 的技术
>   - 它通过将数据库中的表、行、列与编程语言中的类、对象和属性进行对应，实现对象与数据库的无缝交互，从而简化数据的持久化操作。

## 特点

> #### 1、灵活的 SQL 操作
>
> - MyBatis 不会强制你使用特定的查询语言，它允许开发者直接编写 SQL 语句。
>
> #### 2、支持动态 SQL
>
> - MyBatis 提供了动态 SQL 的功能，使得你可以在 XML 文件中通过 `if`、`choose` 等条件来动态生成 SQL 语句
>
> #### 3、自动映射
>
> - MyBatis 能将 SQL 查询结果自动映射为 Java 对象。
> - 可以通过 XML 文件或注解来指定 SQL 结果与对象属性之间的映射关系。
>
> #### 4、XML 配置与注解方式并存
>
> - **XML 文件**：传统的配置方式，适合复杂 SQL 映射。
>
> - **注解**：通过注解直接在 Mapper 接口中编写 SQL，适合简单的查询。

## 核心组件

> #### **SqlSession**：
>
> - MyBatis 提供了 `SqlSession` 对象来与数据库进行交互。
> - 通过它可以执行 SQL 查询、插入、更新和删除操作。每次数据库操作都需要通过 `SqlSession` 完成。
>
> #### **Mapper 接口**：
>
> - MyBatis 使用 Mapper 接口作为 SQL 操作的载体，开发者通过定义接口方法，结合注解或者 XML 配置来完成 SQL 与 Java 方法的映射。
> - Mapper 相当于 DAO（数据访问对象）层，直接面向数据库。
>
> #### **配置文件**： 
>
> - MyBatis 的核心配置文件 `mybatis-config.xml` 用于定义数据源、事务管理、SQL 映射文件位置、插件等信息。
> - 每个 Mapper 通常会对应一个 XML 文件或者注解配置来描述 SQL 与 Java 对象的映射关系。
>
> #### **动态 SQL**：
>
> - MyBatis 支持动态 SQL，通过 XML 配置文件中的 `if`、`choose`、`foreach` 等元素，开发者可以根据不同的条件动态生成 SQL 语句，这极大提高了 SQL 的灵活性。

## 工作流程

> ### MyBatis 的工作流程
>
> 1. **加载配置文件**： MyBatis 通过 `SqlSessionFactory` 加载核心配置文件 `mybatis-config.xml`，初始化数据库连接池、映射配置等。
> 2. **创建 SqlSession**： 每次数据库操作时，MyBatis 会通过 `SqlSessionFactory` 创建一个 `SqlSession` 实例。
> 3. **执行 SQL 操作**： 通过 `SqlSession` 执行 SQL 操作，MyBatis 会根据 Mapper 中的 SQL 映射或者注解，执行对应的查询、插入、更新或删除操作。
> 4. **事务管理**： `SqlSession` 支持事务管理，可以手动提交或回滚事务，或者通过 Spring 管理事务。
> 5. **关闭 SqlSession**： 操作完成后，需要关闭 `SqlSession` 来释放资源

















