# KubeSphere DevOps

# 主要内容

创建企业空间,创建项目,点进项目即可看到 DevOps

## 创建流水线

- 流水线 创建 基本信息 名称 springboot-web 编写 Jenkinsfile 文件

- DevOps 项目设置 凭证 用户和密码 github-id

## 配置 Maven 镜像

- 集群 配置 配置字段 ks-devops-agent 编辑 YAML

```xml
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>        
</mirror>
```

## 编辑 Jenkinsfile 文件

```jenkinsfile
pipeline {
    agent {
        node {
            label 'maven'
        }
    }

    parameters {
        string(name:'TAG_NAME',defaultValue: '',description:'')
    }

    environment {
        DOCKER_CREDENTIAL_ID = 'dockerhub-id'
        GITHUB_CREDENTIAL_ID = 'github-id'
        KUBECONFIG_CREDENTIAL_ID = 'demo-kubeconfig'
        REGISTRY = 'registry.cn-hangzhou.aliyuncs.com'
        DOCKERHUB_NAMESPACE = 'docker_username'
        GITHUB_ACCOUNT = 'kubesphere'
        APP_NAME = 'devops-java-sample'
    }

    stages {
        stage('拉取代码') {
            agent none
            steps {
                container('maven') {
                    git(url: 'https://gitee.com/Awaion/tools.git', credentialsId: "$GITHUB_CREDENTIAL_ID", branch: 'master', changelog: true, poll: false)
                    sh 'ls -al'
                    sh 'ls -al demo002'
                }
            }
        }

        stage('项目编译') {
            agent none
            steps {
                container('maven') {
                    sh 'ls -al demo002'
//                     sh 'mvn  -Dmaven.test.skip=true -gs `pwd`/configuration/settings.xml clean package'
                    sh 'mvn clean package -f demo002/pom.xml -Dmaven.test.skip=true'
                    sh 'ls -al demo002/target'
                }

            }
        }

        stage('构建&推送镜像') {
            parallel {
                stage('推送demo002镜像') {
                    agent none
                    steps {
                        container('maven') {
//                             withCredentials([usernamePassword(passwordVariable : 'DOCKER_PASSWORD' ,usernameVariable : 'DOCKER_USERNAME' ,credentialsId : "$DOCKER_CREDENTIAL_ID" ,)]) {
                            withCredentials([usernamePassword(credentialsId : "$DOCKER_CREDENTIAL_ID" ,usernameVariable : 'DOCKER_USER_VAR' ,passwordVariable : 'DOCKER_PWD_VAR' ,)]) {
                                sh 'ls -al demo002/target'
//                              sh 'docker build -f Dockerfile-online -t $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER .'
                                sh 'docker build -f demo002/Dockerfile -t $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER .'
//                              sh 'docker build -t demo002:latest -f demo002/Dockerfile demo002'

//                              sh 'echo "$DOCKER_PASSWORD" | docker login $REGISTRY -u "$DOCKER_USERNAME" --password-stdin'
                                sh 'echo "$DOCKER_PWD_VAR" | docker login $REGISTRY -u "$DOCKER_USER_VAR" --password-stdin'

//                              sh 'docker push $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER'
                                sh 'docker push $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER'

//                              sh 'docker tag demo002:latest $REGISTRY/$DOCKERHUB_NAMESPACE/demo002:SNAPSHOT-$BUILD_NUMBER'
                                sh 'docker tag  $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:latest '
                                sh 'docker push  $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:latest '
                            }
                        }
                    }
                }
            }
        }

        stage('部署') {
          when{
            branch 'master'
          }
          steps {
            input(id: 'deploy-to-dev', message: 'deploy to dev?')
            kubernetesDeploy(configs: 'demo002/deploy/devops-sample.yaml', enableConfigSubstitution: true, kubeconfigId: "$KUBECONFIG_CREDENTIAL_ID")
          }
        }

        stage('发送确认邮件') {
            agent none
            steps {
//                 mail(to: '1191831992@qq.com', cc: '', subject: 'dev发布成功', body: "构建成功了  $BUILD_NUMBER")
                emailext (to: '1191831992@qq.com', subject: 'dev发布成功', body: "构建成功了  $BUILD_NUMBER")
            }
        }
    }
}

```

## 编辑 Dockerfile 文件

```dockerfile
FROM openjdk:8-jdk
LABEL maintainer=awaion


#启动自行加载   服务名-prod.yml配置
ENV PARAMS="--server.port=8080 --spring.profiles.active=prod --spring.cloud.nacos.server-addr=his-nacos.his:8848 --spring.cloud.nacos.config.file-extension=yml"
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

COPY target/*.jar /app.jar
EXPOSE 8080

#
ENTRYPOINT ["/bin/sh","-c","java -Dfile.encoding=utf8 -Djava.security.egd=file:/dev/./urandom -jar /app.jar ${PARAMS}"]
```

## 服务器 Docker 服务安装

没有 docker 则安装.

## 阿里云容器镜像服务启用

https://cr.console.aliyun.com/cn-hangzhou/instance/repositories

KubeSphere DevOps 项目设置 凭证 用户和密码 dockerhub-id

KubeSphere DevOps 项目设置 凭证 kubeconfig demo-kubeconfig 注意编辑 apiVersion: apps/v1

k8s1.9版本有所更新写法 v1 -> apps/v1,与deploy.yml文件保持一致

## 编辑 deploy.yml 文件

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: kubesphere
    component: ks-sample-dev
    tier: backend
  name: ks-sample-dev
  namespace: kubesphere-sample-dev
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  selector:
    matchLabels:
      app: kubesphere
      component: ks-sample-dev
      tier: backend
  template:
    metadata:
      labels:
        app: kubesphere
        component: ks-sample-dev
        tier: backend
    spec:
      containers:
        - image: $REGISTRY/$DOCKERHUB_NAMESPACE/$APP_NAME:SNAPSHOT-$BUILD_NUMBER
          imagePullPolicy: Always
          name: ks-sample
          readinessProbe:
            httpGet:
              path: /
              port: 8080
            timeoutSeconds: 10
            failureThreshold: 30
            periodSeconds: 5
          ports:
            - containerPort: 8080
              protocol: TCP
          resources:
            limits:
              cpu: 300m
              memory: 600Mi
            requests:
              cpu: 100m
              memory: 100Mi
          terminationMessagePath: /dev/termination-log
          terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      terminationGracePeriodSeconds: 30
```

## 配置邮件通知地址

KubeSphere 集群 应用负载 工作负载 devops-jenkins 编写 YMAL

EMAIL_FROM_PASS,EMAIL_FROM_ADDR,EMAIL_USE_SSL,EMAIL_SMTP_HOST

## 运行

----

[Github 源码](https://github.com/Awaion/tools/tree/master/demo002)

路漫漫兮修远兮,我将上下而求索.

[返回顶部](#主要内容)

