# Java生态发展史

# 主要内容

> [Java历史](#Java历史)  
> [Apache Struts](#apache-struts)  
> [MyBatis](#mybatis)  
> [Hibernate](#hibernate)  
> [Spring Framework](#spring-framework)  
> [Spring Web Flow](#spring-web-flow)  
> [Netty](#netty)  
> [Jenkins](#jenkins)  
> [Apache Dubbo](#apache-dubbo)  
> [Elastic](#elastic)  
> [Apache Kafka](#apache-kafka)  
> [Apache Pulsar](#apache-pulsar)  
> [Apache Spark](#apache-spark)  
> [Apache Hadoop](#apache-hadoop)  
> [Apache Hive](#apache-hive)  
> [Apache HBase](#apache-hbase)  
> [Apache Druid](#apache-druid)  
> [Apache Kylin](#apache-kylin)  
> [Apache Flink](#apache-flink)  
> [Apache Storm](#apache-storm)  
> [Apache Pig](#apache-pig)  
> [Apache Mahout](#apache-mahout)  
> [Spring Boot](#spring-boot)  
> [Spring Cloud](#spring-cloud)  
> [Apache Tomcat](#apache-tomcat)  
> [Eclipse](#eclipse)  
> [IntelliJ IDEA](#intellij-idea)  
> [GraalVM](#graalvm)  
> [近期](#近期)  
> [参考](#参考)  

# 正文

## Java历史

1990年,美国`Sun Microsystems`公司的`James Gosling`博士和他的团队在一个叫做`Stealth`项目计划中,开始开发一种名为
`Oak (办公室外的一颗橡树)`的语言,因商标名称重复而改为`Java`,最初目标设置是在家用电器等小型系统的编程语言.

1994年,由于没能产生收益,开始将该技术应用于万维网.

1996年1月,`JDK1.0`发布.

`JDK1.0`发布后不久,所有主要的 Web 浏览器都包含了在网页中运行`Java Applet`的能力,这使`Java`成为互联网编程中最主流的技术之一.

`JDK2.0`将Java分为`Java SE` `Java EE` `Java ME`,其中`SE`是核心库和API,`EE`是应用程序服务器上运行的模块化组件,如`JDBC` `JPA`
`RMI` `JMS` `Web` `XML`等,`ME`用于开发移动设备和嵌入式系统.

随着互联网的兴起,`Web`应用程序的开发需求急剧增加,`Java`框架也随之而来.

2006年11月,`Sun`根据`GNU`通用公共许可证将其大部分`Java`虚拟机(JVM)作为`免费`和`开源软件`发布.

2007年5月,他们通过完全访问`JVM`的核心代码,完成了`Java开源`的过程.

2009年4月,甲骨文公司完成收购`Sun Microsystems`公司,并由此获得了`Java`技术的所有权利.

2010年4月,`James Gosling`从`Oracle`辞职. 

----

## Apache Struts

[Apache Struts 官网](https://struts.apache.org/index.html)

`Model-View-Controller`设计模式的应用框架   

2000年,`Craig McClanahan`采用了`MVC`的设计模式开发`Struts`.后来该框架产品一度被认为是`最广泛` `最流行`的`Java Web`应用框架.  

2004年3月,成为ASF(Apache Software Foundation)的顶级项目.  

2006年,`WebWork`与`Struts`的`Java EE Web`框架的团体,决定合作共同开发一个新的框架,整合了`WebWork`与`Struts`优点,并且更加`优雅`
`扩展性更强`的框架,命名为`Struts 2`

2008年12月,`Struts1`发布了最后一个正式版`1.3.10`,

2013年4月5日,`Struts开发组`宣布终止了`Struts 1`的软件开发周期.

2024年4月19日,发布`Apache Struts 6.4.0 GA`

----

## MyBatis

[MyBatis iBatis 官网](https://blog.mybatis.org/) 

2001年,`Clinton`启动了一个名为`iBATIS`的项目.最初的重点是加密软件解决方案的开发.`iBatis`发布的第一个产品是`Secrets`,一
个类似于`PGP`的个人数据加密和签名工具.`Secrets`完全用Java编写,并在开源许可下发布.

那一年,微软发表了一篇论文来证明其最新的`.NET 1.0`框架比Java更高效.为此,`Microsoft`构建了自己的`Sun`网络`Pet Store`版本,这是`Sun`
用来展示`Java`最佳实践的`Web`项目(Java BluePrints).微软声称`.NET`比`Java` `快10倍`,生产力`高4倍`.

2002年,`Clinton`开发了一个名为`JPetStore`的应用程序,以证明`Java`比`.NET`更高效,并且还可以实现比Microsoft实现中使用的更好的体系结构.

`JPetStore 1.0`产生了很大的影响,`Clinton`使用的数据库层引起了社区的关注.很快,`iBatis Database Layer 1.0`项目启动,由两个组件组成:
`iBatis DAO`和`iBatis SQL Maps`.

2004年6月,`iBatis 2.0`发布.它是完全重新设计的,同时保留了相同的功能,`Clinton`将`iBatis`名称和代码捐赠给ASF,该项目在ASF中保留了六年.

最终,考虑到有更好的 DAO 框架可用,例如Spring Framework,iBatis DAO 被弃用.

2010年5月19日,`iBatis 3.0`发布,同时开发团队决定在`Google Code`继续开发该框架.在一个名为`MyBatis`的新项目下.

2010年6月16日,`Apache`宣布`iBatis`退役并移至`Apache Attic`.

2024年4月2号,MyBatis发布3.5.16版本

----

## Hibernate

[Hibernate 官网](https://hibernate.org/) 

2001年,`Gavin King`与`Cirrus Technologies`的同事创建了`Hibernate`,作为`EJB2`样式实体`Bean`的替代方案.最初的目标是提供比`EJB2`
更好的持久性功能,简化复杂性并补充某些缺失的功能.

2003年初,`Hibernate`开发团队开始发布`Hibernate2`,它比第一个版本提供了许多重大改进.

`JBoss Inc.(现在是Red Hat的一部分)`后来聘请了主要的`Hibernate`开发人员以进一步开发它.

2005年,`Hibernate 3.0`发布.包括新的`拦截器/回调架构` `用户定义的过滤器`和 `JDK 5.0注释(元数据功能)`.

截至2010年,`Hibernate 3.5`通过`Core`模块的包装器成为`Java Persistence API 2.0规范的认证实现`,该模块符合 JSR 317 标准.

2011年12月,`Hibernate Core 4.0.0 Final`发布.包括`多租户支持` `ServiceRegistry`的引入等.

2015年9月,`Hibernate ORM 5.0.2 Final`发布.它改进了引导`hibernate-java8` `hibernate-spatial` `Karaf`支持.

2022年10月,`Hibernate ORM 6.1.4 Final`发布.

2024年3月3日,`Hibernate 7.0.0.Alpha2`发布.需要`JDK 17`及以上版本支持.

----

## Spring Framework

[Spring Framework 官网](https://spring.io/projects/spring-framework) 

`Spring`框架是`Java`平台的一个开源的全栈`full-stack`应用程序框架和`控制反转容器`实现.该框架的一些核心功能理论上可用于
`任何Java应用`,`Spring`还为基于Java企业版平台构建的Web应用提供了大量的拓展支持.`Spring`没有直接实现任何的编程模型,但它已经在
`Java`社区中广为流行,基本上完全代替了`EJB(Enterprise JavaBeans)`模型.

2002年10月,第一版由`Rod Johnson`开发,发布在`Expert One-on-One J2EE Design and Development`一书中.

2003年6月,`Spring Framework`第一次发布在`Apache 2.0`许可证下.

2004年3月,发布了里程碑的版本`1.0`

2006年10月,发布`Spring 2.0`.

2009年12月,发布`Spring 3.0`.

2013年12月,发布`Spring 4.0`.

2017年9月,发布`Spring 5.0`.引入了`Spring WebFlux` `高性能` `响应式` `异步`的`Web`框架.

2022年11月,发布`Spring 6.0`.

2024年4月16号,发布`Spring 6.2.0`.

----

## Spring Web Flow

[Spring Web Flow 官网](https://spring.io/projects/spring-webflow)

2004年3月,`Spring Framework`发布后的后续产品`Spring MVC`,基于`Spring`和`MVC`设计模式开发.

2017年9月,发布`Spring 5.0`.引入的`Spring WebFlux`基于`Spring MVC`而开发,是异步非阻塞流式框架.

----

## Netty

[Netty 官网](https://netty.io/)  

2004年,`Trustin Lee(韩国Line公司)`开发了Netty.Netty的性能很高,按照Facebook公司开发小组的测试表明,Netty最高能达到接近百万的吞吐.

2008年10月,Netty3发布.

2013年7月,Netty4发布.

2013年12月,Netty5.0.0.Alpha1发布.

----

## Jenkins

[Jenkins 官网](https://www.jenkins.io/)

`Jenkins`是一个开源`自动化`服务器.它有助于`自动化与构建` `测试和部署`相关的软件开发部分,促进`持续集成和持续交付`.它是一个基于
服务器的系统,在`Servlet`容器(例如Apache Tomcat)中运行.它支持`版本控制工具(Git等)`,并且可以执行基于`Apache Ant` `Apache Maven`
以及任意`shell`脚本和`Windows`批处理命令.

2011年,与`Oracle`发生争执后更名为`Hudson`,`Oracle`分叉了该项目并声称拥有该项目名称的权利.Oracle分支Hudson在捐赠给Eclipse基金会
之前继续开发了一段时间.于2017年2月宣布已过时.

2007年,`Hudson`被认为是`Cruise Control`和`其他开源构建服务器`的更好替代品.

2008年5月的`JavaOne`会议上,该软件荣获开发者解决方案类别的`Duke's Choice`奖.

2010年11月,Hudson社区中出现了有关所使用基础设施的问题,该问题逐渐演变为涉及Oracle管理和控制的问题.主要项目贡献者和Oracle之间进
行了谈判,尽管在许多方面达成了一致,但关键的症结是商标名称`Hudson`.

2011年1月11日,有人发起投票,将项目名称从`Hudson`更改为`Jenkins`.

2011年1月29日,社区投票以压倒性多数通过了该提案,创建了`Jenkins`项目.

2011年2月1日,Oracle表示他们打算继续开发Hudson,并认为Jenkins是一个分叉而不是重命名.因此Jenkins和Hudson作为两个独立的项目继续存
在,各自声称对方是分叉.截至2019年6月,GitHub上的Jenkins组织拥有667个项目成员和约2200个公共存储库,而Hudson在2016年最新更新时有28
个项目成员和20个公共存储库.

2011年,创建者`Kohsuke Kawaguchi`因其在`Hudson/Jenkins`项目上的工作而获得了`O'Reilly`开源奖.

2016年4月20日,版本2发布,默认启用`Pipeline`插件.该插件允许使用基于`Apache Groovy`的领域特定语言编写构建指令.

2017年2月8日,`Jenkins`在`Eclipse`中取代`Hudson`.

2018年3月,针对`Kubernetes`的`Jenkins X`软件项目公开发布,支持不同的云提供商.

2024年,官网出现`Stop The War`宣传,并表明援助乌克兰(`We stand with the people of Ukraine`).

----

## Apache Dubbo

[Dubbo 官网](https://dubbo.apache.org/)  

2008年,`Dubbo`在阿里内部诞生,最初是为了解决阿里内部的微服务架构问题而设计和开发的.

2011年,`Dubbo`开源.

2012年,`Dubbo`发布`2.5.3`版本后停止更新

2014年,当当发布`DubboX`,是基于阿里开源的`Dubbo 2.5.3`版本增加`Rest`协议的`Dubbo`版本

2017年,阿里重启`Dubbo`项目.

2018年,进入`Apache`孵化.

2019年成为`Apache`顶级项目,同时也发布了`Dubbo.js`,`Dubbo-go`等多语言`Dubbo`版本.

2020年,发布3.0往云原生项目发展的战略计划

2024年4月14日,发布dubbo-3.3.0-beta.2

----

## Elastic

[Elastic 官网](https://www.elastic.co/)  
[Elastic Stack](https://www.elastic.co/elastic-stack)  
[logstash](https://www.elastic.co/cn/logstash)  
[kibana](https://www.elastic.co/cn/kibana)  
[beats](https://www.elastic.co/beats)  

`Elasticsearch`是一个基于`Lucene`库的搜索引擎.它提供了一个`分布式` `支持多租户`的`全文搜索引擎`,具有`HTTP Web`界面和
`无模式JSON文档`.`Elasticsearch`是用Java开发的,并根据(源可用)服务器端公共许可证和Elastic许可证进行双重许可,而其他部分[属于专
有(源可用)Elastic许可证.官方客户端支持`Java` `.NET` `C#` `PHP` `Python` `Ruby` 和`许多其他语言`.根据`DB-Engines`排名,
`Elasticsearch`是最受欢迎的企业搜索引擎.

2010年2月8日,`Shay Banon`发表了一篇博客,说他基于`Lucene`开发了一个分布式搜索引擎.

2010年5月14日发布,第一个可以查询到发版信息的版本.

2014年2月12日,发布1.0.0.

2015年10月28日,发布2.0.0.

2016年10月26日,发布5.0.0.

2017年11月14日,发布6.0.0.

2019年4月10日,发布7.0.0.

2022年2月10日,发布8.0.0 .

----

## Apache Kafka

[Apache Kafka 官网](https://kafka.apache.org/)  

`Apache Kafka`是一个`分布式` `事件存储`和`流处理`平台.它是由`Java`和`Scala`编写的开源系统.该项目旨在提供一个`统一` `高吞吐量`
`低延迟`的平台来`处理实时数据源`.

2011年,Kafka由`LinkedIn`公司开发,随后开源.`Jay Kreps` `Neha Narkhede`和`Jun Rao`帮助共同创建了`Kafka`.
 
2012年10月23,孵化与Apache,`Jay Kreps`选择以作家`Franz Kafka`的名字命名该软件.

----

## Apache Pulsar

[Apache Pulsar 官网](https://pulsar.apache.org/)

2012年,Pulsar诞生,最初的目的是为在`Yahoo`内部,整合其他消息系统,构建`统一逻辑` `支撑大集群`和`跨区域`的`消息平台`.当时的其他消
息系统(包括Kafka),都不能满足`Yahoo`的需求,比如`大集群多租户` `稳定可靠的IO服务质量` `百万级 Topic` `跨地域复制`等,因此`Pulsar`
应运而生.

`Apache Pulsar`作为Apache软件基金会顶级项目,是下一代云原生分布式消息流平台,集`消息` `存储` `轻量化函数计算`为一体,采用
`计算与存储分离`架构设计,支持`多租户` `持久化存储` `跨区域复制` `具有强一致性` `高吞吐` `低延迟`及`高可扩展性`等`流数据存储特性`.

----

## Apache Spark

[Apache Spark 官网](https://spark.apache.org/)

2009年,`Matei Zaharia`在加州大学伯克利分校的`AMPLab`启动`Spark`.

2010年,在`BSD`许可证下开源.

2013年,该项目被捐赠给Apache软件基金会,并将其许可证切换为`Apache 2.0`.

2014年2月,`Spark`成为Apache顶级项目.

2014年11月，`Spark`创始人`M. Zaharia`的公司`Databricks`创造了使用`Spark`进行`大规模排序的新世界纪录`.

2015年,Spark拥有超过1000名贡献者,使其成为Apache软件基金会中最活跃的项目之一,也是最活跃的开源大数据项目之一.

2023年09月09日,`Spark`发布`3.5.0`版本.

----

## Apache Hadoop

[Apache Hadoop 官网](https://hadoop.apache.org/)

`Apache Hadoop`是一个开源软件实用程序的集合,有助于使用由多台计算机组成的网络来解决涉及大量数据和计算的问题.它提供了一个使用
`MapReduce`编程模型分布式存储和处理大数据的软件框架.`Hadoop`最初是为由商用硬件构建的计算机集群而设计的,目前仍然很常见.此后它也
被用于高端硬件集群.`Hadoop`中的所有模块的设计都基于这样一个基本假设:硬件故障很常见,并且应该由框架自动处理.

根据其联合创始人`Doug Cutting`和`Mike Cafarella`的说法,`Hadoop`的起源是2003年10月发表的Google文件系统论文.这篇论文催生了Google
的另一篇论文`MapReduce：简化数据处理`关于大集群.

开发始于`Apache Nutch`项目,但于2006年1月转移到新的`Hadoop`子项目.`Doug Cutting`曾在Yahoo工作.当时,以他儿子的玩具大象命名.
之后从`Nutch`中分解出来的初始代码包括大约5000行HDFS代码和大约6000行MapReduce代码.

2006年3月,`Owen O'Malley`成为第一个添加到Hadoop项目的提交者. 

2006年4月,`Hadoop 0.1.0`发布. 

2007年,Hadoop分布式文件系统的第一个设计文档由`Dhruba Borthakur`编写.

2012年10月12日,1.0.4版本发布.

2013年08月23日,2.0.6-alpha版本发布.

2018年05月31日,3.0.3版本发布.

2024年5月17日,3.4.0版本发布.

----

## Apache Hive

[Apache Hive 官网](https://hive.apache.org/)

`Apache Hive`是一个数据仓库软件项目.它构建在`Apache Hadoop`之上,用于提供数据查询和分析.`Hive`提供类似`SQL`的接口来查询存储在
与`Hadoop`集成的各种数据库和文件系统中的数据.传统的`SQL`查询必须在`MapReduce Java API`中实现,以执行`SQL`应用程序并对分布式数
据进行查询.

----

## Apache HBase

[HBase 官网](https://hbase.apache.org/)

`HBase`是一个开源的`非关系型` `分布式`数据库,仿照`Google`的`Bigtable`并用`Java`编写.它是作为`Apache Software Foundation`的
`Apache Hadoop`项目的一部分开发的,运行在`HDFS`或`Alluxio`之上,为`Hadoop`提供类似`Bigtable`的功能.也就是说,它提供了一种存储大
量稀疏数据.

----

## Apache Druid

[Apache Druid 官网](https://druid.apache.org/)

`Apache Druid`是一个用`Java`编写的`面向列`的 `开源`的 `分布式` `数据存储`.Druid旨在快速摄取大量事件数据,并在数据之上提供低延迟
查询.Druid这个名字来源于很多角色扮演游戏中的变形德鲁伊类,以体现系统架构可以通过变形来解决不同类型的数据问题.

----

## Apache Kylin

[Apache Kylin 官网](https://kylin.apache.org/)

`Apache Kylin`是一个开源分布式分析引擎,旨在在`Hadoop`和`Alluxio`上提供SQL接口和多维分析`OLAP`,支持超大数据集.它最初由`eBay`开
发,现在是`Apache`软件基金会的一个项目.

2013年,`Kylin`项目在`eBay`位于中国上海的研发中心启动.

2014年10月,`Kylin v0.6`在`github`开源,名称为`KylinOLAP`.

2014年11月,`Kylin`加入`Apache`软件基金会孵化器.

2015年12月`Apache Kylin`成为顶级项目.

2016年3月,`Apache Kylin`的创建者创立了`Kyligence, Inc.`.`Kyligence`为本地和基于云的数据集提供基于`Apache Kylin`的商业分析平台.

----

## Apache Flink

[Apache Flink 官网](https://flink.apache.org/)

`Apache Flink`是由Apache软件基金会开发的`开源` `统一`的`流处理和批处理`框架.`Apache Flink`的核心是一个用`Java`和`Scala`编写的
分布式流数据流引擎.`Flink`以`数据并行`和`流水线(因此称为任务并行)`方式执行任意数据流程序.Flink的流水线运行时系统可以执行
批量/批处理和流处理程序,此外,Flink的运行时本身支持迭代算法的执行.

2010年,由`Volker Markl`领导的研究项目`平流层:云端信息管理`(由德国研究基金会(DFG)资助)与`柏林工业大学` `洪堡大学`合作启动
`柏林和波茨坦哈索普拉特纳研究所`.Flink始于`Stratosphere`分布式执行引擎的一个分支.

2014年3月成为Apache孵化器项目.

2014年12月,Flink被接受为Apache顶级项目.

2016年05月11日,发布1.0.3.

2024年3月21日,发布1.8.0.

----

## Apache Storm

[Apache Storm 官网](https://storm.apache.org/)

`Apache Storm`是一个主要用`Clojure`编程语言编写的分布式流处理计算框架.该项目最初由`Nathan Marz`和`BackType`团队创建,在被
`Twitter`收购后开源.它使用自定义创建的`喷口`和`螺栓`来定义信息源和操作,以允许对流数据进行`批量` `分布式`处理.

----

## Apache Pig

[Apache Pig 官网](https://pig.apache.org/)

`Apache Pig`是一个高级平台,用于创建在`Apache Hadoop`上运行的程序.该平台的语言称为`Pig Latin`.`Pig`可以在`MapReduce` 
`Apache Tez`或`Apache Spark`中执行其`Hadoop`作业.`Pig Latin`将`Java MapReduce`惯用法的编程抽象为一种表示法,这使得`MapReduce`
编程变得更高层次,类似于关系数据库管理系统的SQL编程.`Pig Latin`可以使用用户定义函数(UDF)进行扩展,用户可以用`Java` `Python`
`JavaScript` `Ruby`或`Groovy`编写这些函数,然后直接从该语言调用.

2006年,`Apache Pig`最初是在`Yahoo Research`开发的,目的是让研究人员能够以一种特殊的方式在非常大的数据集上创建和执行`MapReduce`
作业.

2007年,它被移入`Apache`软件基金会.

2008年12月05日,`0.1.1`发布.

2017年06月19日,`0.17.0`发布.

----

## Apache Mahout

[Apache Mahout 官网](https://mahout.apache.org/)

`Apache Mahout`是`Apache`软件基金会的一个项目,旨在免费实现主要关注线性代数的分布式或可扩展的`机器学习算法`.过去,许多实现都使用
`Apache Hadoop`平台,但现在主要集中在`Apache Spark`上.`Mahout`还提供用于常见数学运算(专注于线性代数和统计)和原始`Java`集合的
`Java/Scala`库.`Mahout`是一项正在进行中的工作,已经实施了许多算法.

2009年04月07日,`0.1`发布

2020年10月07日,`14.1`发布

## Spring Boot

[Spring Boot 官网](https://spring.io/projects/spring-boot)  

`Spring Boot`是一个开源`Java`框架,用于以最少的工作量编写`独立的` `生产级`的`基于Spring的`应用程序,`Spring Boot`是Spring Java
平台的`约定优于配置`扩展,旨在帮助在创建基于Spring的应用程序时最大程度地`减少配置`问题.

2013年`vmware` `emc`和`通用资本`合资成立了一家公司,叫`GoPivotal`,`springsource`是一个筹码,做`大数据平台`.

2014年4月,发布全新开源的轻量级框架的第一个`SpringBoot`版本.

2016年10月28日,`SpringBoot 1.5`版本发布.

2018年3月21日,`SpringBoot 2.0`版本发布.

2022年11月24号,`SpringBoot 3.0`版本发布.要求使用`JDK 17`作为最低版本.

----

## Spring Cloud

[Spring Cloud 官网](https://spring.io/projects/spring-cloud)  

2012年铁道部针对12306系统进行了17种技术选型,经过数轮PK,最终选择了`Pivotal GemFire`分布式解决方案对系统进行技术改造.根据改造后的
运行数据记录显示:
- 系统在只采用十几台`X86`服务器的情况下实现了以前数十台小型机的余票计算和查询能力.
- 单次查询的最长时间从`15`秒下降到`0.2`秒以下,缩短了`75`倍以上.
- 订单查询系统从以前只能支持`300-400`个查询/秒,提升至支持每秒上`万次`的并发查询,高峰期间达到`2.6万`个查询/秒的吞吐量,并且单次查
询速度仍可保持在`20`毫秒左右,整个系统性能显著提高.
- 更关键的是,新的技术架构可以`按需弹性动态扩展`,当并发量增加时,可以通过`动态增加X86服务器`来应对,保持毫秒级的响应时间.

2016年1月,`Spring Cloud`发布了第一个版本,名为`Angel.SR51`,为开发人员提供了`基于Spring框架`的`微服务解决方案`.后续依次发布了
`Brixton` `Camden` `Dalston` `Finchley` `Greenwich`和`Hoxton`版本

2020年,这一年对`Spring Cloud`来说是一个重要的转折点.随着`Netflix`宣布其核心微服务组件进入维护状态,`Spring Cloud Netflix`也进入
维护模式,并在后续版本中移除了相关的`Netflix OSS`组件.

2020年12月22日,Spring官方宣布了`Spring Cloud 2020.0(code name: Ilford)`版本的正式发布.这是第一个使用新的版本号命名方案的`Spring Cloud`发行版本.

2022年,`2022.0(code name: Kilburn)`版本发布,这个版本建立在`Spring Framework 6.0`和`Spring Boot 3.0`之上,因此它对JDK的最低要求是`Java 17`,对J2EE
的最低要求是`Jakarta EE 9`.

2023年12月06日,`2023.0(code name: Leyton)`版本发布.

----

## Apache Tomcat

[Apache Tomcat](https://tomcat.apache.org/)

`Apache Tomcat`是`Jakarta Servlet` `Jakarta Expression Language`和`WebSocket`技术的免费开源实现.它提供了一个纯`Java HTTP Web`
服务器环境,Java代码也可以在其中运行.因此,它是一个`Java Web`应用程序服务器,尽管不是完整的 `JEE` 应用程序服务器.

1998年11月,Tomcat发布,`Sun Microsystems`的软件架构师`James Duncan Davidson`的`servlet`参考实现。

1999年,合并捐赠的`Sun Java Web Server`代码和`ASF`,并实现`Servlet 2.2`和`JSP 1.1`规范.

2002年09月06日,发布4.0,支持`Servlet 2.3`和`JSP 1.2`规范.

2003年12月03日,发布5.0,支持`Servlet 2.4` `JSP 2.0`和`EL 1.1`规范.

2007年02月28日,发布6.0,支持`Servlet 2.5` `JSP 2.1`和`EL 2.1`规范.

2011年01月14日,发布7.0,支持`Servlet 3.0` `JSP 2.2` `EL 2.2`和`WebSocket`规范.

2014年06月25日,发布8.0,支持`Servlet 3.1` `JSP 2.3`和`EL 3.0`规范.

2016年06月13日,发布8.5,添加了对`HTTP/2` `OpenSSL for JSSE` `TLS`虚拟主机和`JASPIC 1.1`的支持.

2018年01月18日,发布9.0,支持`Servlet 4.0`规范.

2021年02月02日,发布10.0,支持`Servlet 5.0` `JSP 3.0` `EL 4.0` `WebSocket 2.0`和`Authentication 2.0`规范.

2022年09月26日,发布10.1,支持`Jakarta Servlet 6.0` `JSP 3.1` `EL 5.0` `WebSocket 2.1`和`JASPIC 3.0`规范.

2024年05月08日,发布11.0,支持`Jakarta Servlet 6.1` `JSP 4.0`.

----

## Eclipse

[Eclipse](https://www.eclipse.org/)

Eclipse是一款跨平台开源集成开发环境(IDE).最初主要用来Java语言开发,目前亦有人通过插件使其作为`C++` `Python` `PHP`等其他语言的开发工具.

Eclipse的本身只是一个框架平台,但是众多插件的支持,使得Eclipse拥有较佳的灵活性,所以许多软件开发商以`Eclipse`为框架开发自己的`IDE`.

Eclipse最初是由IBM公司开发的替代商业软件Visual Age for Java的下一代IDE开发环境.

2001年11月,贡献给开源社区,现在它由非营利软件供应商联盟Eclipse基金会(Eclipse Foundation)管理.

2003年,`Eclipse 3.0`选择OSGi服务平台规范为运行时架构.

2007年6月,稳定版3.3发布.

2008年6月,发布代号为Ganymede的3.4版.

2009年6月,发布代号为Galileo的3.5版.

2010年6月发布代号为Helios的3.6版. 

2011年6月发布代号为Indigo的3.7版.

2012年6月发布代号为Juno的4.2版.

2013年6月发布代号为Kepler的4.3版.

2014年6月发布代号为Luna的4.4版.

2015年6月发布代号为Mars的4.5版.

2020年12月,发布4.18版.

----

## IntelliJ IDEA

[IntelliJ IDEA](https://www.jetbrains.com/idea/)

`IntelliJ IDEA`是一个用Java编写的集成开发环境(IDE),用于开发用 `Java` `Kotlin` `Groovy`和其他`JVM编写`的计算机软件基础语言.
它由`JetBrains`(以前称为 IntelliJ)开发,可作为`Apache 2`许可的社区版本和专有商业版本.

2001年1月,发布`IntelliJ IDEA`的第一个版本,是首批集成了`高级代码导航`和`代码重构`功能的`Java IDE`之一.

2009年,`JetBrains` 在开源 `Apache License 2.0`下发布了 `IntelliJ IDEA` 的源代码. `JetBrains`还开始分发 `IntelliJ IDEA` 的
有限版本,其中包含社区版的开源功能.商业终极版提供了额外的功能,并且仍然需要付费.

2010年,InfoWorld报告中,`IntelliJ`在四个顶级 `Java` 编程工具(`Eclipse` `IntelliJ IDEA` `NetBeans`和`JDeveloper`)中获得了最高
的测试中心分数.

2014年12月,`Google` 发布了`Android Studio 1.0` 版本,这是一款基于开源社区版本的`Android`应用开源`IDE`.基于 `IntelliJ` 框架的
其他开发环境包括`AppCode` `CLion` `DataGrip` `GoLand` `PhpStorm` `PyCharm` `Rider` `RubyMine` `WebStorm`和`MPS`.

2020年9月,华为宣布并发布了`DevEco Studio 1.0`版本,这是一款用于`HarmonyOS`应用程序开发的开源`IDE`,基于`Jetbrains IntelliJ IDEA`
以及适用于`Windows`和`macOS`的华为`SmartAssist`.

----

## GraalVM

[GraalVM 官网](https://www.graalvm.org/) 

这是一款通用型虚拟机,一个即时(JIT)编译器,允许在单个应用程序中混合来自任何编程语言的代码,称为`多语言应用程序`.

2019年5月,发布第一个生产就绪版本`GraalVM 19.0` 

待研究...

----

## 近期

2017年,`Oracle`宣布`Java`将进入新的发布周期,每六个月推出一个新版本,随着新的发布周期,`Oracle`还宣布了他们构建和发布`Java`的方式
的重大转变.专有许可的`Oracle JDK`被`OpenJDK`二进制文件`取代`,成为`Oracle`分发的主要版本工件.

根据`Java首席架构师Mark Reinhold`的说法,由于`Java`平台模块系统(Jigsaw),`Java 9`遇到了重大延迟.根据新的发布计划,`Oracle`提出了
严格的基于时间的发布,称为特性发布.这些将在每年的`3月`和`9月`出现,这些版本不会延迟以适应主要功能.新功能在功能完成之前不会合并到发
布源代码控制库中,如果它们错过了一个版本,则必须将它们重新定位到下一个或更晚的版本.

此后根据实际应用程序进行了多轮社区反馈,这个过程不仅让`Java开发人员`有机会在这些功能最终确定之前试验这些功能,而且还纳入了关键反馈,
从而产生了两个真正满足社区需求的坚如磐石的`JEP(JDK Enhancement Proposals)(增强建议,相当于需求单)`.

今天,Java是众所周知的世界上最通用的编程语言之一.就`平台` `技术`和`经济领域`而言,它`几乎无处不在`,数十亿的`Android`手机都在运行
`Java`,许多`游戏`都是用`Java开发和维护`的,更不用说Java在`企业级服务器应用程序上的广泛使用`.全球对`合格和经验丰富的Java开发人员`
的`需求不断增加`,`人工智能` `大数据` `物联网` `区块链`等在内的`新趋势`依旧非常依赖`Java`.

`Java`也面临着来自`JavaScript(世界上最流行的语言,全球有超过1200万JS程序员)`,`Python(作为初学者的编程语言而迅速流行,通常在学龄期学习)`
和`Kotlin(一种开源编程语言,通常被宣传为Java的替代品)`.

但`Java`的许多特性使其成为企业应用程序开发的最常见选择:
- 可扩展性和可靠性: Java是一种非常高效且可扩展的语言,即使在高工作负载下也能够提供强大的性能.
- 编码标准和文档: `OOP(Object Oriented Programming)`开发的指定标准以及涉及Java开发各个方面的丰富可用文档
- 大量可用的库: 数以万计的各种`Java`库的可用性也是企业的一个重要因素,因为它可以使开发过程`更快` `更便宜`
- JVM 和可移植性: Java虚拟机的存在使得用Java编写的应用程序可以在各种其他平台上运行
- JVM支持编译成字节码的任何语言: 如`Kotlin` `Groovy` `JRuby` `Jython` `Scala`等.
- 高频率的迭代周期: 自2018年以来,Java处于6个月的新发布周期
- 新兴技术: 许多最流行的领域和技术,包括`人工智能(AI)(artificial intelligence)` `物联网(IoT)(Internet of things)`
`区块链(blockchain)` `大数据(Big data)`等

纵观Java发展的几十年:
- 在商业化与万维网时代,Java Applet直接嵌入到网页中,揽获大量用户.
- 在互联网极速发展之时,Java已可作为服务器语言使用.
- 在Java大规模应用期间,大量开源社区与伙伴的加入,提供了优秀的开发框架和理论支持.
- 社交媒体与移动互联网的崛起,又能作为app开发语言使用.
- 云计算与大数据兴起,又有大量优秀大数据框架和编程语言的加入.
- 如今机器学习和人工智能时代,Java又会如何发展呢?

## Java迭代过程中的几个概念

## JCP

`JCP(Java Community Process)(Java社区标准过程)`,成立于1998年,是使有兴趣的各方参与定义Java的特征和未来版本的正式过程.成为`JCP`会
员是唯一可以提交`JSR`供审查的唯一方式.

## JSR

`JSR(Java Specification Requests)(Java规范请求)`,`JCP`使用`JSR`作为正式规范文档,描述被提议加入到`Java`体系中的的规范和技术.
使用`JSR`圈定标准范围以后就可以提供给JDK团队成员进行开发.圈定的范围通常是Java相关的,同时又不会成为`Java核心技术(Java SE/EE Core)`
的一部分,一个标准的`JSR`课题通常是一个相对成熟的技术.`JSR`通常以一个编号指定,`JSR-153 Enterprise Java Beans 2.1` 
`241 The Groovy Programming Language`

## JLS

`JLS(Java Language Specification)(Java语法规范)`,用指出Java的语法标准和一些规则,这些规则包括了合规及不合规程序的说明.规范同时
指出了程序的含义并说明了运行后将发生什么.

## JEP

`JEP(JDK Enhancement Proposal)(JDK增强建议)`,JEP是一个JDK核心技术相关的增强建议文档.JEP用于探索新的一些想法,一般比JSR更为早期,
用于前期的探索,是用于收集Java Development Kit和OpenJDK增强的提案.

## 参考

- [MyBatis](https://mybatis.org/mybatis-3/) 
- [MyBatis wiki](https://en.wikipedia.org/wiki/Apache_iBATIS)
- [Hibernate](https://hibernate.org/) 
- [Hibernate wiki](https://en.wikipedia.org/wiki/Hibernate_(framework\))
- [Netty](https://netty.io/)  
- [Jenkins](https://www.jenkins.io/)
- [Jenkins_ wiki](https://en.wikipedia.org/wiki/Jenkins_(software\))
- 
- [Spring Framework](https://spring.io/projects/spring-framework)
- [Spring Framework wiki](https://zh.wikipedia.org/wiki/Spring_Framework)
- [Spring Web Flow](https://spring.io/projects/spring-webflow)
- [Spring Boot](https://spring.io/projects/spring-boot)  
- [Spring Cloud](https://spring.io/projects/spring-cloud)  
- 
- [Elastic](https://www.elastic.co/)
- [Elastic Stack](https://www.elastic.co/elastic-stack)
- [Elastic logstash](https://www.elastic.co/cn/logstash)
- [Elastic kibana](https://www.elastic.co/cn/kibana)
- [Elastic beats](https://www.elastic.co/beats)
- [Elasticsearch wiki](https://en.wikipedia.org/wiki/Elasticsearch)
- 
- [Apache Struts](https://struts.apache.org/index.html)
- [Apache Struts wiki](https://zh.wikipedia.org/wiki/Struts)
- [Apache Kafka](https://kafka.apache.org/)  
- [Apache Kafka wiki](https://en.wikipedia.org/wiki/Apache_Kafka)  
- [Apache Spark](https://spark.apache.org/)
- [Apache Spark wiki](https://en.wikipedia.org/wiki/Apache_Spark)
- [Apache Dubbo](https://dubbo.apache.org/)  
- [Apache Pulsar](https://pulsar.apache.org/)
- 
- [Apache Hadoop](https://hadoop.apache.org/)
- [Apache Hadoop wiki](https://en.wikipedia.org/wiki/Apache_Hadoop)
- [Apache Flink](https://flink.apache.org/)
- [Apache Flink wiki](https://en.wikipedia.org/wiki/Apache_Flink)
- [Apache Storm](https://storm.apache.org/)
- [Apache_Storm wiki](https://en.wikipedia.org/wiki/Apache_Storm)
- [Apache Hive](https://hive.apache.org/)
- [Apache HBase](https://hbase.apache.org/)
- [Apache Druid](https://druid.apache.org/)
- [Apache Kylin](https://kylin.apache.org/)
- [Apache Kylin wiki](https://en.wikipedia.org/wiki/Apache_Kylin)
- [Apache Pig](https://pig.apache.org/)
- [Apache Mahout](https://mahout.apache.org/)
- [Apache Tomcat](https://tomcat.apache.org/)
- [Apache Tomcat wiki](https://en.wikipedia.org/wiki/Apache_Tomcat)
- [Eclipse](https://www.eclipse.org/)
- [Eclipse wiki](https://en.wikipedia.org/wiki/Eclipse_(software\))
- [IntelliJ IDEA](https://www.jetbrains.com/idea/)
- [IntelliJ IDEA wiki](https://en.wikipedia.org/wiki/IntelliJ_IDEA)
- 
- [GraalVM](https://www.graalvm.org/) 
- [Java](https://www.java.com/zh-CN/about/)
- [zgc](https://openjdk.org/projects/zgc/)
- 
- [TIOBE](https://www.tiobe.com/tiobe-index/)
- [SlashData](https://www.slashdata.co/)
- [Salary](https://www.payscale.com/research/US/Job=Java_Developer/Salary)

[返回顶部](#主要内容)

