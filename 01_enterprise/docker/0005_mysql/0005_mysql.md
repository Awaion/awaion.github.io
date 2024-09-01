```shell
# 进入目录
cd /app

# 管理员权限打开文件管理器粘贴 docker-compose 文件
nautilus

# 上传完配置文件 -> 检查配置文件
docker-compose -f docker-compose-mysql.yml config

# 启动 注意端口不要被占用
docker compose -f docker-compose-mysql.yml up -d

# docker 进程查看
docker ps

# docker 日志查看
docker logs

# 文件夹授权 用于因权限问题导致报错
chmod -R 777 /app

# 本地访问 localhost 3306
# 查看 ip 访问 192.168.31.151 3306
ip addr

# 服务下线
docker compose -f docker-compose-mysql.yml down
```

![001.png](./001.png)
