# SpringBoot 3 Log

# 主要内容

> [简介](#简介)  
> [实现原理](#实现原理)    
> [使用方式](#使用方式)  
> [默认格式](#默认格式)  
> [日志分组](#日志分组)  
> [输出日志文件](#输出日志文件)  
> [文件归档与滚动切割](#文件归档与滚动切割)  
> [自定义配置](#自定义配置)  
> [切换日志引入方式](#切换日志引入方式)  
> [最佳实战](#最佳实战)  
> [关于性能问题](#关于性能问题)  

## 简介

SpringBoot 的底层 Spring 使用 commons-logging 作为内部日志,但底层日志实现是开放的,可对接其他日志框架, Spring 5 及以后
commons-logging 被 Spring 重写了.支持 jul, log4j2, logback等日志框架.SpringBoot 提供了默认的控制台输出配置,也可以配置输出为文件.
SpringBoot 的默认实现是 logback.

## 实现原理

- 每个 starter 组件,都会导入 spring-boot-starter
- spring-boot-starter 导入了 spring-boot-starter-logging 日志组件
- spring-boot-starter-logging 使用 logback + slf4j 组合作为默认底层日志实现
- 日志组件的加载是通过 ApplicationListener,应用启动时即可使用日志功能.
- 默认值参照 spring-boot 下 additional-spring-configuration-metadata.json 文件
- 日志所有的配置都可以通过修改配置文件 logging.* 实现.
- 可替换 logback 实现

## 使用方式

```text
// 代码中日志使用
Logger logger = LoggerFactory.getLogger(getClass());

或者 Lombok 的 @Slf4j 注解生成的 log 对象
```

## 默认格式

```text
2024-06-09T18:26:47.511+08:00  INFO 4344 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2024-06-09T18:26:47.511+08:00  INFO 4344 --- [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.7]
```

默认输出格式:

- 时间和日期:毫秒级精度
- 日志级别:ERROR, WARN, INFO, DEBUG, TRACE.
- 进程 ID
- ---: 消息分割符
- 线程名: 使用[]包含
- Logger 名: 通常是产生日志的类名
- 消息: 日志记录的内容

SpringBoot 日志默认级别是 INFO

在 application.properties/yaml 中配置 logging.level.<logger-name>=<level> 指定日志级别

配置 logging.level.root = warn 代表所有未指定日志级别都使用 warn 级别

## 日志分组

将相关的 logger 分组在一起,统一配置.例: Tomcat 相关的日志统一设置

```text
logging.group.tomcat=org.apache.catalina,org.apache.coyote,org.apache.tomcat
logging.level.tomcat=trace
```

SpringBoot 预定义两个组 web 和 sql

web

- org.springframework.core.codec
- org.springframework.http
- org.springframework.web
- org.springframework.boot.actuate.endpoint.web
- org.springframework.boot.web.servlet.ServletContextInitializerBeans

sql

- org.springframework.jdbc.core
- org.hibernate.SQL
- org.jooq.tools.LoggerListener

## 输出日志文件

SpringBoot 默认只把日志写在控制台,如果想额外记录到文件,可以在 application.properties/yaml 中添加 logging.file.name or
logging.file.path, name 优先级更高.

## 文件归档与滚动切割

每天的日志应该独立分割出来存档,使用 logback 可以通过 application.properties/yaml 文件指定日志滚动规则

如果是其他日志系统,需要自行配置 log4j2.xml 或 log4j2-spring.xml

滚动规则设置如下

- logging.logback.rollingpolicy.file-name-pattern 日志存档的文件名格式,默认值:${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz
- logging.logback.rollingpolicy.clean-history-on-start 应用启动时是否清除以前存档,默认值:false
- logging.logback.rollingpolicy.max-file-size 存档前,每个日志文件的最大大小,默认值:10MB
- logging.logback.rollingpolicy.total-size-cap 日志文件存储容量,默认值:0B,设置 1GB 则磁盘存储超过 1GB 日志后就会删除旧日志
- logging.logback.rollingpolicy.max-history 日志文件保存的最大天数,默认值:7

## 自定义配置

- Logback logback-spring.xml/logback-spring.groovy/logback.xml/logback.groovy
- Log4j2 log4j2-spring.xml/log4j2.xml
- Java Util Logging logging.properties

配置文件名为 *-spring.xml, spring 可以初始化此配置.

## 切换日志引入方式

```text
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

log4j2 还支持 yaml 和 json 格式的配置文件

YAML log4j2.yaml/log4j2.yml 所需依赖

- com.fasterxml.jackson.core:jackson-databind
- com.fasterxml.jackson.dataformat:jackson-dataformat-yaml

JSON log4j2.json/log4j2.jsn 所需依赖

- com.fasterxml.jackson.core:jackson-databind

## 最佳实战

- 导入任何第三方框架,可先排除它的日志包,日志交给 SpringBoot 统一管理.
- 修改 application.properties 配置文件,就可以调整日志的所有行为.也可以使用固定配置文件
  logback-spring.xml,log4j2-spring.xml
- 如需对接专业日志系统,只需要把 logback 记录的日志输出至 kafka 之类的中间件,修改配置文件即可
- 不要使用 System.out.println();

## 关于性能问题

测试表明 log4j2 性能大概是 logback 的 2 倍,如果应用有高性能要求,则使用 log4j2,对于多数应用 logback 也是够用的.官方默认使用
logback 是认为
日志性能不是程序的瓶颈,不愿作这项重大变更,影响用户升级.

https://github.com/spring-projects/spring-boot/issues/16864#issuecomment-492570488

----

以上就是本文核心内容.

[log4j2-spring.xml 模版](images%2F0019_springboot3_log%2Flog4j2-spring.xml)

[logback-spring.xml 模版](images%2F0019_springboot3_log%2Flogback-spring.xml)

[返回顶部](#主要内容)

