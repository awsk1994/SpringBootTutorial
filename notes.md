# 04 - springboot-helloworld
 - start project (maven)
 - import spring boot start-web project

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.atguigu</groupId>
    <artifactId>spring-boot-01-helloworld</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

 - Create main method
     - create class -> "com.atguigu.HelloWorldMainApplication" (this will automatically create the package too)
```java
package com.atguigu;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        // Start Spring Application.
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}
```

 - Create controller
 ```java
 package com.atguigu.controller;
 
 import org.springframework.stereotype.Controller;
 import org.springframework.web.bind.annotation.RequestMapping;
 import org.springframework.web.bind.annotation.ResponseBody;
 
 @Controller
 public class HelloController {
     @ResponseBody
     @RequestMapping("/hello") // 接手浏览器的hello请求
     public String hello(){
         return "Hello World";
     }
 }
 ```
 
- 测试
     - 执行 HelloWorldMainApplication (right-click + run)
     - 发现会启动Tomcat，在port8080 执行
     
```
2020-11-04... o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) ...
```

     - 在浏览器，输入 localhost:8080/hello，会得到：
    
```
Hello World
```
 - 如果是以前，我们要把应用打成war包，放进tomcat服务器上。

 - 创建可执行的war包
```
<!--这个插件，可以将应用打包成一个可执行的jar包-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

 - "Maven Projects"(IntelliJ 右边）-> spring-boot-01-helloworld -> Lifecycle -> package

```
Building jar: /Users/alexwong/GitHub/SpringBootTutorial/target/spring-boot-01-helloworld-1.0-SNAPSHOT.jar
```


 - 开terminal，执行
     - Tomcat 已在jar里面
```
java -jar spring-boot-01-helloworld-1.0-SNAPSHOT.jar
```
# 05-springboot-场景启动器

## POM 文件
### 1. 父项目
他(org.springframework.boot) 的父项目是spring-boot-starter-parent
```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.5.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

(cmd + click spring-boot-starter-parent)

spring-boot-starter-parent 的父项目是 spring-boot-dependencies

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.5.RELEASE</version>
</parent>
```

他来真正管理Spring Boot 应用里面的所有依赖版本。

## 倒入的依赖

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

