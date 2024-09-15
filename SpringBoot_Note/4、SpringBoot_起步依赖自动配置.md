## 起步依赖（Starter Dependencies）

### 概述

> - 起步依赖是 Spring Boot 提供的一种便捷方式，通过包含一个依赖，自动引入该项目所需的所有依赖。这让开发者不必手动一个个地添加特定的库。
>
> - 本质就是一个maven坐标（maven项目对象模型），整合了一个功能需要的所有坐标（传递依赖）

### 特点

> - **预定义的依赖集合**：每个起步依赖都包含了一组与某种功能相关的库（例如：Web、数据访问、安全等）。
> - **减少配置工作**：通过添加一个起步依赖，自动引入所有必要的依赖，减少了开发者管理依赖的工作量。
> - **集中管理版本**：Spring Boot 的起步依赖会根据项目的 Spring Boot 版本，自动管理依赖的版本，确保兼容性。

### 常见起步依赖

> - `spring-boot-starter-web`：包含构建 Web 应用的必要依赖，如 Spring MVC、嵌入式 Tomcat。
> - `spring-boot-starter-data-jpa`：包含 JPA、Hibernate、数据库驱动等构建数据访问层的依赖。
> - `spring-boot-starter-test`：包含测试相关的依赖，如 JUnit、Mockito、Spring Test。



## 自动配置

### 概述

> Spring Boot 的自动配置机制是通过分析项目的类路径、配置和 Bean 定义，自动为项目提供合适的 Spring Bean 和配置。这大大减少了开发者手动配置 Bean 的工作量。

### 原理

> - **条件注解**：Spring Boot 使用一系列 `@Conditional` 注解来实现自动配置。例如，只有当某个类存在于类路径时，相关的配置才会生效。
> - **类路径检测**：Spring Boot 会检测项目中的依赖。例如，如果引入了 `spring-boot-starter-web`，它会自动配置 Tomcat、Spring MVC 等组件。
> - **默认配置**：Spring Boot 提供了一些默认的配置，但允许开发者通过配置文件（`application.properties` 或 `application.yml`）或者自定义 Bean 来覆盖这些默认配置。

### 示例

> 当你引入 `spring-boot-starter-web` 依赖时，Spring Boot 会自动为你配置以下组件：
>
> - 嵌入式 Tomcat 服务器。
> - Spring MVC 相关的 Bean（如 `DispatcherServlet`、`RequestMappingHandlerAdapter`）。
> - Jackson 用于 JSON 数据的序列化和反序列化。



## 手动调整自动配置

> 虽然自动配置为开发者提供了便利，但有时需要根据项目需求进行自定义配置。可以通过以下方式手动调整自动配置：
>
> - **配置文件**：通过 `application.properties` 或 `application.yml` 来调整默认配置。
> - **自定义 Bean**：可以在项目中定义自己的 `@Bean` 来覆盖 Spring Boot 的默认配置。
> - **禁用自动配置**：可以使用 `@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})` 来禁用某个特定的自动配置。



##  起步依赖和自动配置的结合

> - Spring Boot 的起步依赖和自动配置紧密结合，起步依赖引入了相关的依赖，自动配置则根据这些依赖为你自动配置合适的 Spring 组件。
>
> - 例如，当你引入 `spring-boot-starter-data-jpa` 时，Spring Boot 会：
>   - 检测项目是否有 JPA 相关依赖（如 Hibernate）。
>   - 如果有，会自动配置一个 `EntityManager` 和 `TransactionManager`。
>   - 如果类路径中有数据库驱动，Spring Boot 会为你自动配置数据源。

