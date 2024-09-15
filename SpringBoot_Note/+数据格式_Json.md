## 概述

> - 一种轻量级的数据交换格式，易于解析，提升小
> - 简洁清晰的层次结构



### 示例

```json
{
    //字符串
  "name": "John",
    
    //数值
  "age": 30,
    
    //布尔值
  "isEmployed": true,
    
    //数组
  "skills": ["Java", "Python", "JavaScript"],
    
    //对象
  "address": {
    "city": "New York",
    "zipCode": "10001"
  },
    
    //null
  "spouse": null
}
```



## 两种基本类型

> - 对象：{　}
> - 数组：［　］



## 常见用途

> - 数据传输
>   - 它通常用于在客户端和服务器之间传递数据，尤其是在 Web 开发中
> - 配置文件
>   - 一些程序使用 JSON 作为配置文件格式，存储应用程序设置等信息。
> - 数据库存储
>   - 一些 NoSQL 数据库（如 MongoDB）使用 JSON 或类似的结构化数据格式来存储数据。



## Java内置Json解析库

```java
import com.fasterxml.jackson.databind.ObjectMapper;

ObjectMapper objectMapper = new ObjectMapper();

// 将JSON字符串解析为Java对象
String jsonString = "{\"name\":\"John\", \"age\":30}";
User user = objectMapper.readValue(jsonString, User.class);

// 将Java对象转换为JSON字符串
String jsonOutput = objectMapper.writeValueAsString(user);
```



## 对象转json

### 引入Gson依赖

```properties
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.9</version>
</dependency>
```



### gson.toJson(　)

```
   //对象转json
    Gson gson = new Gson();
    String jsonString = gson.toJson(user);
```
