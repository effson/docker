# 1
```
✅ docker pause
暂停容器内所有进程（实际上是向它们发送 SIGSTOP 信号，挂起执行），使用 Linux 的 cgroups freezer 实现。

docker pause <容器名或容器ID>

容器仍然在 docker ps 中显示为 Up (Paused)
容器的进程挂起，不会继续执行
网络连接挂起、服务不响应请求
```

# 2
```
✅ docker unpause
恢复已 pause 的容器进程（相当于发送 SIGCONT 信号）。

docker unpause <容器名或容器ID>

恢复后容器恢复正常运行。

```
