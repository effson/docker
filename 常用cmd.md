```
✅ 1.docker images
docker images 命令用于列出 Docker 主机上所有可用的镜像。这个命令提供了关于每个镜像的基本信息，包括其名称、标签和大小。
docker images -a 命令用于列出所有 Docker 镜像，包括未标记的（dangling）镜像。
```
*
```
虚悬镜像（Dangling Images）
docker images -f "dangling=true"
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>             abc123def456        2 days ago         150MB

清理虚悬镜像
docker rmi $(docker images -f "dangling=true" -q)
```


```
✅ 2.docker images

```
