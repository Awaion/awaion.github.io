# Spring Boot 3 + Spring Web + MyBatis

# 主要内容

> [简介](#简介)  
> [集成步骤](#集成步骤)  
> [自动配置原理](#自动配置原理)  
> [快速定位生效的配置](#快速定位生效的配置)  

## 简介

常用 Web 框架和 ORM 框架

## 集成步骤

```
// pom.xml 新增依赖
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>3.0.3</version>
</dependency>
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>

// application.properties 新增配置
# 数据源配置
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/demo014
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
# 指定mapper映射文件位置
mybatis.mapper-locations=classpath:/mapper/*.xml
# 驼峰映射
mybatis.configuration.map-underscore-to-camel-case=true

// *Mapper
public interface LoginInfoMapper {
    LoginInfo getLoginInfoById(@Param("id") Long id);
}

// *Mapper.xml
<select id="getLoginInfoById" resultType="com.awaion.demo014.domain.LoginInfo">
    select
        *
    from login_info
    where
        id = #{id}
</select>

// *Controller
@RestController
public class Demo014Controller {
    @GetMapping("/get")
    public LoginInfo getById() {
        return mapper.getLoginInfoById(1L);
    }
}
```

## 自动配置原理

mybatis-spring-boot-starter 导入 spring-boot-starter-jdbc
- DataSourceAutoConfiguration 数据源自动配置
- DataSourceProperties 数据源配置键值对
- HikariDataSource 默认数据源
- JdbcTemplateAutoConfiguration 数据操作模版
- JndiDataSourceAutoConfiguration Java Naming and Directory Interface
- XADataSourceAutoConfiguration 基于XA二阶提交协议的分布式事务数据源
- DataSourceTransactionManagerAutoConfiguration 事务管理

mybatis-spring-boot-starter 导入 mybatis-spring-boot-autoconfigure
- classpath:/META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports 自动加载路径
- MybatisLanguageDriverAutoConfiguration
- MybatisAutoConfiguration 包含 SqlSessionFactory,SqlSessionTemplate 组件,用于操作数据库
- MybatisProperties MyBatis配置键值对
- @Import(MapperScannerRegistrar.class) 解析 @MapperScan 指定的路径

## 快速定位生效的配置

```text
# 开启调试模式, 详细打印配置信息 Positive(生效的自动配置) Negative(不生效的自动配置)
debug=true
```
----

以上就是本文核心内容.

[Github 源码](https://github.com/Awaion/tools/tree/master/demo014)

[返回顶部](#主要内容)

