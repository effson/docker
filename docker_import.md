# 1
```
docker import 命令用于从一个 tar 归档文件创建 Docker 镜像。
这个命令通常与 docker export 配合使用，可以将导出的容器文件系统重新导入为新的镜像

✅ 基本语法
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]

主要选项
-o, --change: 可以在导入时设置镜像的配置，例如环境变量、工作目录等。
--help: 显示帮助信息。


✅ 示例
从 tar 文件导入镜像
docker import my_container.tar my_image:latest

设置环境变量
docker import -o ENV=production my_container.tar my_image:latest


✅ 注意事项
docker import 只会导入文件系统，不包含任何 Docker 元数据（如容器的历史、元数据或标签）。
导入的镜像会被赋予默认的唯一 ID，并可以通过指定的名称和标签进行访问。


✅ 常见用途
恢复备份：将先前导出的容器文件系统导入为新的镜像，以便恢复数据。
迁移应用：从一台机器迁移到另一台机器，保持应用及其文件系统的一致性。
创建基础镜像：将现有的容器文件系统导入为新的基础镜像，以供后续构建和开发使用。
```
