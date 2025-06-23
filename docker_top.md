# 1
```
docker top 是用来查看某个正在运行的 容器中正在执行的进程列表，类似于 Linux 中的 top 或 ps 命令。

✅ 基本语法
docker top <容器名称或容器ID>

✅ 示例：
假设你有一个正在运行的 nginx 容器叫 mynginx：
docker top mynginx

输出示例
UID       PID       PPID      C       STIME     TTY       TIME       CMD
root      12345     12320     0       10:00     ?         00:00:00   nginx: master process nginx -g daemon off;
nginx     12367     12345     0       10:00     ?         00:00:00   nginx: worker process

你可以看到容器内部所有的进程，包括：
容器内运行的命令（如 nginx、bash、python）
每个进程的 PID、PPID、UID、CPU 时间等信息

✅ 可选参数（高级）
你也可以自定义 ps 命令样式（仅适用于 Linux 宿主机）：
docker top mynginx -eo pid,cmd

这会按指定格式输出容器中进程信息（与 ps -eo 类似），但注意这依赖于宿主机支持的 ps。

✅ 注意事项
docker top 查看的是容器内的进程，不是宿主机上的。
只能用于正在运行的容器，停止的容器无进程可查。
不显示 CPU 内存等实时资源使用情况（如需，请使用 docker stats）。
```

# 2
```
docker top mynginx
和
docker exec mynginx ps aux  或  docker exec -it mynginx ps -ef
这两种方式看到的进程列表有什么不同？

✅ docker top mynginx
这是 Docker 引擎调用宿主机的 ps 命令，去查看该容器所属的 Linux namespace 中的进程。它依赖于宿主机提供的 /proc 系统，并不是容器内执行的 ps。
显示信息由宿主机决定（默认 ps -ef）
有时可以看到 更多完整字段，比如宿主机用户（UID）
输出字段与宿主机 ps 命令格式一致

✅ 2. docker exec mynginx ps aux 或 ps -ef
这是在容器内部真正运行 ps 命令，它看到的是容器进程视角下的进程列表。
它只能看到容器中“本地”的进程
输出格式由容器中安装的 ps 决定（比如 Ubuntu vs Alpine）
如果容器基于 Alpine，有可能只能看到非常精简的进程（因为没有 full 版的 ps）

✅ ✅ 核心区别总结：
对比项	docker top	docker exec ... ps
执行位置	宿主机执行 ps，查看容器进程 namespace	容器内执行 ps
工具来源	宿主机的 ps	容器中安装的 ps
输出格式	通常更详细	可能更少字段（如 Alpine）
用户字段	取决于宿主机	取决于容器内 ps 支持
能力	不依赖容器内容	容器必须安装 ps 工具
```
