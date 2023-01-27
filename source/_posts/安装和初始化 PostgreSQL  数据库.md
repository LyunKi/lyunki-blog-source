---
title: 安装和初始化 PostgreSQL  数据库
---

近期，想在自己的服务器上进行生产环境的搭建，数据库选用的是 PostgreSQL  这里记录一下数据库的搭建流程 <!-- more -->

## PostgreSQL 下载

参考[官网](https://www.postgresql.org/download/), 在服务器上执行 uname -a ，能够看到是 Debian 的系统，他是自带 postgresql 的
![](/images/posts/2023-01-27-10-11-33.png)
因此执行 `sudo apt-get install postgresql` 即可

## PostgreSQL 初始化配置
初次安装后，会默认生成一个名为 postgres 的数据库和数据库用户，并且会同时生成一个名为 postgres 的 linux 系统用户。由于默认的 postgres 数据库用户没有密码，我们需要先为其配置对应的密码，
1. 首先切换到 postgres 用户 `sudo su - postgres`
2. 登录 postgres 控制台 `psql`
3. 执行 `\password postgres` 命令设置密码
  
现在，我需要初始化一个名为 xxx 的数据库和用户,紧接着刚刚的步骤
1. 创建数据库用户 ` CREATE USER xxx WITH PASSWORD 'xxx'`;
2. 创建数据库 `CREATE DATABASE xxx OWNER xxx;`
3. 对数据库 xxx 的所有权限授予 xxx `GRANT ALL PRIVILEGES ON DATABASE xxx to xxx;`
4. \q 退出 postgres 控制台
5. 使用新建的数据库用户 xxx 来登录 postgres 控制台 `psql -U xxx -d xxx -h 127.0.0.1 -p 5432`

