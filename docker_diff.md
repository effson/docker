# 1
```
docker diff 命令用于查看 Docker 容器文件系统的改动。它会显示自容器创建以来，对文件系统所做的添加、修改或删除的文件


✅ 基本语法
docker diff CONTAINER

docker diff 的输出会包含以下信息：
A: 表示添加的文件或目录。
C: 表示修改的文件。
D: 表示删除的文件或目录。


✅ 示例
docker diff my_container

输出可能类似于：
A /new_file.txt
C /existing_file.txt
D /deleted_file.txt


✅ 主要用途
变更跟踪：帮助用户跟踪容器运行期间的文件系统变化，了解应用的状态。
调试：在调试过程中，查看哪些文件被修改、添加或删除，有助于识别问题的根源。
容器优化：分析哪些文件不必要，帮助优化容器镜像。


✅ 注意事项
docker diff 只显示文件系统的变化，不包括容器的配置或运行时状态。
该命令不适用于已停止的容器。
```
