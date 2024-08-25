```shell
# ubuntu-22.04.4-desktop-amd64.iso
# 设置主机名
sudo hostnamectl set-hostname master

# 管理员权限打开文件管理器粘贴
sudo nautilus

# 重启
reboot

# 初始化 root 密码
sudo passwd root

# 选择 root 用户
su root

# 修改为国内 apt 镜像地址
sudo bash -c "cat << EOF > /etc/apt/sources.list && apt update
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
EOF"

# 修改后查看 apt
cat /etc/apt/sources.list

# 安装常用工具
apt install curl net-tools openssh-server git -y

# 进入根目录 创建文件夹 进入 app 文件夹
cd /
mkdir app
cd app

# 更新软件包
apt update

# docker依赖项
apt install apt-transport-https ca-certificates curl software-properties-common

# GPG 密钥 不确定是否会失效
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# GPG 密钥添加到 apt 的信任密钥环
echo "trusted=yes" | sudo tee /etc/apt/trusted.gpg.d/docker.gpg

# docker 镜像仓库 不确定是否会失效
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 更新软件包
apt update

# 安装 docker 及相关
apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 安装 python3 python3-pip
apt install python3 python3-pip -y

# 版本查看 3.10.12 版本查看 22.0.2
python3 --version
pip3 --version

# 添加镜像仓库 不确定是否会失效
pip3 config set global.index-url https://mirrors.aliyun.com/pypi/simple

# 安装 docker-compose
pip3 install docker-compose

# 版本查看 1.29.2
docker-compose --version

# 管理员权限打开文件管理器粘贴 docker-compose-cluster.yml
sudo nautilus

# 检测文件配置
docker-compose -f docker-compose-mongo.yml config

# 新增 dockerhub 镜像仓库 不确定何时失效
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://dockerhub.icu"
  ]
}
EOF
cat /etc/docker/daemon.json
systemctl daemon-reload
systemctl restart docker
docker info

# 进入目录
cd /app
docker run hello-world

# 启动
docker compose -f docker-compose-mongo.yml up -d

# 查看启动的容器
netstat -nltp | grep docker

# 查看 docker 日志
docker ps
docker logs d26ff8e6e155

# 查看 ip 访问 docker-express http://192.168.31.151:27027/
ip addr

# 停止 
docker compose -f docker-compose-mongo.yml down

```
