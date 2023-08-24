---
title: 在docker容器中使用acme.sh生成证书.md
---

记录用 acme 工具在 docker 容器中使用 nginx 模式生成签名证书 <!-- more -->

## 进入 nginx docker 容器

```shell
docker exec -it 1a89718e5960 /bin/sh
```

## 安装 crontab

容器通常没有 cron，需要提前安装

```shell
apt-get update
apt-get install cron

# 可选，有时需要进行编辑
apt-get install vim-tiny
```

## 用 curl 下载安装 acme.sh，并开启自动更新

```shell
curl https://get.acme.sh | sh -s email=xxx@gmail.com
```

## 指定 acme.sh 版本

遇到了一个 3.0.7 版本的坑，需要指定 acme.sh 版本 (也可以直接参考链接修改 acme.sh 文件 https://github.com/acmesh-official/acme.sh/pull/4749/commits/13d31ecb7f195f623a9a4deff9c2eb3fc6aecaeb)

```shell
cp -r ~/.acme.sh ~/.acme.sh.backup
wget https://github.com/acmesh-official/acme.sh/archive/refs/tags/v3.0.6.tar.gz
tar -zxvf v3.0.6.tar.gz
cd acme.sh-3.0.6
./acme.sh --install
```

## 自动更新和关闭自动更新

```shell
~/.acme.sh/acme.sh --upgrade --auto-upgrade
~/.acme.sh/acme.sh --upgrade --auto-upgrade 0
```

## 修改 ca

```shell
~/.acme.sh/acme.sh --set-default-ca --server letsencrypt
```

## 还原默认的 ca

```shell
~/.acme.sh/acme.sh --set-default-ca --server zerossl
```

## 签发证书

此时还没有证书，需要先移除 nginx 中的 ssl 配置

```shell
export domain_name=cx.cloud-xiao.online && ~/.acme.sh/acme.sh --issue --nginx -d $domain_name
```

## 安装证书 (要确保路径正确)

```shell
export domain_name="cx.cloud-xiao.online" certs_dir=/etc/nginx/certs &&
mkdir -p $certs_dir/$domain_name &&
~/.acme.sh/acme.sh --install-cert -d $domain_name \
--key-file       $certs_dir/$domain_name/cert.key \
--fullchain-file $certs_dir/$domain_name/fullchain.cer \
--reloadcmd "service nginx force-reload"
```

## 查看定时任务

```shell
crontab -l
```
