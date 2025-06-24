```
✅ 1.docker images
docker images 命令用于列出 Docker 主机上所有可用的镜像。这个命令提供了关于每个镜像的基本信息，包括其名称、标签和大小。
docker images -a 命令用于列出所有 Docker 镜像，包括未标记的（dangling）镜像。
docker images prune 命令用于清理未使用的 Docker 镜像，包括虚悬镜像（dangling images）

**虚悬镜像（Dangling Images）
docker images -f "dangling=true"
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>             abc123def456        2 days ago         150MB
**清理虚悬镜像
docker rmi $(docker images -f "dangling=true" -q)
```


```
✅ 2.docker rmi
docker rmi 命令用于删除 Docker 镜像。

docker rmi [OPTIONS] IMAGE [IMAGE...]
**主要选项
-f, --force: 强制删除镜像，即使该镜像被一个或多个容器使用。
--no-prune: 不删除任何未被标记的父镜像。
```

```
✅ 3.docker tag
docker tag 命令用于为现有的 Docker 镜像创建一个新的标签。
可以管理镜像版本或将镜像标记为不同的版本

docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```
