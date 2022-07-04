---
title: powershell 学习
---

本来作为一名前端人员，用 nodejs 实现脚本的想法是自然的，然而实际使用过程中，发现还是怀念在 oh my zh 里面畅快使用命令的日子，但是工作的开发环境是 Windows ，所幸 powershell 脚本其实也挺好用，于是也记录一些对于 powershell 的学习 <!-- more -->

## Prelude

首先在 powershell 执行 `Code $PROFILE` （Code 会全局的打开 VSCode 编辑器，可能需要配置环境变量，可自行选择文本编辑器）,然后保存文件，这样能够迅速的创建一个脚本，会在每次启动 powershell 时执行。

## open 命令

open . 在 omz 中能迅速的打开当前文件夹，这对我来说算是非常常用的一个命令，而在 powershell 中对应的命令是 ii, 因此可以实现一个 open 方法（对于有 Typescript 经验的开发人员来说，这实在太过熟悉。。。以至于我甚至没有阅读过任何 ps 脚本文档，只是仿照着其他脚本就实现了一个简单的 open 命令

```powershell
function open([string]$path){
    ii "$path"
}
```

## 环境变量

读取和设置环境变量，这里直接在机器级别进行，方便后续账号切换后，配置依然生效。

```powershell
function getEnv([string]$key) {
    [Environment]::GetEnvironmentVariable("$key", [EnvironmentVariableTarget]::Machine)
}

# 只添加不修改，方便回退
function setEnv([string]$key,[string]$value) {
    [Environment]::SetEnvironmentVariable(
        "$key",
        [Environment]::GetEnvironmentVariable("$key", [EnvironmentVariableTarget]::Machine) + ";$value",
        [EnvironmentVariableTarget]::Machine)
}
```
