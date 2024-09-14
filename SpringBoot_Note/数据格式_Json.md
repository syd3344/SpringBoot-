











## 对象转json

### 引入依赖

```properties
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.9</version>
</dependency>
```



### 实现

```
   //对象转json
    Gson gson = new Gson();
    String jsonString = gson.toJson(user);
```

