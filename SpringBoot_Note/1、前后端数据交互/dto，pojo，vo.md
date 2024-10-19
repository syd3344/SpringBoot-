## 概述

> - DTO`（数据传输对象）、`POJO`（简单 Java 对象）和 `VO（视图对象）



## DTO -- （Data Transfer Object）  --  Request

> - 接收前端 json 对象
> - 一种用于传输数据的对象，专注于数据传输

### 如何创建一个DTO

> 查看所有前端请求，将所有json对象字段融合



## POJO --  （Plain Old Java Object）  --  Mapper

> - 和数据库对接
>   - 字段名要和属性值一致
> - 主要用于定义数据模型或实体类
> - 通常只包含字段（属性）和 getter/setter 方法



## VO -- （View Object）  --  Responce

### 概述

> - 表示前端需要的数据显示形式
> - 可以封装用于前端展示的各种数据。

### 如何创建一个VO

> - **确定数据需求**：
>   - 首先，你需要了解当前**==页面需要哪些数据==**
>   - 通过查看当前页面的所有响应请求，**==确定需要汇总的数据字段==**
>     - 返回给前端的数据**==可以包含冗余字段==**，前端可以根据实际需要选择所需的字段。
> - **定义 VO 类**：
>   - 根据你收集到的数据，创建一个 Java 类来表示这些数据。
>   - 这个类应当包含必要的字段和**==相应的 getter 和 setter 方法==**
> - **填充 VO 数据**：
>   - 在服务层或控制器中，将获取到的数据填充到 VO 对象中。
>   - 可以通过调用其他服务或者直接从数据库中获取所需的数据。
> - **返回 VO 对象**：
>   - 最后，将填充好的 VO 对象作为响应返回给前端。





## 三者转换 

### 导入hutool依赖

```xml
 <!--hutool工具包-->
 <dependency>
 	<groupId>cn.hutool</groupId>
 	<artifactId>hutool-all</artifactId>
	 <version>${hutool.version}</version>
 </dependency>
```



### 具体转换方法 -- toBean

```java
 public static <T> T toBean(Object source, Class<T> clazz) {
        return toBean((Object)source, (Class)clazz, (CopyOptions)null);
    }
```



### 示例

```java
 //对象拷贝-从dto传输对象转成po持久化对象
 Bed bed = BeanUtil.toBean(bedDto, Bed.class);
```

