# Docker 删除及清理镜像

## 通过标签或者ID删除镜像

$ `docker rmi [image]`    或者    $ `docker image rm [image]`

支持的子命令:

`-f ` 强制删除镜像，即使有容器引用该镜像

`-no-prune` 不要删除未带标签的父镜像

`[image]` 部分 可以为 `REPOSITORY:TAG` 形式，也可以为 `IMAGE ID`

当一个镜像有多个标签时，`docker rmi` 命令只会删除该镜像众多标签中指定的标签，不会影响原始的镜像文件。

## 删除镜像的限制

当该镜像创建的容器未被销毁时，镜像是无法删除的。加上 -f ，会变成 <none>,但依然存在，目前不懂 <none>是什么意思。

1. 先删除引用这个镜像的容器
2. 删除这个镜像 `docker rm containerID`

## 清理镜像

系统残存一些临时的、没有使用的镜像文件

$ `docker image prune`

支持的子命令：

`-a` 删除所有没有用的镜像，而不仅仅是临时文件

`-f` 强制删除镜像文件，无需弹出提示确认