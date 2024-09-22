## 示例1

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/POM/4.0.0">
    
<!--  project 是文件的根节点-->
    <modelVersion>4.0.0</modelVersion>
<!--  对象模型版本4.0.0没变过-->
    <groupId>com.example</groupId>
<!--    项目名称-->
    <artifactId>demo</artifactId>
<!--    模块名称-->
    <version>1.0-SNAPSHOT</version>
<!--    产品的版本号-->


    <properties>
        <java.version>11</java.version>
        <spring-boot.version>2.7.6</spring-boot.version> <!-- 确保使用兼容的版本 -->
    </properties>

    <!--    
			使用 <properties> 定义的属性是 Maven 内部的属性，可以用于配置各种插件、依赖项等。
            显示指定JDK版本和Spring Boot版本
             在${spring-boot.version} 和 ${java.version}处使用
    -->

    <!--  
			在 pom.xml 中通过 <properties> 显式指定 Java 版本，与在外部的 .properties 文件中指定的效果基本一致，但两者的使用场景和加载方式有所不同。
            .properties 文件主要用于 Java 应用程序的运行时配置，不会自动影响 Maven 构建。
			除非你显式地在 pom.xml 中配置插件读取 .properties 文件的内容，它不会影响编译器插件或构建行为。
            在 pom.xml 中使用 <properties>：这是影响 Maven 构建的推荐方式，尤其是控制 Java 版本等编译设置。
            在 .properties 文件中使用：主要用于应用程序的外部配置，不影响 Maven 的构建行为，除非你明确配置了插件去读取它。
    -->


    <dependencyManagement>
<!--
            是一个“依赖版本的管理者”。它帮助你统一和管理项目中的所有依赖版本，但它不会直接决定项目里哪些依赖会被使用。
            它不会直接添加依赖到项目中，而是用于声明所有子模块（如果有的话）可以使用的依赖版本。

            统一版本:
                你可以在一个地方（通常是父 POM 文件）定义所有常用依赖的版本，这样项目中的所有子模块（如果有的话）就能统一使用这些版本。这就像是一个中心仓库，记录了所有依赖的版本。

            不直接添加依赖:
                只在 <dependencyManagement> 中声明依赖和版本，但它不会直接把这些依赖加入到你的项目中。
				实际的依赖还是需要在子模块的 POM 文件中声明，这些子模块会自动使用 <dependencyManagement> 中定义的版本。

            减少重复:
                你不需要在每个子模块中都写一遍版本号，减少了重复劳动和版本不一致的风险。
-->

        <dependencies>
<!--            
				这个标签包含了所有在 dependencyManagement 中定义的依赖项。注意，这里声明的依赖项并不会被自动添加到项目的类路径中。
				它们只是定义了依赖版本，使得项目中的其他模块可以使用这些版本。
-->

            <dependency>
<!--                这是一个依赖项的定义。-->

                <groupId>org.springframework.boot</groupId>
<!--                这个标签指定了依赖的组 ID。在这个例子中，org.springframework.boot 是 Spring Boot 的组 ID。-->

                <artifactId>spring-boot-dependencies</artifactId>
<!--                这个标签指定了依赖的构件 ID。spring-boot-dependencies 是一个包含了 Spring Boot 所有相关依赖及其版本的 POM 文件。-->

                <version>${spring-boot.version}</version>
<!--                这个标签指定了依赖的版本号。${spring-boot.version} 是一个 Maven 属性，它的值在其他地方（通常是在 properties 标签中）定义。-->

                <type>pom</type>
<!--                这个标签指定了依赖的类型。在这里，pom 类型表示这是一个 POM 文件，通常用于导入和管理其他依赖项的版本。-->

                <scope>import</scope>
<!--                这个标签指定了依赖的作用范围。import 范围用于导入 POM 文件中的依赖管理部分。-->

            </dependency>
        </dependencies>
    </dependencyManagement>


    <dependencies>

<!--   启动器     -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

<!--    起步Web依赖    -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

<!--    起步test依赖    -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

<!--    lombok    -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.24</version>
            <scope>provided</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>

<!--      在这里，用于处理代码编译任务。      -->
<!--            是 Maven 官方提供的插件之一，主要用于处理项目的编译任务。它负责将 Java 源代码编译为字节码（.class 文件），并允许你自定义编译器相关的配置，如 Java 版本、编译选项、源代码路径等。-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>

<!--     这是 Spring Boot 的 Maven 插件，用于处理与 Spring Boot 项目相关的任务，如运行、打包成可执行 JAR 文件等。它允许你通过 Maven 构建和运行 Spring Boot 应用程序。 -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



## 示例2

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.6</version>
        <relativePath/>
    </parent>
<!--它为常用的 Spring Boot 依赖（如 spring-boot-starter-web、spring-boot-starter-data-jpa 等）提供了默认版本，这样你不需要在子模块中显式地指定这些依赖的版本，确保了一致性和兼容性。-->
<!--
		spring-boot-starter-parent 还为常用的 Maven 插件（如 maven-compiler-plugin、maven-surefire-plugin）提供了默认的配置。
		这简化了插件的配置，并确保你使用的插件版本是与 Spring Boot 兼容的版本。
-->

    <groupId>com.example</groupId>
    <artifactId>SpringBoot2</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>SpringBoot2</name>
    <description>Demo project for Spring Boot</description>

<!--配置jdk版本-->
    <properties>
        <java.version>11</java.version>
    </properties>


<!--依赖配置-->
    <dependencies>
<!--        web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

<!--        lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

<!--        test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>


<!--        dom4j-->
        <dependency>
            <groupId>org.dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>2.1.3</version>
        </dependency>
    </dependencies>


<!--    maven配置-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>

                <configuration>
<!--                    在 Spring Boot 项目的构建中，spring-boot-maven-plugin 插件被用来打包和运行应用。
                        默认情况下，这个插件会将所有的依赖项打包到最终的 JAR 文件中。
                        然而，有时候你可能希望排除某些依赖项，尤其是那些在运行时不需要的库，比如 lombok。
                        lombok 是一个编译时库，不需要被打包到运行时 JAR 中。-->
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```



