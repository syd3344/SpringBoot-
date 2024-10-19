## JSON库

> - **fastjson**（阿里巴巴的库）
> - **Hutool**



## JSON字符串转JSONObject对象

### fastjson

#### 单层解析

> - 将 JSON 字符串解析为 JSONObject 对象
> - JSONObject对象本质是一个Map

```Java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;

public class JsonParseExample {
    public static void main(String[] args) {
        
        // 假设这是一个 JSON 字符串
        String jsonString = "{ \"name\": \"John\", \"age\": 30, \"city\": \"New York\" }";
        
        // 将 JSON 字符串解析为 JSONObject
        JSONObject jsonObject = JSON.parseObject(jsonString);
        
        // 获取 JSON 对象中的值
        String name = jsonObject.getString("name");
        int age = jsonObject.getIntValue("age");
        String city = jsonObject.getString("city");

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("City: " + city);
    }
}
```

#### 嵌套解析

> - 对于嵌套的 JSON 数据，可以通过嵌套的 `JSONObject` 来访问：

```java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;

public class JsonParseNestedExample {
    public static void main(String[] args) {
        // 带有嵌套 JSON 对象的 JSON 字符串
        String jsonString = "{ \"person\": { \"name\": \"John\", \"age\": 30 }, \"city\": \"New York\" }";
        
        // 解析 JSON 字符串
        JSONObject jsonObject = JSON.parseObject(jsonString);
        
        // 获取嵌套的 JSON 对象
        JSONObject person = jsonObject.getJSONObject("person");
        
        // 从嵌套对象中获取值
        String name = person.getString("name");
        int age = person.getIntValue("age");
        String city = jsonObject.getString("city");

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("City: " + city);
    }
}
```



### Hutool

#### 解析JSON字符串为JSONObject

```java
import cn.hutool.json.JSONObject;
import cn.hutool.json.JSONUtil;

public class Main {
    public static void main(String[] args) {
        String jsonStr = "{\"name\":\"John\",\"age\":25}";
        // 将 JSON 字符串解析为 JSONObject
        JSONObject jsonObject = JSONUtil.parseObj(jsonStr);
        
        // 访问具体字段
        String name = jsonObject.getStr("name");
        int age = jsonObject.getInt("age");
        
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
    }
}
```

### ==**获取深层次的值**==

```java
public class Main {
    public static void main(String[] args) {
        String jsonStr = "{\"name\":\"John\",\"address\":{\"city\":\"New York\",\"zip\":\"10001\"}}";
        JSONObject jsonObject = JSONUtil.parseObj(jsonStr);
        
        // 获取深层次的字段值
        String city = jsonObject.getByPath("address.city", String.class);
        System.out.println("City: " + city);
        
        // City: New York
    }
}
```



#### 解析包含多个对象的 JSON 字符串为 JSONArray

```java
import cn.hutool.json.JSONArray;
import cn.hutool.json.JSONUtil;

public class Main {
    public static void main(String[] args) {
        String jsonArrayStr = "[{\"name\":\"John\",\"age\":25},{\"name\":\"Jane\",\"age\":30}]";
        // 将 JSON 字符串解析为 JSONArray
        JSONArray jsonArray = JSONUtil.parseArray(jsonArrayStr);
        
        // 遍历 JSON 数组
        for (int i = 0; i < jsonArray.size(); i++) {
            System.out.println(jsonArray.getJSONObject(i).getStr("name"));
            System.out.println(jsonArray.getJSONObject(i).getInt("age"));
        }
    }
}
```

#### 解析字符串为java对象

```java
import cn.hutool.json.JSONUtil;

public class Main {
    public static void main(String[] args) {
        String jsonStr = "{\"name\":\"John\",\"age\":25}";
        // 将 JSON 字符串解析为 Java 对象
        Person person = JSONUtil.toBean(jsonStr, Person.class);
        
        System.out.println("Name: " + person.getName());
        System.out.println("Age: " + person.getAge());
    }
}

class Person {
    private String name;
    private int age;

    // Getters and setters
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

#### 解析字符串为map对象

```java
import cn.hutool.json.JSONUtil;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        String jsonStr = "{\"name\":\"John\",\"age\":25}";
        // 将 JSON 转换为 Map
        Map<String, Object> map = JSONUtil.toBean(jsonStr, Map.class);
        
        System.out.println(map);
        // {name=John, age=25}
    }
}
```



## 对象转Json

```java
        // 1. 创建参数对象，准备要发送的数据
        Map<String, Object> paramMap = new HashMap<>();
        paramMap.put("name", "John");
        paramMap.put("age", 25);

        // 2. 将参数对象转化为JSON字符串
        String jsonString = JSONUtil.toJsonStr(paramMap);

        // 3. 发送POST请求，传递JSON格式数据
        String response = HttpUtil.post("https://example.com/api", jsonString);
```



```java
import cn.hutool.json.JSONUtil;

public class Main {
    public static void main(String[] args) {
        Person person = new Person("John", 25);
        // 将对象转换为 JSON 字符串
        String jsonStr = JSONUtil.toJsonStr(person);
        System.out.println(jsonStr);
    }
}

class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```



### Java 对象列表转换为 JSON 数组字符串

```java
ublic class Main {
    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();
        people.add(new Person("John", 25));
        people.add(new Person("Jane", 30));

        // 将 Java 对象列表转换为 JSON 数组字符串
        String jsonArrayStr = JSONUtil.toJsonStr(people);
        System.out.println(jsonArrayStr);
    }
}

class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```



```json
[{"name":"John","age":25},{"name":"Jane","age":30}]
```



### 格式化输出

```java
import cn.hutool.json.JSONUtil;

public class Main {
    public static void main(String[] args) {
        String jsonStr = "{\"name\":\"John\",\"age\":25}";
        // 格式化输出 JSON 字符串
        String prettyJson = JSONUtil.formatJsonStr(jsonStr);
        System.out.println(prettyJson);
    }
}
```



```json
{
    "name": "John",
    "age": 25
}
```



## json字符串转成指定对象

JsonUtil.toBean

























