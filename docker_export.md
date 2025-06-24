# 1
```
✅ 含义：
docker export 命令用于将一个 Docker 容器的文件系统导出为一个 tar 归档文件。这个命令可以在需要备份容器的文件系统或迁移容器时非常有用。


✅ 用法：
docker export [OPTIONS] CONTAINER
主要选项
-o, --output: 指定输出文件的名称。例如，-o my_container.tar。
--help: 显示帮助信息。


示例
导出容器文件系统
docker export my_container > my_container.tar

指定输出文件
docker export -o my_container.tar my_container

注意事项
docker export 只会导出容器的文件系统，不包括容器的配置、环境变量或其他 Docker 元数据。
导出的 tar 文件可以在其他 Docker 主机上使用 docker import 命令导入为新的镜像。

✅ 常见用途
备份容器的文件系统。
迁移容器内容到其他环境。
创建镜像的基础内容，供后续构建使用。
```
