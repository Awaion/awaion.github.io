# Linux 常用命令

# 主要内容

----

```shell
# List 列出目录列表 
ls -l

# Print working directory 打印工作目录 
pwd

# Change directory 改变目录 
cd /app

# free 显示内存信息 
free -hm

# network statistics 监控网路连接,路由,接口等 
netstat -nltp

# make directory 创建目录 
mkdir app

# Change mode 给所有者,所属组,其它用户授予读,写,执行(二进制为111,转为十进制为7),三种权限 
chmod 755 app/

# CommandLine Uniform Resource Locator 用命令行找url(统一定位资源) 
curl https://xxx

# disk usage 磁盘使用情况统计
du -ah

# concatenate 查看文件内容
cat xxx.log

# Copy 复制
cp source.txt destination.txt
cp -r sourcedir/ destinationdir/

# Move 移动或重命名
mv olddirectory/ newdirectory/
mv oldname.txt newname.txt

# Remove 删除文件或目录 
rm -f filename.txt
rm -r directoryname/

# Global Regular Expression Print 全局正则表达式打印
grep -in 'pattern' filename.txt

# find 查找
find -name '*.sh'

# Process Status 进程状态
ps -e -o pid,ppid,cmd,%mem,%cpu | grep nginx

# Table Of Processes 进程表
top

# kill 杀进程
kill pid

# 网络接口配置 network interfaces configuring 
ifconfig

# 分组网间探测 Packet Internet Groper
ping www.google.com

# 安全外壳协议 Secure shell
ssh

# Secure copy 安全复制
scp

# Tape Archive 磁带归档,打包解压
tar -czvf archive_name.tar.gz directory_or_file
tar -xzvf archive_name.tar.gz

# Disk File system 磁盘文件系统 腾讯云不加路径会显示其它服务器信息(安全?)
df /app -haT --total

# Web Get
wget http://example.com/path/to/file

# Change Owner 更改文件所有者
chown newowner filename
chown -R newowner:newgroup directoryname/

# password 修改用户密码
passwd 用户名

# 添加用户
useradd -m -d /path/to/new/home/directory 用户名

# 删除用户
userdel 用户名

# 添加组
groupadd 组名

# 组删除
groupdel 组名

# Control the systemd system and service manager
systemctl start 服务名

# Yellowdog Updater Modified 包管理工具
sudo yum install 软件包名

# 服务
service 服务名 start

# 计划任务
crontab -l
0 10 * * 1 /path/to/backup.sh

```

## 参考

[返回顶部](#主要内容)

