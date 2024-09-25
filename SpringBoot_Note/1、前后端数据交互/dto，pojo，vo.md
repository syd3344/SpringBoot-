## 概述

> - DTO`（数据传输对象）、`POJO`（简单 Java 对象）和 `VO（视图对象）



## DTO -- （Data Transfer Object）  --  Request

> - 接收前端 json 对象
> - 一种用于传输数据的对象，专注于数据传输



## POJO --  （Plain Old Java Object）  --  Mapper

> - 和数据库对接
>   - 字段名要和属性值一致
> - 主要用于定义数据模型或实体类
> - 通常只包含字段（属性）和 getter/setter 方法



## VO -- （View Object）  --  Responce

> - 表示前端需要的数据显示形式
> - 可以封装用于前端展示的各种数据。



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

