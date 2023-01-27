---
title: 服务器安装和初始化 Redis
---

记录一下 Redis 的安装和初始化流程 <!-- more -->

## Redis 下载
参考[官网](https://redis.io/docs/getting-started/installation/install-redis-on-linux/)
1. 确认服务器是否已经安装了 lsb-release (尝试执行 `lsb_release -cs` )，如果没有，执行安装 `sudo apt install lsb-release`
2. 按顺序执行如下命令

```
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
sudo apt-get update
sudo apt-get install redis
``` 

## 设置 Redis 密码
1. 打开 redis 配置文件，如上方法安装后，默认的配置文件在 `/etc/redis/redis.conf` 下
2. 找到 `requirepass xxx`，取消注释，修改为自己希望的密码
3. 重启 redis-server `service redis-server restart`.
4. 执行 `redis-cli -a xxx`完成客户端认证