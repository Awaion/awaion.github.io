# KubeSphere 安装

# 主要内容

> [简介](#简介)  
> [前置配置](#前置配置)  
> [社区版](#社区版)  
> [企业版](#企业版)  
> [Docker 安装](#docker-安装)  
> [其它](#其它)  
> [Web 页面简介](#web-页面简介)  
> [术语](#术语)  

## 简介

KubeSphere 是在 Kubernetes 之上构建的面向云原生应用的分布式操作系统,完全开源,支持多云与多集群管理,提供全栈的 IT 自动化运维能力,简化企业的 
DevOps 工作流.它的架构可以非常方便地使第三方应用与云原生生态组件进行即插即用 (plug-and-play) 的集成.

作为全栈的多租户容器平台,KubeSphere 提供了运维友好的向导式操作界面,帮助企业快速构建一个强大和功能丰富的容器云平台.KubeSphere 为用户提供构建企
业级 Kubernetes 环境所需的多项功能,例如多云与多集群管理,Kubernetes 资源管理,DevOps,应用生命周期管理,微服务治理(服务网格),日志查询与收集,
服务与网络,多租户管理,监控告警,事件与审计查询,存储管理,访问权限控制,GPU 支持,网络策略,镜像仓库管理以及安全管理等.

KubeSphere 还开源了 KubeKey 帮助企业一键在公有云或数据中心快速搭建 Kubernetes 集群,提供单节点,多节点,集群插件安装,以及集群升级与运维.

KubeSphere 官网: https://kubesphere.io/zh/

KubeSphere 企业收费版: https://kubesphere.com.cn/

![架构图](./images/0015_kubesphere/architecture.png)

## 前置配置

```shell
# 机器配置 4核 10G 40G,VMWare 全新 Ubuntu 22.04 LTS
sudo ping www.github.com

# 设置主机名
sudo hostnamectl set-hostname k8s-master

# 重启
reboot

# 切换 root 用户
sudo passwd root
su root

# 根目录下创建 app 目录
mkdir /app

# 进入 app 目录
cd /app/

# 更新 Advanced Package Tool
sudo apt update

# 查看 apt 软件源列表 默认有清华源
sudo cat /etc/apt/sources.list

# 安装 curl net-tools openssh git
sudo apt install curl net-tools openssh-server git -y

# 获取 kubekey 安装包
获取 kubekey-v3.1.1-linux-amd64.tar.gz 安装包 https://github.com/kubesphere/kubekey/releases 

# 管理员权限打开文件管理器粘贴
sudo nautilus

# 文件解压
sudo tar -xzf kubekey-v3.1.1-linux-amd64.tar.gz

# 安装依赖
sudo apt install socat conntrack ebtables ipset ipvsadm -y
crictl etcd

## 查看可安装版本
sudo ./kk version --show-supported-k8s

## 临时环境变量 重启后会消失
export KKZONE=cn

# 查看环境变量
env | grep KKZONE
```

## 社区版

```shell
# 社区版 All-in-one 一键安装 查看下环境变量是否生效 或者分开安装 k8s kubesphere
sudo ./kk create cluster --with-kubernetes v1.22.12 --with-kubesphere v3.4.1

# 检查安装结果
sudo kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l 'app in (ks-install, ks-installer)' -o jsonpath='{.items[0].metadata.name}') -f

# 清除一下污点(单机一般不会有污点)
sudo kubectl get nodes 
sudo kubectl describe nodes k8s-master | grep Taints
sudo kubectl taint node k8s-master 污点名-

# 查看 kubelet 日志
journalctl -xeu kubelet

# /etc/docker/daemon.json 配置镜像源,并且 ping 域名是否可解析
sudo ping registry.cn-beijing.aliyuncs.com
sudo ping rsk59qvc.mirror.aliyuncs.com
sudo docker images
sudo kubectl describe pod ks-console-768dbcdf9c-6bqsx -n kubesphere-system

# 卸载集群
sudo ./kk delete cluster
```

[helm安装脚本](./images/0015_kubesphere/get_helm.sh)

[kubekey安装脚本](./images/0015_kubesphere/get_kk.sh)

[docker安装脚本](./images/0015_kubesphere/get_docker.sh)

[kubesphere删除脚本](./images/0015_kubesphere/kubesphere-delete.sh)

## 企业版

```shell
# 企业版 快速创建一个 Kubernetes 集群 KKZONE=cn 很重要,国内服务器没有无法安装成功
sudo ./kk create cluster --skip-pull-images --with-local-storage  --with-kubernetes v1.25.4 --container-manager containerd  -y

# 每隔 1 秒查看容器信息,等状态是 Running
sudo watch -n 1 kubectl get pods -A

# 列出 helm 仓库
helm repo list

# Kubernetes 集群可用后, 执行以下命令通过 helm 安装 KubeSphere Core
sudo helm upgrade --install -n kubesphere-system --create-namespace ks-core https://charts.kubesphere.io/main/ks-core-0.4.0.tgz --set apiserver.nodePort=30881 --debug --wait

# 看系统是否起来
sudo kubectl get pod -n kubesphere-system
```

## Docker 安装

```shell
# 下载 docker 安装脚本
curl -fsSL https://get.docker.com -o get_docker.sh

# 运行 docker 安装脚本
sh get_docker.sh

# 开机自启动 docker 服务
sudo systemctl enable docker

# 给 docker 添加镜像仓库和 cgroup (资源限制,优先级管理,资源统计,控制访问)
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://rsk59qvc.mirror.aliyuncs.com"],
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

# 查看配置
sudo cat /etc/docker/daemon.json

# 重启 docker
sudo systemctl restart docker

# 查看 docker 信息
sudo docker info

# 验证镜像部署是否正常
sudo docker run hello-world

# Kubernetes 常用命令
# 集群信息
kubectl cluster-info

# 查看所有 pod
kubectl get pods -A

# 查看节点
kubectl get nodes -A

# 查看所有服务
kubectl get svc -A

# 查看指定命名空间 pod 详情
kubectl describe pods -n devops01jmn9p demo002-675c968d59-gbbsw

# 创建一个 deployment
kubectl create deployment <deployment-name> --image=<image-name>

# 缩放 deployment
kubectl scale deployment <deployment-name> --replicas=<num-replicas>

# 将服务暴露为 NodePort
kubectl expose deployment <deployment-name> --type=NodePort --port=<port-number>

# 更新 deployment 的镜像
kubectl set image deployment/<deployment-name> <container-name>=<new-image-name>

# 删除 deployment
kubectl delete deployment <deployment-name>
```

## 其它

```shell
# 停止防火墙服务
sudo systemctl stop firewalld 
# 禁止防火墙开机自启   
sudo systemctl disable firewalld 

# 检查 DNS 解析文件
sudo cat /etc/resolv.conf
sudo cat /etc/systemd/resolved.conf
sudo cat /run/systemd/resolve/resolv.conf
# 添加 DNS 解析服务器 阿里
sudo vi /etc/systemd/resolved.conf
DNS=223.5.5.5
# 重启域名解析
sudo systemctl restart systemd-resolved
# 查看解析服务状态
sudo systemctl status systemd-resolved

# 查看 kubelet 日志
journalctl -xeu kubelet

# 查看 containerd 配置,是否有国内镜像
sudo cat /etc/containerd/config.toml
sandbox_image = "registry.aliyuncs.com/google_containers/pause:3.9"
sandbox_image = "registry.cn-beijing.aliyuncs.com/kubesphereio/pause:3.9"

# 查看 kubelet 配置
sudo cat /var/lib/kubelet/config.yaml

# 列出 helm 仓库
helm repo list
# 配置仓库,默认仓库在国外,访问慢
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add stable http://mirror.azure.cn/kubernetes/charts
helm repo add aliyun https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
helm repo add incubator https://charts.helm.sh/incubator
helm repo add kaiyuanshe http://mirror.kaiyuanshe.cn/kubernetes/charts/
# 手动更新仓库(一般都会自动更新,不生效时手动更新)
helm repo update

# 部署
mkdir ~/.kube
echo "$KUBECONFIG_CONTENT" > ~/.kube/config
envsubst < deploy/no-branch-dev/devops-sample-svc.yaml | kubectl apply -f -
envsubst < deploy/no-branch-dev/devops-sample.yaml | kubectl apply -f -

# GitHub
https://github.com/kubesphere/kubekey/blob/master/README_zh-CN.md
```

## Web 页面简介

#### 工作台
- 企业空间管理
- 集群管理
- 用户和角色管理
- 扩展中心

#### 扩展市场

#### 工具箱

```text
用户和角色管理,默认三个角色
platform-admin 管理 KubeSphere 平台上的所有资源
platform-self-provisioner 创建企业空间并成为所创建的企业空间的管理员
platform-regular 被邀请加入企业空间之前无法访问任何资源
```

```text
扩展市场,安装DevOps
CI/CD -> DevOps -> 订阅 -> 绑定 KubeSphere 云账号 -> 订阅成功 -> 返回 -> DevOps 管理 -> 安装 -> 版本选择 -> 扩展组件安装 -> 
集群选择 -> 差异化配置
```

## 术语

#### 通用术语

集群(Cluster) 集群是一组相互独立的计算机或服务器组成的一个较大的计算机服务系统.

节点(Node) 节点是组成集群的每一台工作机器,可以是虚拟机也可以是物理机.每个节点都可以独立运行和处理任务.

企业空间(Workspace) 企业空间是用来管理项目和组织成员的基本逻辑单元,是 KubeSphere 多租户系统的基础.

企业空间成员 邀请至企业空间中工作的用户,拥有特定的权限.

项目 KubeSphere 中的项目对应 Kubernetes 中的命名空间.

多集群项目 工作负载部署在多个集群上的项目.

项目成员 邀请至项目中工作的用户,拥有特定的权限.

控制台 用户的登录页面,会显示租户拥有访问权限的资源,例如企业空间和项目.

容器组(Pod) Pod 是应⽤程序的最⼩管理单元,相当于应⽤程序的逻辑主机.每个容器组包含⼀个或多个容器,这些容器共享一些集群资源.每个容器组都旨在运行给定应用程序的单个实例.

容器(Container) 容器是可移植,可执行的轻量级的镜像,用于封装应用程序及其依赖项的独立运行环境.

镜像(Image) 镜像是保存的容器实例,包含了应用程序的代码,运行时环境和依赖项.

Docker 一个开源的应用容器引擎,用于创建,部署和管理容器.

Docker Hub 一个容器镜像存储库.

KubeKey 一种全新的安装工具,提供灵活的安装选择,既可以仅安装 Kubernetes,也可以同时安装 Kubernetes 和 KubeSphere.KubeKey 还支持多种安装选项,
例如 All-in-One,多节点安装以及离线安装,用户只需要先准备好配置文件再执行相关命令即可.

ks-installer 在已有 Kubernetes 集群上部署 KubeSphere 的安装包.

kube-proxy kube-proxy 是集群中每个节点上所运行的网络代理.

Kubectl 亦称作: kubectl,与集群的控制平面进行通信的命令行工具,用于集群管理,应用部署,资源状态 查询等操作.

Kubelet kubelet 会在集群中每个节点上运行.它保证容器(containers)都运行在 Pod 中.

#### 集群

集群节点 集群本地的节点,通常所有集群节点都属于同⼀个私有⽹络.包含控制平面节点和工作节点.

控制平面节点 也称为主节点,用来控制和管理整个集群.

工作节点 提供容器运行环境,用来运行实际部署的应用.

边缘节点 部署在边缘环境中受 KubeSphere 管理的节点.

主集群 又称为 host 集群, host 集群管理成员集群,并提供统一的多集群中央控制平面.

成员集群 又称为 member 集群,member 集群在多集群架构中由主集群统一管理.

直接连接 当主集群的任意节点均可访问成员集群的 kube-apiserver 地址时可使用此方式直接连接主集群和成员集群.

代理连接 当主集群无法直接连接成员集群时可使用代理方式连接主集群和成员集群.

jwtSecret 主集群和成员集群中用于校验用户身份的密钥.

Tower 多集群代理连接组件,包含 proxy 和 agent 两个部分,分别部署于主集群和成员集群.

代理服务地址 使用代理连接时,成员集群上的 Tower agent 需要获取的主集群的通信服务地址.

集群可⻅性 控制集群授权给哪些企业空间,以便企业空间可以使用所授权的集群.

#### 应用程序和应用负载

OpenPitrix 一个用于打包,部署和管理不同类型应用的开源系统.

应用模板 某个应用程序的模板,租户可使用应用模板部署新的应用程序实例.

应用商店 应用商店包含内置应用,平台租户也可在应用商店中分享不同的应用程序.

⼯作负载(Workload) 工作负载是在 Kubesphere 上运行的应用程序,负责管理⼀个应⽤程序的一个或多个容器组.

部署(Deployment) 一种工作负载类型,⽤于管理⽆状态应⽤.一个部署运行着应用程序的几个副本,它会自动替换宕机或故障的实例.

有状态副本集(StatefulSet) 有状态副本集是用于管理有状态应用程序的工作负载对象,例如 MySQL.

守护进程集(DaemonSet) 守护进程集管理多组容器组副本,确保 Pod 的副本在集群中的一组节点上运行.

任务(Job) Job 是需要运行完成的确定性的或批量的任务,⽤于管理仅运⾏⼀次或周期性运⾏的容器组.

定时任务(CronJob) 定时任务是需要在特定的时间运行,或在指定的时间间隔内重复运行的批处理任务.

服务(Service) 将运行在容器组上的应用程序公开为网络服务,提供了固定的地址(域名或 IP 地址)供客⼾端访问.

NodePort 通过每个节点上的 IP 和静态端口(NodePort)暴露服务,可通过<节点 IP>:<NodePort>方式来访问服务.

LoadBalancer 使用云服务商提供的负载均衡器向外部暴露服务.

应⽤路由(Ingress) 应⽤路由⽤于对服务进⾏聚合并提供给集群外部访问.每个应⽤路由包含域名及其⼦路径到不同服务的映射规则.KubeSphere 应用路由对应
Kubernetes 中的 Ingress.

#### 存储

存储卷 一个基础资源对象,用于向容器提供存储.

存储类(Storage Class) 定义可供容器使⽤的存储卷类型.

持久卷声明(Persistent Volume Claim, PVC) 持久卷声明是用户对于存储需求的一种声明,它是命名空间里的资源,声明信息中可以指定存储大小,访问模式等.
系统根据持久卷声明创建持久卷.

持久卷(Persistent Volume, PV) 根据持久卷声明中的参数,在后端存储系统中创建的可供容器使⽤的存储区域.它是通用的,可插拔的,并且不受单个 Pod 生命
周期约束的持久化资源.

卷快照类 定义可保存快照数据的⼀类卷快照.

卷快照 卷的数据在某一个时刻的完整拷贝或镜像.可通过快照将数据完整地恢复到快照时间点.

卷快照内容 根据卷快照中的参数,在后端存储系统中保存的快照数据.

#### DevOps

DevOps 项目 DevOps 项目用于创建和管理流水线和凭证.

SCM (Source Control Management) 源控制管理,例如 GitHub 和 Gitlab.

In-SCM 通过 SCM 工具构建基于 Jenkinsfile 的流水线.

Out-of-SCM 通过图形编辑面板构建流水线,无需编写 Jenkinsfile.

CI 节点 流水线,S2I 和 B2I 任务的专用节点.一般来说,应用程序往往需要在构建过程中拉取多个依赖项,这可能会导致如拉取时间过长,网络不稳定等问题,从而使
得构建失败.为了确保流水线正常运行并加快构建速度(通过缓存),您可以配置一个或一组 CI 节点以供 CI/CD 流水线和 S2I/B2I 任务专用.

B2I (Binary-to-Image) B2I 是一套从二进制可执行文件(例如 Jar 和 War 等)构建可再现容器镜像的工具和工作流.开发者和运维团队在项目打包成 War
和 Jar 这一类的制品后,可快速将制品或二进制的 Package 打包成 Docker 镜像,并发布到 DockerHub 或 Harbor 等镜像仓库中.

S2I (Source-to-Image) S2I 是一套从源代码构建可再现容器镜像的工具和工作流.通过将源代码注入容器镜像,自动将编译后的代码打包成镜像.在
KubeSphere 中支持 S2I 构建镜像,也支持以创建服务的形式,一键将源代码生成镜像推送到仓库,并创建其部署和服务最终自动发布到 Kubernetes 中.

#### 日志,事件和审计

日志 日志是集群或应用程序记录的事件列表.

⽇志接收器 收集系统的各类⽇志,包括：容器⽇志,资源事件,审计⽇志.

审计策略 审计策略定义事件记录和所含数据的一系列规则.

审计规则 审计规则定义如何处理审计日志.

审计 Webhook Kubernetes 审计日志会发送至审计 Webhook.

#### 网络

网关(Gateway) 为服务提供反向代理.⽹关根据应⽤路由中定义的规则将业务流量转发给不同的服务.

⽹络策略 ⽤于控制集群中容器组的访问和被访问权限.可以只允许容器组访问特定的其他容器组或⽹段;只允许容器组被特定的其他容器组或⽹段访问.

容器组 IP 池 包含多个虚拟 IP 地址,⽤于为容器组分配虚拟 IP 地址.每个容器组 IP 池包含⼀个可在集群内部访问的私⽹ IP ⽹段.

#### 监控,告警和通知

告警规则组 用于在特定监控指标满⾜预设条件和持续时间时⽣成告警.

Prometheus 负责监控存储系统的各项数据,根据告警规则向告警管理器发送告警信息.

#### 其他

污点(Taint) ⽤⼾在节点上创建的标记,由键,值和效果三部分组成.与容器组上创建的容忍度配合使⽤,以确保不会将 Pod 调度到不适合的节点上.

容忍度(Toleration) 容忍度表示允许将 Pod 调度到具有对应污点的节点或节点组上.由键,值和效果三部分组成.容忍度和污点共同作用可以确保不会将 Pod 调度
在不适合的节点上.

标签(Label) 标签是为对象设置的可标识的键值对,通常用来管理和选择对象子集.

注解(Annotation) 注解是以键值对的形式给资源对象附加随机的无法标识的元数据.

会话保持 将同⼀个会话中来⾃同⼀个客⼾端的请求全部转发给同⼀个容器组.

保密字典(Secret) 包含 Base64 编码的键值对,⽤于存储密码,令牌,密钥等保密数据.

配置字典(ConfigMap) 以键值对的形式存储环境变量,命令⾏参数和配置⽂件等⾮保密数据.

服务账户(ServiceAccount) 存储当前集群的访问信息,⽤于向集群内外的应⽤程序提供集群的访问权限.

定制资源定义(CustomResourceDefinition) 使⽤定制资源定义创建定制资源.通过定制化的代码给 API 服务器增加资源对象,而无需编译完整的定制 API 服务
器.

----

以上就是本文核心内容.

路漫漫兮修远兮,我将上下而求索.

[返回顶部](#主要内容)

