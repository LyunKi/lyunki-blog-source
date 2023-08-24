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

# 因为 set 的时候只做了添加，为了防止重复，这里清理重复的 path
function cleanPath(){
    $CurrentPath = [Environment]::GetEnvironmentVariable('Path','Machine')
    $SplittedPath = $CurrentPath -split ';'
    $CleanedPath = $SplittedPath | Sort-Object -Unique
    $NewPath = $CleanedPath -join ';'
    [Environment]::SetEnvironmentVariable('Path', $NewPath,'Machine')
    echo "complete clean"
}
```

### 删除

powershell 本身提供了 remove-item 命令，这里做一个简单的映射到 rm 即可，虽然没有 -rf，但可以通过 -Recursive 和 -Force 来代替，并且即使没有传入 Flag ，也会有一个友好的交互提示如何进行下一步

```
function rm([string]$path){
    remove-item $path
}
```

### 设置 windows 文件夹大小写敏感
**要求管理员模式下运行**
查询文件是否大小写敏感
```powershell
fsutil.exe file queryCaseSensitiveInfo <path>

```

设置文件大小写敏感
```powershell
fsutil.exe file setCaseSensitiveInfo <path> enable
```

设置文件大小写不敏感
```powershell
fsutil.exe file setCaseSensitiveInfo <path> disable
```

