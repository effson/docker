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

```
✅ 4.docker save
docker save 命令用于将 Docker 镜像保存为一个 tar 归档文件。
通常用于备份镜像或将镜像传输到其他系统

docker save [OPTIONS] IMAGE [IMAGE...]
**主要选项
-o, --output: 指定输出文件的名称，默认为 -（标准输出）。
--help: 显示帮助信息。
```

```
✅ 5.docker commit
docker commit 命令用于将正在运行的容器的当前状态保存为一个新的 Docker 镜像。
可以在容器中进行更改后，创建一个新的镜像以便后续使用。

docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
**主要选项
-a, --author: 指定镜像的作者信息。
-m, --message: 添加提交信息，用于描述这次提交的更改。
--change: 修改镜像的元数据，例如设置环境变量、工作目录等。
```
