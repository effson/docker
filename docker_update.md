# 1
```
docker update 命令用于更新运行中 Docker 容器的资源限制和配置。这可以包括更改 CPU 配额、内存限制和其他运行时参数

✅ 基本语法
docker update [OPTIONS] CONTAINER...

主要选项
--cpus: 设置容器可使用的 CPU 核心数。
--memory: 设置容器的内存限制。
--restart: 设置容器的重启策略。
--cpu-shares: 设置 CPU 共享权重。
--memory-swap: 设置容器的内存和交换空间限制。


✅ 示例
更新容器的内存限制
docker update --memory 512m my_container

更新容器的 CPU 配额
docker update --cpus 2 my_container

同时更新多个参数
docker update --memory 1g --cpus 1.5 my_container


✅ 注意事项
只能更新某些参数，不能更改容器的网络设置或环境变量等。
更新的操作会立即生效，但可能会影响容器的运行状态。
检查容器的状态，以确保更新不会导致不必要的中断。


✅ 主要用途
资源优化：根据实际需求调整容器的资源使用情况，以提高性能。
动态调整：在负载变化时，动态调整容器配置以适应不同的工作负载。

```
