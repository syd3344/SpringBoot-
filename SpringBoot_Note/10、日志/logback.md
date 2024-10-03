## 概述

> - 是一个基于 Java 的日志框架，由 `Log4j` 的作者 Ceki Gülcü 创建，旨在替代 Log4j
> - **Log4j** 是 Java 中最早被广泛使用的日志框架之一，但它存在性能、配置复杂度等方面的不足。
> - 为了改进这些问题，Ceki Gülcü 创建了 **SLF4J**（Simple Logging Facade for Java），作为一个通用的日志门面，允许开发者在编写代码时不依赖具体的日志实现。
> - **Logback** 是 SLF4J 的默认实现，它在性能、功能和灵活性方面进行了优化，现被广泛用于 Java 企业级应用。



## 组成

> - **logback-core**：Logback 的基础模块，提供日志系统的核心组件。
> - **logback-classic**：用于实现与 SLF4J 的无缝集成，兼容 SLF4J 的日志接口。
> - **logback-access**：主要用于与 Servlet 容器（如 Tomcat）集成，管理 HTTP 请求的日志输出。



## 依赖

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.30</version>
</dependency>
```



## XML配置

### 位置

> - 通常情况下，`logback.xml` 放在 **src/main/resources** 目录下。
> - Spring Boot 也可以使用 **logback-spring.xml**，它支持更多的 Spring 特性，比如占位符和 profile。

### 加载顺序

> - **Spring Boot** 默认会自动加载 `logback-spring.xml` 或 `logback.xml`，不需要手动指定路径。
> - 如果同时存在两个文件，Spring Boot 优先使用 `logback-spring.xml`。

### 属性

#### Appender

> - 定义日志输出的位置

> - name="FILE"，FILE是一个自定义的名称，可自定义
> - class = "ch.qos.logback.core.FileAppender"，这是logback提供的Appender类，固定书写
>   - ConsoleAppender
>     - ch.qos.logback.core.ConsoleAppender
>     - 将日志输出到控制台
>   - FileAppender
>     - ch.qos.logback.core.FileAppender
>     - 将日志输出到指定文件
>   - RollingFileAppender
>     - ch.qos.logback.core.rolling.RollingFileAppender
>     - 将日志输出到文件，并且当文件达到一定大小或者时间间隔时，创建一个新的日志文件。
>   - TimeBasedRollingPolicy
>     - ch.qos.logback.core.rolling.TimeBasedRollingPolicy
>     - 与 `RollingFileAppender` 一起使用，允许基于时间滚动日志文件
>   - SizeAndTimeBasedRollingPolicy
>     - ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy
>     - 基于文件大小和时间滚动日志文件
>   - AsyncAppender
>     - ch.qos.logback.classic.AsyncAppender
>     - 异步 Appender，能够异步地将日志事件输出到指定的 appender，避免在日志输出时阻塞应用程序。
>   - SMTPAppender
>     - ch.qos.logback.classic.net.SMTPAppender
>     - 用于将日志通过电子邮件发送。典型的应用场景是发送错误级别的日志到指定的邮件地址。
>   - SocketAppender
>     - ch.qos.logback.classic.net.SocketAppender
>     - 用于通过网络将日志事件发送到远程服务器，适用于分布式系统或集中日志处理的场景。
>   - DBAppender
>     - ch.qos.logback.classic.db.DBAppender
>     - 用于将日志事件保存到数据库中。可以将日志直接插入到数据库表中，适合于需要持久化存储日志的场景。

#### encoder

> - 定义日志的输出格式

> - pattern
>   - %d
>     - 表示日期
>     - {yyyy-MM-dd HH:mm:ss} 
>     - 输出日志的时间戳，格式为 `yyyy-MM-dd HH:mm:ss`。
>   - [%thread] 
>     - 表示线程名
>   - %-5level
>     - %level 表示日志级别 
>     - -5表示左对齐且最少占5个字符
>   - %logger{36}
>     - 输出产生日志的 `logger` 名称
>     - {36} 表示最多显示 36 个字符
>   - -%msg
>     - 输出日志的消息内容
>   - %n
>     - 换行

#### root

> - 定义日志的根级别
> - 所有没有被单独配置的日志器都会使用这个配置

> - `level` 
>   - 控制日志的最低输出级别
> - appender
>   - appender-ref ref="STDOUT" 
>   - 指定将日志输出到名为 `STDOUT` 的 appender



### 输出到控制台

```xml
<configuration>

    <!-- Appender 定义：定义日志输出目的地，如控制台、文件等 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 设置日志级别：日志根目录的级别可以是 TRACE, DEBUG, INFO, WARN, ERROR -->
    <root level="INFO">
        <appender-ref ref="STDOUT" />
    </root>

</configuration>
```



### 输出到文件

> - **file**：指定日志文件的路径。
> - **append**：设置为 `true` 时，日志会追加到现有的日志文件末尾；设置为 `false` 时会覆盖现有的日志文件。

```xml
<appender name="FILE" class="ch.qos.logback.core.FileAppender">
    <file>app.log</file>
    <append>true</append>
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>

<root level="INFO">
    <appender-ref ref="FILE" />
</root>
```



## 日志文件切割

> - 在生产环境中，日志文件的大小会不断增长，因此可以使用 `RollingFileAppender` 来定期切割日志文件
> - 通过设置**日志滚动策略**，可以限制日志文件的大小或数量，防止单个日志文件无限增大，影响性能和管理
> - Logback 提供了灵活的日志文件切割机制，主要包括 **按大小切割** 和 **按时间切割**。

### 按文件大小切割

> - 通过   **SizeBasedTriggeringPolicy**   来实现，当日志文件达到设定的大小时，会自动生成一个新的日志文件。
>   - **fileNamePattern**：日志文件的命名格式，其中 `%d{yyyy-MM-dd}` 表示日期，`%i` 表示文件索引号。生成的文件可能会是 `app.2024-09-30.0.log`、`app.2024-09-30.1.log` 等
>   - **maxFileSize**：设置单个日志文件的最大大小（如 10MB），当文件大小超过这个限制时，日志将被写入新的文件。
>   - **maxHistory**：日志文件保留的天数，超过此时间的旧日志文件将被删除。

```xml
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <!-- 指定日志文件的输出路径 -->
    <file>app.log</file>
    
    <!-- 配置文件滚动策略 -->
    <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
        <!-- 日志文件名格式，使用日期和索引号 -->
        <fileNamePattern>app.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
        
        <!-- 设置文件大小的阈值，达到此大小后创建新文件 -->
        <maxFileSize>10MB</maxFileSize>
        
        <!-- 保留最多30天的日志 -->
        <maxHistory>30</maxHistory>
    </rollingPolicy>
    
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>
```



### 按时间切割文件

> - 通过   **TimeBasedRollingPolicy**  来实现按时间切割日志文件，常见的是每天生成一个新的日志文件。
>   - **fileNamePattern**：定义日志文件按日期命名，每天生成一个新的日志文件，如 `app.2024-09-30.log`。
>   - **maxHistory**：保留最近 30 天的日志文件，超过这个时间的日志文件会被删除。

```xml
<appender name="DAILY_FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>daily_app.log</file>
    
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <!-- 按日期进行日志文件切割 -->
        <fileNamePattern>app.%d{yyyy-MM-dd}.log</fileNamePattern>
        
        <!-- 保留最近30天的日志 -->
        <maxHistory>30</maxHistory>
    </rollingPolicy>
    
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>
```



## 按时间和大小共同切割日志文件

> - 可以使用 `SizeAndTimeBasedRollingPolicy`，这样在日志文件达到某个大小时或每天都会创建一个新的文件。

```xml
<appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>app.log</file>
    
    <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
        <!-- 每天生成一个新日志文件，同时按大小滚动 -->
        <fileNamePattern>app.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
        
        <!-- 文件最大 10MB -->
        <maxFileSize>10MB</maxFileSize>
        
        <!-- 日志最多保留 30 天 -->
        <maxHistory>30</maxHistory>
        
        <!-- 日志文件总大小不能超过 3GB -->
        <totalSizeCap>3GB</totalSizeCap>
    </rollingPolicy>
    
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
</appender>
```



## 配置滚动日志文件的压缩

> - 日志切割后可以将旧日志文件压缩以节省空间。通过 `fileNamePattern` 属性的后缀设置为 `.gz` 或 `.zip`，Logback 会自动压缩切割后的日志文件。

```xml
<fileNamePattern>app.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern>
```



## 异步日志写入

> - Logback 提供了 `AsyncAppender` 来异步写入日志。
> - 日志的写入操作会在后台线程异步进行，而不会阻塞应用的主线程。

```xml
<appender name="ASYNC_FILE" class="ch.qos.logback.classic.AsyncAppender">
    <appender-ref ref="FILE"/>
</appender>
```



## 日志级别控制

> - Logback 可以为不同的包或类单独设置日志级别



 	



















