# 1
```
docker attach 是用于附加（连接）到一个正在运行的容器的标准输入/输出的命令，就像你正在使用容器内部的终端一样。

✅ 基本语法
docker attach <容器名或容器ID>

✅ 使用场景举例
用如下命令启动了一个前台运行的容器：
docker run -it --name test ubuntu

这个容器在运行 bash，并且你按 Ctrl+P Ctrl+Q 暂时退出了终端，但容器仍在运行。

此时可以使用：
docker attach test
重新回到这个容器的会话中。

✅ 特点总结
特点	说明
连入当前容器主进程的 stdin/stdout/stderr	
适用于 -it 启动的容器	
所见即所得（原样输出）	
不会启动新的进程（不像 docker exec）	
如果容器主进程退出，attach 会断开	
可以按下 Ctrl + P，再按 Ctrl + Q 退出 attach 而不停止容器	

⚠️ 注意事项

不是进入 shell 的手段
如果容器启动的是 nginx、redis 等非 shell 服务，attach 进去后你不会看到 shell。
输入会与容器共享
如果多个终端 attach 同一个容器，输入输出是共享的（容易混乱）
容易误退出容器
如果你在 attach 状态下按了 Ctrl+C，可能会终止容器主进程

✅ 和 docker exec -it 的区别
项目	docker attach	docker exec -it
接入	主进程	启动一个新的子进程（通常是 shell）
场景	恢复会话 / 查看容器日志	临时执行命令、调试
退出影响	Ctrl+C 会杀掉主进程	Ctrl+C 仅退出子进程
退出快捷键	Ctrl + P 再 Ctrl + Q	直接 exit 或 Ctrl+D
```
