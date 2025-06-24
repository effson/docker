# 1
```
docker cp 命令用于在 Docker 容器和主机之间复制文件或目录。可以轻松地管理容器内的文件，进行备份或恢复

✅ 基本语法
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

主要选项
-a, --archive: 归档模式，保留文件的属性。
--help: 显示帮助信息。


✅ 示例
从容器复制文件到主机
docker cp my_container:/path/to/file /local/path

从主机复制文件到容器
docker cp /local/path/myfile my_container:/path/in/container

复制整个目录
docker cp my_container:/path/to/directory /local/path


✅ 主要用途
备份：从容器中提取重要文件进行备份。
恢复：将文件从主机复制到容器，以恢复数据或配置。
调试：在容器运行时，检查和修改文件以帮助调试应用。


✅ 注意事项
使用 docker cp 时，确保目标路径存在，否则会导致错误。
复制的文件或目录会保留原有的权限和属性（如果使用了 -a 选项）。
```
