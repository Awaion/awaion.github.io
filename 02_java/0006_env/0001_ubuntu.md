# 下载
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz

# 创建文件夹
mkdir /usr/lib/jvm

# 解压
tar -xzvf jdk-21_linux-x64_bin.tar.gz -C /usr/lib/jvm/

# 配置环境变量
vim /etc/profile
export JAVA_HOME=/usr/lib/jvm/jdk-21
export PATH=$PATH:$JAVA_HOME/bin

# 生效
source /etc/profile

# 验证
java -version

# 下载
IntelliJ IDEA的.tar.gz

# 解压
tar -zxvf /path/to/ideaIU-2023.1.1.tar.gz -C /opt/

# 启动
cd /opt/idea-IU-231.8770.65/bin/
./idea.sh

# maven
export MAVEN_HOME=/root/.m2/apache-maven-3.9.9
export PATH=$PATH:$MAVEN_HOME/bin

mvn package
mvn clean package -DskipTests

# npm
apt update

apt install npm
npm config set registry https://registry.npmmirror.com/
npm config get registry
npm install -g npm@latest
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
