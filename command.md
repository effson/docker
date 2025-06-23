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
