# VMware Workstation 安装和使用许可证 https://www.vmware.com/

# ubuntu iso 系统安装 16核16G200G桥接网络 https://mirrors.ustc.edu.cn/

# ip addr 地址查看 192.168.1.21

# uname -m 架构查看

# chrome 下载 https://www.google.cn/chrome/

# 安装 chrome
dpkg -i google-chrome-stable_current_amd64.deb

# 更新软件源
apt-get update

# 下载 jdk
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz

# 创建文件夹
mkdir /usr/lib/jvm

# 解压
tar -xzvf jdk-21_linux-x64_bin.tar.gz -C /usr/lib/jvm/

# 安装 vim
apt install vim

# 配置环境变量
vim /etc/profile
export JAVA_HOME=/usr/lib/jvm/jdk-21.0.4
export PATH=$PATH:$JAVA_HOME/bin

# 生效
source /etc/profile

# 验证
java -version

# 管理员权限打开文件管理器粘贴
nautilus

# 下载 https://www.jetbrains.com/idea/download
ideaIU-2024.2.1.tar.gz

# 解压
tar -zxvf /app/software/ideaIU-2024.2.1.tar.gz -C /app/

# 文件夹改名
mv idea-IU-242.21829.142 idea

# 授予执行权限
chmod +x idea.sh

# 启动
./idea.sh

# maven 下载 https://maven.apache.org/

# 解压 maven
tar -zxvf /app/software/apache-maven-3.9.9-bin.tar.gz -C /app/

# 配置环境变量
vim /etc/profile
export MAVEN_HOME=/app/apache-maven-3.9.9
export PATH=$PATH:$MAVEN_HOME/bin

# 生效
source /etc/profile

# 验证
mvn -v
mvn package
mvn clean package -DskipTests

# npm
apt update
apt install npm
node -v
npm config set registry https://registry.npmmirror.com/
npm config get registry

# 更新 npm
npm install -g n
n stable
hash -r

# 安装 vue
npm install vue-cli-service

tee /opt/idea-IU-242.21829.142/bin/idea.desktop <<-'EOF'
[Desktop Entry]
Name=IntelliJ IDEA
Comment=IntelliJ IDEA
Exec=/opt/idea-IU-242.21829.142/bin/idea.sh
Icon=/opt/idea-IU-242.21829.142/bin/idea.png
Terminal=false
Type=Application
Categories=Development;IDE;
EOF

# 若依
docker pull openjdk:8-jre
mvn package
cd ruoyi-ui
# 打包正式环境
npm run build:prod
# 打包预发布环境
npm run build:stage
cd docker
./copy.sh

初始化sql

# 启动基础环境（必须）
./deploy.sh base

# 启动程序模块（必须）
./deploy.sh modules

# 关闭所有环境/模块
./deploy.sh stop
