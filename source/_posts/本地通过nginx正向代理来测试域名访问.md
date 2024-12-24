---
title: 本地通过nginx正向代理来测试域名访问
---
有些场景，手头没有域名，又需要测试通过域名访问的场景，将整个操作流程记录于此 <!-- more -->

## 修改本地host来完成域名解析
假设，我们希望正常访问 mock.localhost ，我们需要进行如下操作：
1. 进入 C:\Windows\System32\drivers\etc 路径，备份 hosts 文件
2. 在 hosts 中新增一行`127.0.0.1 mock.localhost`

## 安装 nginx 
1. 配置choco, 增加环境变量 ChocolateyToolsLocation: xxx\choco\tools
2. 执行 choco install nginx --params '"/port:80"' （80端口被占用时可以修改
3. 配置环境变量，path下新增 xxx\choco\tools\nginx-xxx


## 增加 nginx 配置进行正向代理
1. 将如下配置保存在 test_505.conf 文件内
```
server {
    listen       80;
    server_name  mock.localhost;

    location / {
        proxy_pass https://10.xxx.193.156:31943;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
2. 检查配置文件语法，执行 nginx -t -c xxx\forward-proxy\test_505.conf
3. 启动nginx并加载指定配置 nginx -c xxx\forward-proxy\test_505.conf
4. 修改配置，重新执行 nginx -s reload -c xxx\forward-proxy\test_505.conf
5. 停止运行 nginx -s stop