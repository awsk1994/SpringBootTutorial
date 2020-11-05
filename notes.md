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

## 1. POM 文件

### 1.1. 父项目

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

### 1.2. 倒入的依赖

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

 - spring-boot-starter-web:
     - "spring-boot-starter"部分：
         - springboot场景启动器；帮助我们倒入web模块正常运行所依赖的组建。
         - https://www.baeldung.com/spring-boot-starters

# 06-springboot-场景启动器

## 2. 主程序类，主入口类
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

 - @SpringBootApplication: Spring Boot 应用标注在某个类上 说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用。


```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication 

...


@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration

...

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration

...

@Import({Registrar.class})
public @interface AutoConfigurationPackage

```

 - @SpringBootConfiguration: Spring Boot 的配置类；
     - 标注在某个类上，表示这是一个SpringBoot 的配置类
 - @Configuration：配置类上来标注这个注解：
     - 配置类 --- 配置文件；配置类也是容器中的额一个组件
 - @EnableAutoConfiguration：开启自动配置功能
     - 以前我们需要配置的东西，SpringBoot 帮我们自动配置；告诉SpringBoot开启自动配置功能
     - @AutoConfigurationPackage: 自动配置包
         - 将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器 [class' package -> auto scan]

TODO: come back in the future and learn this again.


# 07-springboot-场景启动器-使用向导快速创建Spring Boot应用

 - 使用 Spring Initializer -> Choose Web
 - Create Controller

```java
package com.atguigu.springboot01helloworld.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ResponseBody;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

//@ResponseBody // 这个类的所有方法返回的数据直接写给浏览器
//@Controller
@RestController // 可以替换 @Controller 和 @ResponseBody
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        return "hello world quick!";
    }
}
```

 - 去localhost:8080/hello 试试

src/main/resources:
 - static 文件夹: 保存所有的静态资源；js, css, images
 - templates 文件夹：保存所有的模版页面
 - application.properties: 修改SpringBoot自动配置的默认值

 - 改改application.properties
```
server.port=8081
```

 - 从新启动springboot，去localhost:8081/hello (NOT :8080, is not running on 8080 anymore)

# 08-springboot-配置yaml

 - Spring Boot 使用一个全局的配置文件
     - application.properties
     - application.yml

 - 可以使用application.yml 代替 application.properties

 -  YAML以数据为中心，比json, xml等 更适合做配置文件 

 ```yml
server:
  port: 8081
 ```

以前的写法（xml）就是
```xml
<server>
    <port>8081</port>
</server>
```

当我们使用xml，大量的数据都花在标签 (eg. <XXX></XXX>)


