# Java知识点

# 主要内容

> [基础](#基础)  
> [基础强化](#基础强化)  
> [基础强化](#基础强化)  
> [计算机基础](#计算机基础)  
> [企业应用](#企业应用)  
> [企业进阶](#企业进阶)  
> [常用类库](#常用类库)  
> [常用软件](#常用软件)  
> [并发编程](#并发编程)  
> [虚拟机](#虚拟机)  
> [架构设计](#架构设计)  
> [常见系统](#常见系统)  
> [热门文档](#热门文档)  

----

# 正文

## 基础

- 环境搭建
- IDE工具
- AI编程助手
- 基础语法
- 注释
- 对象和类
- 基本类型和包装类型
- 命名规范
- 修饰符
- 运算符
- 循环
- 条件语句
- switch
- 常用类(`Number` `Math` `Character` `String` `Date` `LocalDate`)
- 数组
- 正则表达式
- 方法
- `Stream` `File` `IO`
- Scanner
- 异常处理

#### 对象基础

- 封装
- 继承
- 多态
- Override/Overload
- 抽象类
- 接口
- 枚举
- package

## 基础强化

- 数据结构
- 集合
- ArrayList
- LinkedList
- HashSet
- HashMap
- Iterator
- Object
- 泛型
- 序列化
- 网络编程
- 邮件
- 多线程
- Applet
- 文档注释
- 实例
- MySQL
- 各版本新特性
- 动态代理
- Java探针
- 字节码 `https://github.com/fuzhengwei/itstack-demo-bytecode`
- UnSafe 类
- 进程 / 线程
- 书籍`Java 核心技术卷`
- 数据结构与算法

## 计算机基础

- 计算机组成原理
- 操作系统

#### 计算机网络
   
- 网络分层模型
- 网络传输过程
- `IP` `端口`
- `UDP` `TCP`
- `HTTP` `HTTPS`
- `ARP`地址解析协议
- `DNS`域名解析
- 网络安全

## 企业应用

#### 数据库

- 数据库搭建
- SQL语句编写
- 约束
- 索引
- 事务
- 锁
- 库表设计
- 性能优化

#### `Java Web`

- html,css,js
- XML
- JSON
- Servlet
- Filter
- Listener
- JSP
- JSTL
- Cookie
- Session

#### Spring

- 控制反转IoC(Inversion of Control)
- 依赖注入DI(Dependency Injection)
- 面向切面编程AOP(Aspect Oriented Programming)
- `事件` `资源` `i18n` `验证` `数据绑定` `类型转换` `SpEL`
- 生态(测试,数据访问,Web,集成,语言)

#### SpringMVC

- 模型-视图-控制 MVC(Model View Controller)
- 拦截器
- 注解
- `Restful API`

#### `MyBatis`/`MyBatis Plus`

- 增删改查
- 全局配置
- 动态`SQL`
- 缓存

#### SpringBoot

- 常用注解
- 资源整合
- 自动注入实现

#### `Spring Security`/`Shiro`

- 用户认证
- 权限管理

#### `Maven`/`Gradle`

- 构建
- 依赖管理
- 插件
- 配置
- 子父工程
- 多模块打包构建
- `Nexus`私服搭建

#### 开发规范

- 阿里巴巴`Java`开发手册

#### `Git`/`SVN`

- 工作区
- 代码`提交` `推送` `拉取` `回退` `重置`
- `代码合并` `解决冲突`
- 分支
- 标签
- cherry-pick
- Git Flow

#### Linux

- Linux系统安装
- 环境变量
- 文件管理
- 用户管理
- 内存管理
- 磁盘管理
- 进程管理
- 网络管理
- 软件包管理
- 服务管理
- 日志管理
- Linux内核
- 常用命令
- 常用环境搭建
- Shell脚本编程
- VIM的使用

#### 前端

- HTML
- CSS
- JavaScript
- Ajax
- `Vue`/`React`

## 企业进阶

#### 软件工程

- 软件的本质
- 软件特性
- 软件过程
- 软件开发原则
   - 开闭原则
   - 里氏替换原则
   - 依赖倒置原则
   - 单一职责原则
   - 接口隔离原则
   - 迪米特法则
- 软件过程模型
- 敏捷开发
- 软件开发模型
- 需求建模
- 软件设计
- UML
- 体系结构设计
- 设计模式
- 软件质量管理
- 评审
- 软件质量保证
- 软件测试
   - 单元测试
   - 集成测试
   - 系统测试
   - 压力测试
   - 部署测试
- 软件配置管理
- 软件项目管理
- 软件项目估算
- 项目进度安排
- 风险管理
- 软件过程改进
- 成熟度模型

#### 设计模式

- 创建型模式 对象实例化的模式，创建型模式用于解耦对象的实例化过程
   - 单例模式
   - 工厂方法模式
   - 抽象工厂
   - 建造者模式
   - 原型模式
- 结构型模式 把类或对象结合在一起形成一个更大的结构
   - 适配器模式
   - 组合模式
   - 装饰器模式
   - 代理模式
   - 享元模式
   - 外观模式
   - 桥接模式
- 行为型模式 类和对象如何交互，及划分责任和算法
   - 迭代器模式
   - 模板方法模式
   - 策略模式
   - 命令模式
   - 状态模式
   - 责任链模式
   - 备忘录模式
   - 观察者模式
   - 访问者模式
   - 中介者模式
   - 解释器模式

#### 缓存

- 缓存概念
- 本地缓存
- 多级缓存
- Redis基础
   - Redis分布式缓存
   - 数据类型
   - 常用操作
   - Java操作Redis
   - Spring Boot Redis Template
   - Redisson
   - 主从模型搭建
   - 哨兵集群搭建
   - 日志持久化
- Redis应用场景
   - 数据共享
   - 单点登录
   - 计数器
   - 限流
   - 点赞
   - 实时排行榜
   - 分布式锁
- 缓存常见问题
   - 缓存雪崩
   - 缓存击穿
   - 缓存穿透
   - 缓存更新一致性
- Caffeine
- Memcached
- Ehcache

#### 消息队列

- 使用场景
- RabbitMQ
   - 生产消费模型
   - 交换机模型
   - 死信队列
   - 延迟队列
   - 消息持久化
   - Java 操作
   - 集群搭建
- `Kafka` `ActiveMQ` `TubeMQ` `RocketMQ`

#### Nginx

- 简介
- 正向代理
- 反向代理
- 负载均衡
- 常用命令
- 配置
- 动静分离
- 网站部署
- 集群搭建
- `HAProxy` `Apache`

#### Netty网络编程

- IO模型`BIO`/`NIO`
- Channel
- Buffer
- Seletor
- Netty模型
- WebSocket编程
- 相关技术:[Vertx](http://vertxchina.github.io/vertx-translation-chinese/)

#### 微服务

##### Dubbo

- 架构演进
- RPC
- Zookeeper
- 服务提供者
- 服务消费者
- 项目搭建
- DubboX

##### Spring Cloud

- 服务注册与发现
- 注册中心 `Eureka` `Zookeeper` `Consul`
- `Ribbon` 负载均衡
- `Feign` 服务调用
- `Hystrix` `服务限流` `降级` `熔断`
- `Resilience4j` 服务容错
- `Gateway` `Zuul` 微服务网关
- `Config` 分布式配置中心
- 分布式服务总线
- `Sleuth` + `Zipkin` 分布式链路追踪

##### Spring Cloud Alibaba

- `Nacos` `注册` `配置中心`
- `OpenFeign` 服务调用
- `Sentinel` 流控
- `Seata` 分布式事务

#### 接口管理

- Swagger
- Knife4j
- Spring Doc
- Postman
- Apifox

#### 容器

###### Docker

- 容器概念
- 镜像
- 部署服务
- Dockerfile
- Docker Compose
- Docker Machine
- Docker Swarm
- 多阶段构建

###### K8S `Kubernetes`

- K8S 架构
- 工作负载
- 资源类型
- Pod
- Pod 生命周期
- Pod 安全策略
- K8S 组件
- K8S 对象
- 部署应用
- 服务
- Ingress
- Kubectl 命令行
- 集群管理

##### `Apache Mesos` `Mesosphere`

####  `CI` / `CD`

- 概念
- 平台
- `Jenkins` `GitLab` `微信云托管`

## 常用类库
- Lombok [https://github.com/projectlombok/lombok](https://github.com/projectlombok/lombok)
- Hutool [https://github.com/looly/hutool](https://github.com/looly/hutool)
- JUnit [https://github.com/junit-team/junit4](https://github.com/junit-team/junit4)
- Guava [https://github.com/google/guava](https://github.com/google/guava)
- Apache Commons [https://github.com/apache/commons-lang](https://github.com/apache/commons-lang)
- Apache HttpComponents Client [https://github.com/apache/httpcomponents-client](https://github.com/apache/httpcomponents-client)
- OkHttp [https://github.com/square/okhttp](https://github.com/square/okhttp)
- Gson [https://github.com/google/gson](https://github.com/google/gson)
- Jcommander [https://github.com/cbeust/jcommander](https://github.com/cbeust/jcommander)
- Apache PDFBox [https://github.com/apache/pdfbox](https://github.com/apache/pdfbox)
- EasyExcel [https://github.com/alibaba/easyexcel](https://github.com/alibaba/easyexcel)
- Apache POI [https://github.com/apache/poi](https://github.com/apache/poi)
- Mockito [https://github.com/mockito/mockito](https://github.com/mockito/mockito)
- Selenium [https://github.com/SeleniumHQ/selenium](https://github.com/SeleniumHQ/selenium)
- htmlunit [https://github.com/HtmlUnit/htmlunit](https://github.com/HtmlUnit/htmlunit)
- TestNG [https://github.com/cbeust/testng](https://github.com/cbeust/testng)
- Jacoco [https://github.com/jacoco/jacoco](https://github.com/jacoco/jacoco)
- cglib [https://github.com/cglib/cglib](https://github.com/cglib/cglib)
- Arthas [https://github.com/alibaba/arthas](https://github.com/alibaba/arthas)
- config [https://github.com/lightbend/config](https://github.com/lightbend/config)
- Quasar [https://github.com/puniverse/quasar](https://github.com/puniverse/quasar)
- drools [https://github.com/kiegroup/drools](https://github.com/kiegroup/drools)
- Caffeine [https://github.com/ben-manes/caffeine](https://github.com/ben-manes/caffeine)
- Disruptor [https://github.com/LMAX-Exchange/disruptor](https://github.com/LMAX-Exchange/disruptor)
- Knife4j [https://doc.xiaominfo.com/](https://doc.xiaominfo.com/)
- Thumbnailator [https://github.com/coobird/thumbnailator](https://github.com/coobird/thumbnailator)
- Logback [https://github.com/qos-ch/logback](https://github.com/qos-ch/logback)
- Apache Camel [https://github.com/apache/camel](https://github.com/apache/camel)
- Quartz [https://github.com/quartz-scheduler/quartz](https://github.com/quartz-scheduler/quartz)
- Apache Mahout [https://github.com/apache/mahout](https://github.com/apache/mahout)
- Apache OpenNLP [https://github.com/apache/opennlp](https://github.com/apache/opennlp)
- RxJava [https://github.com/ReactiveX/RxJava](https://github.com/ReactiveX/RxJava)
- JProfile [https://www.ej-technologies.com/products/jprofiler/overview.html](https://www.ej-technologies.com/products/jprofiler/overview.html)
- jsoup [https://jsoup.org/](https://jsoup.org/)
- webmagic [https://github.com/code4craft/webmagic/](https://github.com/code4craft/webmagic/)

## 常用软件

- JetBrains IDEA [https://www.jetbrains.com/idea/](https://www.jetbrains.com/idea/)
- Redis Desktop Manager Redis [https://github.com/uglide/RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)
- Postman [https://www.postman.com/](https://www.postman.com/)
- Apifox [https://apifox.com/](https://apifox.com/)
- VMware [https://www.vmware.com/](https://www.vmware.com/)
- XMind [https://www.xmind.cn/](https://www.xmind.cn/)
- Visual Studio Code [https://code.visualstudio.com/](https://code.visualstudio.com/)
- Sublime Text [https://www.sublimetext.com/](https://www.sublimetext.com/)
- Navicat [https://www.navicat.com.cn/](https://www.navicat.com.cn/)
- JMeter Java [https://jmeter.apache.org/](https://jmeter.apache.org/)
- JVisual VM [https://visualvm.github.io/](https://visualvm.github.io/)
- MobaXterm [https://mobaxterm.mobatek.net/](https://mobaxterm.mobatek.net/)
- XShell SSH [https://www.netsarang.com/zh/xshell/](https://www.netsarang.com/zh/xshell/)
- XFtp FTP [https://www.netsarang.com/zh/xftp/](https://www.netsarang.com/zh/xftp/)
- Chocolatey Windows [https://chocolatey.org/](https://chocolatey.org/)
- Typora [https://typora.io/](https://typora.io/)
- Ditto [https://ditto-cp.sourceforge.io/](https://ditto-cp.sourceforge.io/)
- uTools [https://u.tools/](https://u.tools/)
- Qdir Windows [https://q-dir.en.softonic.com/](https://q-dir.en.softonic.com/)

## 并发编程

- 线程和进程
- 线程状态
- 并行和并发
- 同步和异步
- Synchronized
- `Volatile` 关键字
- Lock 锁
- 死锁
- 可重入锁
- 线程安全
- 线程池
- JUC 的使用
- AQS
- Fork Join
- CAS

## 虚拟机

- JVM 内存结构
- JVM 生命周期
- 主流虚拟机
- Java 代码执行流程
- 类加载
- 类加载器
   - 类加载过程
   - 双亲委派机制
- 垃圾回收
   - 垃圾回收器
   - 垃圾回收策略
   - 垃圾回收算法
   - StopTheWorld
- 字节码
- 内存分配和回收
- JVM 性能调优
   - 性能分析方法
   - 常用工具
   - 参数设置
- Java 探针
- 线上故障分析

## 架构设计

- 分布式理论
   - CAP
   - BASE
- 分布式缓存
   - Redis
   - Memcached
   - Etcd
- 一致性算法
   - Raft
   - Paxos
   - 一致性哈希
- 分布式事务
   - 解决方案
   - 2PC
   - 3PC
   - TCC
   - 本地消息表
   - MQ 事务消息
   - 最大努力通知
   - LCN 分布式事务框架 https://github.com/codingapi/tx-lcn
- 分布式 id 生成
   - Leaf 分布式 id 生成服务 https://github.com/Meituan-Dianping/Leaf
- 分布式任务调度
   - XXL-JOB 调度平台 https://www.xuxueli.com/xxl-job/
   - elastic-job https://gitee.com/elasticjob/elastic-job
- 分布式服务调用
   - trpc
- 分布式存储
   - HDFS
   - Ceph
- 分布式数据库
   - TiDB
   - OceanBase
- 分布式文件系统
   - HDFS
- 分布式协调
   - Zookeeper
- 分布式监控
   - Prometheus
   - Zabbix
- 分布式消息队列
   - RabbitMQ
   - Kafka
   - Apache Pulsar
- 分布式日志收集
   - Elastic Stack
   - Loki
- 分布式搜索引擎
   - Elasticsearch
- 分布式链路追踪
   - Apache SkyWalking
- 分布式配置中心
   - Apollo
   - Nacos  
- 大数据5v `volume大体量` `variety多样性` `velocity时效性` `veracity准确性` `value大价值` 

#### 高可用

- 限流
- 降级熔断
- 冷备
- 双机热备
- 同城双活
- 异地双活
- 异地多活
- 容灾备份

#### 高并发

- 数据库
   - 分库分表
   - MyCat 中间件
   - Apache ShardingSphere 中间件
   - 读写分离
- 缓存
   - 缓存雪崩
   - 缓存击穿
   - 缓存穿透
- 负载均衡
   - 负载均衡算法
   - 软硬件负载均衡(`2` `3` `4` `7`层)

#### 服务网格

- Istio
   - 流量管理
   - 安全性
   - 可观测性
- `Envoy`开源的边缘和服务代理

#### 领域驱动设计

- DDD 的优势
- DDD 的适用场景
- DDD 核心概念
   - 领域模型分类：失血、贫血、充血、涨血
   - 子域划分：核心域、通用域、支撑域
   - 限界上下文
   - 实体和值对象
   - 聚合设计
   - 领域事件
- DDD 实践

#### 技术前沿

- Sidecar
- Serverless
- 云原生
- 热数据探测技术 京东 `HotKey`
- 数据库流水订阅 阿里 `Canal`
- 监控告警
- 应用安全
- 故障演练
- 流量回放

## 常见系统

- 广告系统
- 电商系统
- 搜索系统
- 支付转账
- 游戏后台
- 即时通讯
- 社交系统
- CMS 系统
- ERP 系统
- OA 系统
- 代码生成
- 权限管理
- 秒杀活动

## 热门文档

- [Spring Framework](https://spring.io/projects/spring-framework)  
- [Spring MVC](https://spring.io/projects/spring-webflow)  
- [Spring Boot](https://spring.io/projects/spring-boot)  
- [Spring Cloud](https://spring.io/projects/spring-cloud)  
- [MyBatis](https://blog.mybatis.org/)  
- [Hibernate](https://hibernate.org/)  
- [Apache Struts](https://struts.apache.org/)  
- [Apache Kafka](https://kafka.apache.org/)  
- [Apache Spark](https://spark.apache.org/)  
- [JSF](https://www.tutorialspoint.com/jsf/index.htm)  
- [Apache Wicket](https://wicket.apache.org/)  
- [Netty](https://netty.io/)  
- [Dubbo](https://dubbo.apache.org/)  
- [Quasar](http://www.quasarchs.com/)  
- [Istio](https://istio.io/)  
- [GraalVM](https://www.graalvm.org/)  
- [zgc](https://openjdk.org/projects/zgc/)  
- [MySQL](https://www.mysql.com/)  
- [PostgreSQL](https://www.postgresql.org/)  
- [Redis](https://redis.io/)  
- [Apache Pulsar](https://pulsar.apache.org/)
- [Elastic Stack](https://www.elastic.co/elastic-stack)
- [Elastic](https://www.elastic.co/)
- [logstash](https://www.elastic.co/cn/logstash)
- [kibana](https://www.elastic.co/cn/kibana)
- [beats](https://www.elastic.co/beats)
- [Docker](https://www.docker.com/)
- [K8S](https://kubernetes.io/)
- [Jenkins](https://www.jenkins.io/)
- [Hadoop](https://hadoop.apache.org/)
- [Spark](https://spark.apache.org/)
- [Flink](https://flink.apache.org/)
- [Storm](https://storm.apache.org/)
- [Hive](https://hive.apache.org/)
- [HBase](https://hbase.apache.org/)
- [Druid](https://druid.apache.org/)
- [Kylin](https://kylin.apache.org/)
- [Pig](https://pig.apache.org/)
- [Mahout](https://mahout.apache.org/)

- [StackOverflow](https://stackoverflow.com/questions/tagged/java)  
- [GitHub Java 专区](https://github.com/topics/java)  
- [GitHub Java 合集](https://github.com/akullpp/awesome-java)  
- [掘金 Java 专区](https://juejin.cn/tag/Java)  
- [美团技术团队](https://tech.meituan.com/)  
- [ZGC 介绍](https://juejin.cn/post/6859276583656980493)  
- [GraalVM 介绍](https://juejin.cn/post/6850418120570437646)  
- [服务网格介绍](https://www.redhat.com/zh/topics/microservices/what-is-a-service-mesh)  
- [云原生介绍](https://www.jianshu.com/p/a37baa7c3eff)  

[返回顶部](#主要内容)

