---
title: Lombok
date: 2018-12-20 10:25:56
tags: 
    - Lombok
categories: 
    - Java
---

**目录 start**
 
1. [Lombok](#lombok)
    1. [何为Lombok](#何为lombok)
    1. [为什么要用](#为什么要用)
    1. [为什么不要用](#为什么不要用)
    1. [个人见解](#个人见解)
1. [配置](#配置)
    1. [Maven](#maven)
        1. [普通Java项目](#普通java项目)
        1. [Groovy和Java项目使用Lombok](#groovy和java项目使用lombok)
    1. [Gradle](#gradle)
1. [使用](#使用)
    1. [注解使用](#注解使用)
        1. [POJO常用](#pojo常用)
        1. [日志相关](#日志相关)
        1. [异常相关](#异常相关)

**目录 end**|_2019-04-19 13:04_| [Kuangcp](https://github.com/Kuangcp/Note) | [yi-yun](https://github.com/yi-yun/Memo)
****************************************
# Lombok

## 何为Lombok
> [Github: lombok](https://github.com/rzwitserloot/lombok) | [Official site](https://projectlombok.org/)

## 为什么要用
> 简化JavaBean 更为直观 省去了Setter Getter toString hashCode 构造器等方法  
> 一定有人就会跳出来说 在IDE中几个快捷键的事情,何必这么复杂, 
>> 那他们一定是没有遇到修改的时候吧, 改个属性的名字,类型, 对应的方法你需要改吧, 但是使用lombok就不用担心了

## 为什么不要用

## 个人见解
> Lombok在IDE中安装插件是为了编译和构建中能够动态的添加Getter Setter 等方法, 而在Maven或者Gradle中添加是为了注解能够引用得到??

************************************************

# 配置
## Maven
> [Official Guide](https://projectlombok.org/setup/maven)

### 普通Java项目
```xml
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.16.10</version>
    </dependency>
```

### Groovy和Java项目使用Lombok

_配置编译插件_
```xml
<!--lombok-->
<plugin>
<artifactId>maven-compiler-plugin</artifactId>
<version>3.7.0</version>
<configuration>
    <target>1.8</target>
    <source>1.8</source>
    <encoding>UTF-8</encoding>
    <compilerId>groovy-eclipse-compiler</compilerId>
    <verbose>true</verbose>
    <fork>true</fork>
    <compilerArguments>
    <javaAgentClass>lombok.launch.Agent</javaAgentClass>
    </compilerArguments>
</configuration>
<dependencies>
    <dependency>
    <groupId>org.codehaus.groovy</groupId>
    <artifactId>groovy-eclipse-compiler</artifactId>
    <version>2.9.3-01</version>
    </dependency>
    <dependency>
    <groupId>org.codehaus.groovy</groupId>
    <artifactId>groovy-eclipse-batch</artifactId>
    <version>2.5.0-01</version>
    </dependency>
    <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.10</version>
    </dependency>
</dependencies>
</plugin>
```
_添加依赖_
```xml
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.16.10</version>
    </dependency>
```

## Gradle 

> [使用Lombok的正确方式](https://stackoverflow.com/questions/50519138/annotationprocessor-gradle-4-7-configuration-doesnt-run-lombok)   
> [gradle lombok plugin](https://projectlombok.org/setup/gradle)  
> [Official Guide](https://docs.gradle.org/4.7-rc-1/userguide/java_plugin.html#sec:java_compile_avoidance)  

```groovy
  annotationProcessor 'org.projectlombok:lombok:1.18.2'
  compileOnly 'org.projectlombok:lombok:1.18.2'
  testAnnotationProcessor 'org.projectlombok:lombok:1.18.2'
  testCompileOnly 'org.projectlombok:lombok:1.18.2'
```
*************************

# 使用
- [Lombok 注解在线帮助文档](http://projectlombok.org/features/index)

## 注解使用
### POJO常用
- `@Data` ：注解在类上；提供类所有属性的 getter 和 setter 方法，此外还提供了equals、canEqual、hashCode、toString 方法
- `@Setter`：注解在属性上；为属性提供 setter 方法
- `@Getter`：注解在属性上；为属性提供 getter 方法
- `@NoArgsConstructor`：注解在类上；为类提供一个无参的构造方法
- `@AllArgsConstructor`：注解在类上；为类提供一个全参的构造方法

### 日志相关
> [Official log](https://projectlombok.org/features/log)

- @CommonsLog
  - `private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog(LogExample.class);`
- @Flogger
  - `private static final com.google.common.flogger.FluentLogger log = com.google.common.flogger.FluentLogger.forEnclosingClass();`
- @JBossLog
  - `private static final org.jboss.logging.Logger log = org.jboss.logging.Logger.getLogger(LogExample.class);`
- @Log
  - `private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName());`
- @Log4j
  - `private static final org.apache.log4j.Logger log = org.apache.log4j.Logger.getLogger(LogExample.class);`
- @Log4j2
  - `private static final org.apache.logging.log4j.Logger log = org.apache.logging.log4j.LogManager.getLogger(LogExample.class);`
- @Slf4j
  - `private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class);`
- @XSlf4j
- `private static final org.slf4j.ext.XLogger log = org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class);`

### 异常相关
1. [@SneakyThrows](https://projectlombok.org/features/SneakyThrows)

