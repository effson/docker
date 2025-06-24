# 1
```
docker port 命令用于查看 Docker 容器的端口映射信息。可以确认容器内部的端口如何映射到主机上的端口


✅ 用法
docker port CONTAINER [PRIVATE_PORT[/PROTOCOL]]
主要选项
CONTAINER: 容器的名称或 ID。
PRIVATE_PORT: 可选参数，指定容器内部的端口。如果未指定，命令将列出所有端口映射。
PROTOCOL: 可选参数，指定协议（如 tcp 或 udp）。


✅ 示例
查看所有端口映射
docker port my_container

输出可能类似于：
80/tcp -> 0.0.0.0:8080

查看特定端口的映射
docker port my_container 80

输出可能为：
0.0.0.0:8080
```
