# 1
```
docker stats 命令用于实时显示运行中 Docker 容器的资源使用情况，包括 CPU、内存、网络和块 I/O 等指标。监控容器性能

✅ 基本语法
docker stats [OPTIONS] [CONTAINER...]

主要选项
--no-stream: 显示一次当前的资源使用情况，而不是持续更新。
--format: 自定义输出格式。
--all: 显示所有容器的信息，即使它们没有在运行


✅ 示例
查看所有运行中的容器资源使用情况
docker stats

查看特定容器的资源使用情况
docker stats my_container

一次性显示资源使用情况
docker stats --no-stream


✅ 输出信息
CONTAINER ID: 容器的 ID。
NAME: 容器的名称。
CPU %: 容器使用的 CPU 百分比。
MEM USAGE / LIMIT: 使用的内存与限制的内存。
MEM %: 使用的内存的百分比。
NET I/O: 网络输入/输出。
BLOCK I/O: 块输入/输出。
```
