## 概述

> - `@Value` 注解是 Spring 中用于将配置文件中的值注入到 Java 类中的常用方式。它适用于多种配置文件类型



## Properties

**application.properties**

```properties
server.port=8080
app.name=MyApp
app.description=This is a Spring Boot application.

```

**@value注入**

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class AppConfig {
    
    @Value("${server.port}")
    private int serverPort;
    
    @Value("${app.name}")
    private String appName;

    @Value("${app.description}")
    private String appDescription;

    // Getters and setters

    public void printConfig() {
        System.out.println("Server Port: " + serverPort);
        System.out.println("App Name: " + appName);
        System.out.println("App Description: " + appDescription);
    }
}

```



## 自定义properties 

**自定义 config.properties**

```properties
app.version=1.0.0
app.author=John Doe

```

**@PropertySource 和 @value注入**

```properties
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@Configuration
@PropertySource("classpath:config.properties")
public class CustomConfig {

    @Value("${app.version}")
    private String appVersion;

    @Value("${app.author}")
    private String appAuthor;

    // Getters and setters

    public void printConfig() {
        System.out.println("App Version: " + appVersion);
        System.out.println("App Author: " + appAuthor);
    }
}

```



## Yml

**application.yml**

```yml
server:
  port: 8080

app:
  name: MyApp
  description: This is a Spring Boot application.

```

**@Value注入**  -- 注入的路径会通过  **`.`**  分隔来表示层级关系。  

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class AppConfig {
    
    @Value("${server.port}")
    private int serverPort;
    
    @Value("${app.name}")
    private String appName;

    @Value("${app.description}")
    private String appDescription;

    // Getters and setters

    public void printConfig() {
        System.out.println("Server Port: " + serverPort);
        System.out.println("App Name: " + appName);
        System.out.println("App Description: " + appDescription);
    }
}

```

















































