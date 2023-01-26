---
title: vscode 配置远程登录免密码
---

最近，发现 vscode 的远程开发能力很棒，这里记录一个免密登录的方法，避免每次链接远程服务器都需要使用密码 <!-- more -->

## Prerequisite

1. 本地安装好了 ssh 客户端
2. vscode 安装了 remote development 相关的远程插件

## 配置秘钥

1. 执行 ssh-keygen， 这里建议给生成的文件配置一个单独的名称，例如`id_rsa.vultr`
   ![](/images/vscode/id_rsa.vultr.png)
2. 执行 `cat ~/.ssh/id_rsa.vultr.pub`, 将内容复制下来
3. 在远程服务器上，打开 authorized_keys 文件，将刚刚的内容贴上去
4. 在 vscode 远程资源管理中，点击右上角配置文件
   ![](/images/vscode/2.png)
5. 在配置文件中配置 IdentiyFile 路径
   ![](/images/vscode/3.png)

这时，在尝试登录远程服务器就不再需要密码了
