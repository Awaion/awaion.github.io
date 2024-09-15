# Spring Boot 3 特性

# 主要内容

> [banner](#banner)  
> [自定义 SpringApplication](#自定义-springapplication)  
> [Profiles](#profiles)  
> [单元测试](#单元测试)  

## banner

- 默认读取文件 resource/banner.txt
- 自定义读取文件 spring.banner.location=banner.txt
- 字符图案生成 https://www.bootschool.net/ascii-art/religion

## 自定义 SpringApplication

```text
// 方式一 new
public static void main(String[] args) {
    SpringApplication application = new SpringApplication(MyApplication.class);
    application.setBannerMode(Banner.Mode.OFF);
    application.run(args);
}

// 方式二 FluentBuilder API
new SpringApplicationBuilder()
    .sources(Parent.class)
    .child(Application.class)
    .bannerMode(Banner.Mode.OFF)
    .run(args);
```

## Profiles

- Spring Profiles 提供一种隔离配置的方式,使其仅在特定环境生效
- 任何 @Component, @Configuration, @ConfigurationProperties等组件注解,都可以使用 @Profile 标记,指定何时被加载
- 激活指定配置文件 spring.profiles.active=prod,init
- 命令行激活指定配置文件 --spring.profiles.active=prod,init
- 指定默认配置文件 spring.profiles.default=test
- 默认配置文件 *-default.properties
- 新增配置文件 spring.profiles.include[0]=common
- 配置分组 spring.profiles.group.prod[0]=db 表示 prod,db 都生效
- 环境配置文件 application-{profile}.properties
- 导入配置文件 spring.config.import=my.properties
- 占位符 app.description=${app.name}

----

- 配置优先级 常用方式 命令行 + 配置文件
1. 默认属性 通过 SpringApplication.setDefaultProperties 指定的
2. @PropertySource 指定加载的配置,需要写在 @Configuration 类上才可生效
3. 配置文件 application.properties/yml 等 .properties 优先于 .yml
   1. jar 包内的application.properties/yml 目录优先级 /resource < /resource/config < /resource/config/**
   2. jar 包内的application-{profile}.properties/yml 目录优先级 /resource < /resource/config < /resource/config/**
   3. jar 包外的application.properties/yml 目录优先级 /resource < /resource/config < /resource/config/**
   4. jar 包外的application-{profile}.properties/yml 目录优先级 /resource < /resource/config < /resource/config/**
4. RandomValuePropertySource 支持的 random.* 配置 如：@Value("${random.int}")
5. OS 环境变量
6. Java 系统属性 System.getProperties()
7. JNDI 属性 来自java:comp/env
8. ServletContext 初始化参数
9. ServletConfig 初始化参数
10. SPRING_APPLICATION_JSON属性 内置在环境变量或系统属性中的 JSON
11. 命令行参数
12. 测试属性 @SpringBootTest进行测试时指定的属性
13. 测试类 @TestPropertySource 注解
14. Devtools 设置的全局属性 $HOME/.config/spring-boot

## 单元测试

```text
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>

默认提供的组件 JUnit 5, Spring Test, AssertJ, Hamcrest, Mockito, JSONassert, JsonPath

使用 @SpringBootTest 注解后,直接用 @Autowired 进行测试
```

----

常用注解

- @Test 表示方法是测试方法,与 JUnit4 不同,他的职责非常单一不能声明任何属性,拓展的测试将会由Jupiter提供额外测试
- @ParameterizedTest 表示方法是参数化测试
- @RepeatedTest 表示方法可重复执行
- @DisplayName 为测试类或者测试方法设置展示名称
- @BeforeEach 表示在每个单元测试之前执行
- @AfterEach 表示在每个单元测试之后执行
- @BeforeAll 表示在所有单元测试之前执行
- @AfterAll 表示在所有单元测试之后执行
- @Tag 表示单元测试类别,类似于 JUnit4 中的 @Categories
- @Disabled 表示测试类或测试方法不执行,类似于 JUnit4 中的 @Ignore
- @Timeout 表示测试方法运行如果超过了指定时间将会返回错误
- @ExtendWith 为测试类或测试方法提供扩展类引用
- https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations

----

断言

- assertEquals 判断两个对象或两个原始类型是否相等
- assertNotEquals 判断两个对象或两个原始类型是否不相等
- assertSame 判断两个对象引用是否指向同一个对象
- assertNotSame 判断两个对象引用是否指向不同的对象
- assertTrue 判断给定的布尔值是否为 true
- assertFalse 判断给定的布尔值是否为 false
- assertNull 判断给定的对象引用是否为 null
- assertNotNull 判断给定的对象引用是否不为 null
- assertArrayEquals 数组断言
- assertAll 组合断言
- assertThrows 异常断言
- assertTimeout 超时断言
- fail 快速失败

----

嵌套测试

JUnit 5 可以通过 Java 中的内部类和@Nested 注解实现嵌套测试,从而可以更好的把相关的测试方法组织在一起.在内部类中可以使用 @BeforeEach 和 
@AfterEach 注解,而且嵌套的层次没有限制.

----

参数化测试

参数化测试是 JUnit5 很重要的一个新特性,它使得用不同的参数多次运行测试成为了可能,也为我们的单元测试带来许多便利.

- @ValueSource 为参数化测试指定入参来源,支持八大基础类以及 String 类型, Class 类型
- @NullSource 表示为参数化测试提供一个 null 的入参
- @EnumSource 表示为参数化测试提供一个枚举入参
- @CsvFileSource 表示读取指定 CSV 文件内容作为参数化测试入参
- @MethodSource 表示读取指定方法的返回值作为参数化测试入参,注意方法返回需要是一个流

----

以上就是本文核心内容.

[返回顶部](#主要内容)

