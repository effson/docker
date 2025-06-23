# 1
```
docker run -d -p 是 docker run 命令中的两个常用选项，分别是：

✅ -d：Detached 模式
表示容器后台运行（detached mode），不会在当前终端中输出容器的日志或信息。

docker run -d nginx
容器会在后台运行，不会占用你的终端。

✅ -p：端口映射（port mapping）
用于将 宿主机端口 映射到 容器内端口。格式如下：

-p <宿主机端口>:<容器端口>
例如：
docker run -d -p 8080:80 nginx
表示将宿主机的 8080 端口映射到容器内部的 80 端口（Nginx 默认监听端口），然后你就可以通过访问：

http://localhost:8080
来访问容器内运行的 Nginx 服务。

✅ 综合示例

docker run -d -p 8080:80 --name web nginx

-d：后台运行
-p 8080:80：将宿主机 8080 端口映射到容器 80 端口
--name web：容器名为 web
nginx：使用的镜像

需要进一步说明 -p 支持多个端口映射，如：
docker run -d -p 8080:80 -p 443:443 nginx

还支持指定绑定的主机 IP，例如：
docker run -d -p 127.0.0.1:8080:80 nginx
表示该端口只监听在本机环回地址（localhost）上，外部无法访问。
```

# 2
```
docker run -d -P （大写P）：

✅ -P（大写 P）：自动分配端口（Publish all exposed ports）
表示 Docker 会将容器中所有通过 EXPOSE 指令声明的端口，自动映射到宿主机的随机高位端口（通常是 32768~60999 之间）。

📌 举个例子
运行：
docker run -d -P --name mynginx nginx

因为 nginx 镜像在 Dockerfile 中写了：EXPOSE 80 443
这个命令会：把容器的 80 端口自动映射到宿主机的某个随机端口（比如 32769）
把容器的 443 端口自动映射到另一个随机端口（比如 32770）

通过以下命令查看具体分配了哪些端口：
docker ps
输出示例：
CONTAINER ID   IMAGE   ...   PORTS
abcdef123456   nginx   ...   0.0.0.0:32769->80/tcp, 0.0.0.0:32770->443/tcp
就可以通过浏览器访问：http://localhost:32769来访问 Nginx 服务。
```

# 3
```
docker run -d -i -t --entrypoint sh nginx
含义如下：

✅ 各个参数解释：
参数	作用
-i	保持标准输入（stdin）打开，即使不连接
-t	分配一个伪终端（tty），通常和 -i 一起用
--entrypoint sh	覆盖镜像默认的启动命令（CMD/ENTRYPOINT），改为执行 sh
nginx	镜像名，表示用 nginx 镜像来启动容器

🧠 实际效果：
以后台方式运行一个 Nginx 镜像，但启动的并不是 Nginx，而是一个 sh（shell 进程）。
这样做的好处是可以进入这个容器后手动执行命令，比如调试或查看镜像内部内容。


```
