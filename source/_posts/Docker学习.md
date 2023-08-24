---
title: Docker学习
---

记录一些常用的 Docker 命令和知识 <!-- more -->

要进入 Docker 中的 Nginx 容器，您可以在终端中使用 docker exec 命令。以下是如何操作的步骤：

1. 打开您的终端或命令提示符。

2. 输入以下命令以列出系统上所有正在运行的 Docker 容器：

```
docker ps
```

3. 这将显示所有正在运行的容器列表，包括它们的名称和 ID。

从列表中找到您想要进入的 Nginx 容器的名称或 ID。

4. 要进入容器，请使用以下命令，在其中将 "container-name" 替换为您想要进入的容器的实际名称或 ID：

```bash
docker exec -it container-name /bin/bash
```

-it 选项允许您交互式地进入容器并附加到其控制台。/bin/bash 是您想要在容器内运行的 shell 命令。

5. 进入容器后，您可以像在容器的命令行界面上一样运行任何命令。

6. 要退出容器，请输入 exit 或按下 CTRL + D。

注意：如果您在容器中运行的是不同的 shell（例如，是 sh 而不是 bash），请将 /bin/bash 替换为相应的 shell 命令。




