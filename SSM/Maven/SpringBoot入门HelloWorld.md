# SpringBoot入门HelloWorld

## 一、 SpringBoot简介

## 二、 SpringBoot HelloWorld

​		实现一个最基础的功能：浏览器发送一个hello请求，服务器接收请求并处理，响应Hello World字符串

​		可以跟随<a href="https://spring.io/quickstart">官方网站演示教程</a>完成一次SpringBoot的Hello World应用入门

### 1. 创建一个Maven工程（不需要任何模板）

可以在Spring的官网直接生成一个演示工程--<a href="https://start.spring.io/">官方演示项目生成网址</a>

### 2. 导入SpringBoot相关的依赖

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.3.1.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.Lany</groupId>
	<artifactId>SpringBootTest</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>SpringBootTest</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

### 3.编写一个主程序入口；启动SpringBoot应用

```java
package com.Lany.SpringBootTest;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * SpringBootApplication 来标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

}
```

### 4.编写相关的Controller、Service

注意：Controller要在启动类所在的目录下

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {
        return String.format("Hello %s!", name);
    }

}
```

### 5.运行主程序测试

![image-20200616183757989](D:\笔记\SpringBoot入门HelloWorld.assets\image-20200616183757989-1592305309767.png)

### 6.简化部署

在Maven中对项目进行打包，将会产生一个jar包，这个jar包可以在服务器上使用java -jar的命令直接运行

![image-20200616183103255](D:\笔记\SpringBoot入门HelloWorld.assets\image-20200616183103255.png)

然后在生成的target目录中会有一个jar包生成

![image-20200616183554668](D:\笔记\SpringBoot入门HelloWorld.assets\image-20200616183554668.png)

我们可以直接在服务器上使用java -jar的命令运行此jar包

![image-20200616183708919](D:\笔记\SpringBoot入门HelloWorld.assets\image-20200616183708919.png)