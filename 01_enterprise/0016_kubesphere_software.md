# KubeSphere 软件安装

# 主要内容

> [简介](#简介)  
> [MySQL](#mysql)  
> [Redis](#redis)  
> [ElasticSearch](#elasticsearch)  

## 简介

KubeSphere 是一个多租户容器平台,在安装软件前先创建企业空间和项目

## 创建企业空间

- 填写基本信息
- 选择集群

## 企业空间里创建项目

- 填写基本信息

## MySQL

#### 创建配置

- 配置
  - 配置字典
  - 创建
  - 填写名称 mysql-conf
  - 添加数据 my.cnf
  
```text
[client]
default-character-set=utf8mb4
 
[mysql]
default-character-set=utf8mb4
 
[mysqld]
init_connect='SET collation_connection = utf8mb4_unicode_ci'
init_connect='SET NAMES utf8mb4'
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```

#### 创建存储类

- 存储
  - 存储类 
  - 创建存储类
  - 基本信息 mysql-pvc
  
```shell
# 查看存储类型
kubectl get sc
```

#### 创建有状态副本集

- 应用负载
  - 工作负载
  - 有状态副本集
  - 创建
  - 名称 mysql
  - 容器组设置 mysql:5.7 -> 使用默认端口 -> 环境变量 MYSQL_ROOT_PASSWORD 123456 -> 同步主机时区
  - 存储设置 添加存储卷 mysql-pvc /var/lib/mysql (读写)
  - 存储设置 配置字典 mysql-conf /etc/mysql/conf.d (只读)
  - 创建
  - 有状态副本集 -> 名称 -> 查看事件 -> 容器组日志/终端 -> 查看配置 -> 修改字典后重新查看配置 -> 重启生效修改配置

- 应用负载
  - 服务 
  - 创建
  - 名称 mysql-node
  - 指定工作负载 有状态副本集 mysql
  - 端口 3306 3306
  - 高级设置 外部访问 NodePort
  - 服务器是通过 iptable 规则找到 NodePort

```shell
kubectl get ns
kubectl get services
kubectl get svc
sudo iptables -L -n -t nat
```

## Redis

#### 创建配置

- 配置
  - 配置字典
  - 创建
  - 基本信息 名称 redis-conf
  - 数据设置 redis.conf

```text
appendonly yes
port 6379
bind 0.0.0.0
```

#### 创建有状态副本集

- 应用负载
  - 工作负载
  - 有状态副本集
  - 创建
  - 基本信息 名称 redis
  - 容器组设置 redis:7.2
  - 容器组设置 使用默认端口镜像
  - 容器组设置 启动命令 参数 redis-server /etc/redis/redis.conf
  - 容器组设置 同步主机时区
  - 存储设置 存储类 redis-pvc /data (读写)
  - 存储设置 配置卷 redis-conf /etc/redis (只读)
  - 创建
  - 服务 创建
  - 基本信息 redis-node
  - 服务设置 指定工作负载 端口号 6379
  - 高级设置 外部访问 NodePort
  - 测试 副本删除重启 数据仍在

每次创建存储卷,每启动一个副本则持久卷声明多一个

## ElasticSearch

#### 创建配置
- 配置
  - 配置字典
  - 创建
  - 基本信息 名称 es-conf
  - 数据设置 elasticsearch.yml

```text
cluster.name: "docker-cluster"
network.host: 0.0.0.0
```
  - 数据设置 jvm.options

```text
-XX:+UseG1GC
-Djava.io.tmpdir=${ES_TMPDIR}
20-:--add-modules=jdk.incubator.vector
-XX:+HeapDumpOnOutOfMemoryError
-XX:+ExitOnOutOfMemoryError
-XX:HeapDumpPath=data
-XX:ErrorFile=logs/hs_err_pid%p.log
-Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,level,pid,tags:filecount=32,filesize=64m
```

[jvm.options配置文件](https://github.com/elastic/elasticsearch/blob/main/distribution/src/config/jvm.options)

#### 创建有状态副本集

- 应用负载
  - 工作负载 
  - 创建有状态副本集
  - 基本信息 名称 es
  - 容器组设置 镜像 elasticsearch:8.13.4
  - 容器组设置 使用默认端口镜像
  - 容器组设置 环境变量 discovery.type=single-node ES_JAVA_OPTS="-Xms512m -Xmx512m
  - 容器组设置 同步主机时区
  - 存储设置 存储类 es-pvc /usr/share/elasticsearch/data (读写)
  - 存储设置 配置字典 es-conf /usr/share/elasticsearch/config/elasticsearch.yml (只读) elasticsearch.yml(子路径) elasticsearch.yml
  - 存储设置 配置字段 es-conf /usr/share/elasticsearch/config/jvm.options (只读) jvm.options(子路径) jvm.options
  - 创建
  - 基本信息 es-node
  - 服务设置 指定工作负载 端口号 9200
  - 高级设置 外部访问 NodePort
  - 发送 http 测试

```shell
# 重置密码
cd bin/
elasticsearch-reset-password -u elastic
kubectl describe nodes
kubectl get pods -A
kubectl describe nodes
```

## Pod状态分析

当 Kubernetes(k8s)中的 Pod 被标记为 Unschedulable 时,表示 Kubernetes 的调度器 Scheduler 无法找到一个满足 Pod 需求的节点来运行这个 
Pod.以下是可能导致 Pod 处于 Unschedulable 状态的一些常见原因及其相应的解决方法:

资源不足,集群中的节点可能没有足够的 CPU, 内存或其他资源来满足 Pod 的资源请求.
解决方法:检查并扩展集群的资源容量,可以通过增加新的节点或者调整现有节点的资源配额. 调整 Pod 的资源请求和限制,使其更符合集群中可用资源的情况.

Taints 和 Tolerations 不匹配,如果节点上设置了 Taints,而 Pod 没有相应的 Tolerations,那么 Pod 将无法被调度到这些节点上.
解决方法:检查节点的 Taints 和 Pod 的 Tolerations 设置,确保它们相匹配. 可以为 Pod 添加适当的 Tolerations,以允许它们被调度到带有特定 
Taints 的节点上.

亲和性 Affinity 和反亲和性 Anti-Affinity 规则,Pod 可能定义了特定的亲和性或反亲和性规则,而这些规则导致调度器无法在集群中找到合适的节点.
解决方法:审查 Pod 的亲和性和反亲和性规则,确保它们与集群中节点的标签相匹配.可以调整 Pod 的亲和性规则,以放宽调度限制或指定更合适的节点.

存储问题,如果 Pod 请求了特定类型的存储(例如 PersistentVolumes), 并且集群中没有满足要求的存储资源,那么 Pod 将无法被调度.
解决方法:确保集群中有足够的存储资源,并且正确配置了 PersistentVolumes 和 PersistentVolumeClaims.可以创建新的存储资源或调整 Pod 的存储请求.

节点选择器(Node Selector)不匹配,Pod 可能定义了节点选择器,而集群中没有节点的标签与之匹配.
解决方法:检查 Pod 的节点选择器设置,确保它与集群中至少一个节点的标签相匹配.可以修改节点的标签或调整 Pod 的节点选择器。

其他调度约束,Pod 可能还受到其他调度约束,例如优先级,抢占(Preemption)策略等.
解决方法:检查集群的调度策略和配置,确保它们不会阻止 Pod 的调度.根据需要调整调度策略或 Pod 的优先级设置。
```

```shell
k8s 通用命令

# 查看 pod 详细信息命令
kubectl describe pod pod-name -n namespace-name

# 列出所有节点
kubectl get nodes

# 查看指定节点信息
kubectl describe node 指定节点

# 列出所有容器
kubectl get pod -A

# 查看容器信息
kubectl describe pod 容器名

# 查看容器日志
kubectl logs 容器名

# 列出所有服务
kubectl get services
```

----

路漫漫兮修远兮,我将上下而求索.

[返回顶部](#主要内容)

