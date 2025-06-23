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


[root@master01 ~]# docker run -d -i -t --entrypoint sh nginx
fe676dad04735ba74aa60ccfa8c81fb3b801bc4122fcce31456be5a4f055203b
[root@master01 ~]# docker ps
CONTAINER ID   IMAGE                       COMMAND                   CREATED          STATUS          PORTS     NAMES
fe676dad0473   nginx                       "sh"                      12 seconds ago   Up 12 seconds   80/tcp    laughing_fermi


```
# 4

```
docker run -d --rm --health-cmd="curl http://localhost" --health-interval=30s --health-timeout=5s --health-retries=3 --health-start-period 10s  nginx

🧠 含义说明：
参数	说明
-d	后台运行容器
--rm	容器停止后自动删除
--health-cmd	执行 curl 访问 nginx 来检查健康状态
--health-interval=30s	每 30 秒检查一次
--health-timeout=5s	单次健康检查超时时间为 5 秒
--health-retries=3	连续失败 3 次判定为 unhealthy
--health-start-period 10s	容器启动后 10 秒内不判定失败（用于预热）

[root@master01 ~]# docker run -d --rm  --health-cmd="curl http://localhost" --health-interval=30s  --health-timeout=5s  --health-retries=3  --health-start-period 10s  nginx
d57a6995114ca7b89b0c754d65af7e1a7ecce2cd2c9fff52e719fde582e6da58

✅ 查看健康状态
运行后，可以用：

docker ps 查看 STATUS 字段：
[root@master01 ~]# docker ps
CONTAINER ID   IMAGE                       COMMAND                   CREATED          STATUS                   PORTS     NAMES
d57a6995114c   nginx                       "/docker-entrypoint.…"   2 minutes ago    Up 2 minutes (healthy)   80/tcp    dreamy_torvalds

也可以进一步查看详细健康信息：

docker inspect <容器ID或名称>
或更精准输出：

docker inspect --format='{{json .State.Health}}' <容器ID或名称> | jq

[root@master01 ~]# docker inspect --format='{{json .State.Health}}' d57a6995114c | jq
{
  "Status": "healthy",
  "FailingStreak": 0,
  "Log": [
    {
      "Start": "2025-06-23T23:24:25.741255624+08:00",
      "End": "2025-06-23T23:24:25.783943038+08:00",
      "ExitCode": 0,
      "Output": "  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0\r100   615  100   615    0     0  1048k      0 --:--:-- --:--:-- --:--:--  600k\n<!DOCTYPE html>\n<html>\n<head>\n<title>Welcome to nginx!</title>\n<style>\nhtml { color-scheme: light dark; }\nbody { width: 35em; margin: 0 auto;\nfont-family: Tahoma, Verdana, Arial, sans-serif; }\n</style>\n</head>\n<body>\n<h1>Welcome to nginx!</h1>\n<p>If you see this page, the nginx web server is successfully installed and\nworking. Further configuration is required.</p>\n\n<p>For online documentation and support please refer to\n<a href=\"http://nginx.org/\">nginx.org</a>.<br/>\nCommercial support is available at\n<a href=\"http://nginx.com/\">nginx.com</a>.</p>\n\n<p><em>Thank you for using nginx.</em></p>\n</body>\n</html>\n"
    },
    {
      "Start": "2025-06-23T23:24:55.784346747+08:00",
      "End": "2025-06-23T23:24:55.842291862+08:00",
      "ExitCode": 0,
      "Output": "  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0\r100   615  100   615    0     0   835k      0 --:--:-- --:--:-- --:--:--  600k\n<!DOCTYPE html>\n<html>\n<head>\n<title>Welcome to nginx!</title>\n<style>\nhtml { color-scheme: light dark; }\nbody { width: 35em; margin: 0 auto;\nfont-family: Tahoma, Verdana, Arial, sans-serif; }\n</style>\n</head>\n<body>\n<h1>Welcome to nginx!</h1>\n<p>If you see this page, the nginx web server is successfully installed and\nworking. Further configuration is required.</p>\n\n<p>For online documentation and support please refer to\n<a href=\"http://nginx.org/\">nginx.org</a>.<br/>\nCommercial support is available at\n<a href=\"http://nginx.com/\">nginx.com</a>.</p>\n\n<p><em>Thank you for using nginx.</em></p>\n</body>\n</html>\n"
    },
    {
      "Start": "2025-06-23T23:25:25.843458176+08:00",
      "End": "2025-06-23T23:25:25.891235877+08:00",
      "ExitCode": 0,
      "Output": "  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0\r100   615  100   615    0     0   999k      0 --:--:-- --:--:-- --:--:--  600k\n<!DOCTYPE html>\n<html>\n<head>\n<title>Welcome to nginx!</title>\n<style>\nhtml { color-scheme: light dark; }\nbody { width: 35em; margin: 0 auto;\nfont-family: Tahoma, Verdana, Arial, sans-serif; }\n</style>\n</head>\n<body>\n<h1>Welcome to nginx!</h1>\n<p>If you see this page, the nginx web server is successfully installed and\nworking. Further configuration is required.</p>\n\n<p>For online documentation and support please refer to\n<a href=\"http://nginx.org/\">nginx.org</a>.<br/>\nCommercial support is available at\n<a href=\"http://nginx.com/\">nginx.com</a>.</p>\n\n<p><em>Thank you for using nginx.</em></p>\n</body>\n</html>\n"
    },
    {
      "Start": "2025-06-23T23:25:55.891785413+08:00",
      "End": "2025-06-23T23:25:55.939013921+08:00",
      "ExitCode": 0,
      "Output": "  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!DOCTYPE html>\n<html>\n<head>\n<title>Welcome to nginx!</title>\n<style>\nhtml { color-scheme: light dark; }\nbody { width: 35em; margin: 0 auto;\nfont-family: Tahoma, Verdana, Arial, sans-serif; }\n</style>\n</head>\n<body>\n<h1>Welcome to nginx!</h1>\n<p>If you see this page, the nginx web server is successfully installed and\nworking. Further configuration is required.</p>\n\n<p>For online documentation and support please refer to\n<a href=\"http://nginx.org/\">nginx.org</a>.<br/>\nCommercial support is available at\n<a href=\"http://nginx.com/\">nginx.com</a>.</p>\n\n<p><em>Thank you for using nginx.</em></p>\n</body>\n</html>\n\r100   615  100   615    0     0   999k      0 --:--:-- --:--:-- --:--:--  600k\n"
    },
    {
      "Start": "2025-06-23T23:26:25.939745155+08:00",
      "End": "2025-06-23T23:26:26.009136641+08:00",
      "ExitCode": 0,
      "Output": "  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!DOCTYPE html>\n<html>\n<head>\n<title>Welcome to nginx!</title>\n<style>\nhtml { color-scheme: light dark; }\nbody { width: 35em; margin: 0 auto;\nfont-family: Tahoma, Verdana, Arial, sans-serif; }\n</style>\n</head>\n<body>\n<h1>Welcome to nginx!</h1>\n<p>If you see this page, the nginx web server is successfully installed and\nworking. Further configuration is required.</p>\n\n<p>For online documentation and support please refer to\n<a href=\"http://nginx.org/\">nginx.org</a>.<br/>\nCommercial support is available at\n<a href=\"http://nginx.com/\">nginx.com</a>.</p>\n\n<p><em>Thank you for using nginx.</em></p>\n</body>\n</html>\n\r100   615  100   615    0     0  1256k      0 --:--:-- --:--:-- --:--:--  600k\n"
    }
  ]
}
```
```
[root@master01 ~]# docker logs -f d57a6995114ca7b89b0c754d65af7e1a7ecce2cd2c9fff52e719fde582e6da58
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2025/06/23 15:36:08 [notice] 1#1: using the "epoll" event method
2025/06/23 15:36:08 [notice] 1#1: nginx/1.27.5
2025/06/23 15:36:08 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2025/06/23 15:36:08 [notice] 1#1: OS: Linux 3.10.0-1160.119.1.el7.x86_64
2025/06/23 15:36:08 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/06/23 15:36:08 [notice] 1#1: start worker processes
2025/06/23 15:36:08 [notice] 1#1: start worker process 29
2025/06/23 15:36:08 [notice] 1#1: start worker process 30
2025/06/23 15:36:08 [notice] 1#1: start worker process 31
2025/06/23 15:36:08 [notice] 1#1: start worker process 32
2025/06/23 15:36:08 [notice] 1#1: start worker process 33
2025/06/23 15:36:08 [notice] 1#1: start worker process 34
2025/06/23 15:36:08 [notice] 1#1: start worker process 35
2025/06/23 15:36:08 [notice] 1#1: start worker process 36
127.0.0.1 - - [23/Jun/2025:15:36:10 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:13 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:15 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:17 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:19 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:21 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:23 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:25 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:27 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:29 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:31 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:33 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:35 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:37 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:39 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:41 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:43 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:45 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"
127.0.0.1 - - [23/Jun/2025:15:36:47 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.88.1" "-"

```
