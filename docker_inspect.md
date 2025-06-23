# 1
```
docker inspect 是 Docker 中非常强大的调试和信息查看工具，用于查看镜像、容器、网络、卷等对象的详细元数据（以 JSON 格式）。

✅ 基本语法：
docker inspect <对象类型> <名称或ID>
可以用于：容器、镜像、网络、volume 等,默认返回的是 JSON 格式的完整信息

✅ 示例一：查看容器信息
docker inspect mycontainer

会输出一个非常详细的 JSON 对象，包含：
字段	含义
.Id	容器 ID
.Name	容器名称
.State.Status	当前状态（running, exited, etc.）
.State.Health	健康检查状态（如果配置了）
.Config.Image	使用的镜像
.NetworkSettings.Ports	端口映射
.Mounts	挂载的卷
.HostConfig	宿主机相关配置，比如内存限制、网络模式等

✅ 查看镜像信息
docker inspect nginx
可以看到镜像层、创建者、标签、EXPOSE 端口等信息。

✅ 自定义输出（常用）
用 --format 提取关键字段：
docker inspect --format '{{.State.Status}}' mycontainer
输出容器当前状态（如 running）。

再比如：
docker inspect --format '{{.NetworkSettings.IPAddress}}' mycontainer
提取容器 IP 地址（注意：bridge 模式时才有值）。

✅ 结合 jq 美化 JSON 输出（可选）
docker inspect mycontainer | jq
前提是你已安装 jq 工具。

✅ 支持的对象类型包括：
docker inspect container <ID|NAME>
docker inspect image <IMAGE>
docker inspect network <NETWORK>
docker inspect volume <VOLUME>

例如：
docker inspect --type=network bridge

🧠 常见用途：
查看容器的健康状态
查看容器挂载了哪些卷
获取容器的 IP、端口映射
查看镜像是从哪个层构建的
获取 volume 的挂载点
```
