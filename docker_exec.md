# 1
```
docker exec 用于在正在运行的容器中执行命令，是容器交互和调试中最常用的命令之一。它也支持一些非常实用的选项。

✅ docker exec 常用选项及其作用
选项	说明
-i	保持标准输入打开（stdin），适用于交互式命令
-t	分配伪终端（tty），用于模拟终端环境（如 bash、sh）
-u	以指定的用户（UID 或用户名）运行命令
-e	设置环境变量（多个 -e 可设置多个变量）
-w	指定执行命令时的工作目录
--privileged	给予额外的内核能力（很少用）
--detach, -d	在后台运行命令
--detach-keys	自定义退出键（默认是 Ctrl+P+Q）
--env-file	从文件加载环境变量（用于设置多个变量）

✅ 常用示例
1. 进入容器交互式 shell
docker exec -it mycontainer bash

或如果容器没有 bash（如 Alpine）：
docker exec -it mycontainer sh


2. 以某个用户身份执行命令
docker exec -u www-data mycontainer whoami


3. 设置环境变量
docker exec -e VAR1=abc -e VAR2=123 mycontainer env


4. 指定工作目录再执行命令
docker exec -w /usr/share/nginx/html mycontainer ls


5. 后台执行（无输出）
docker exec -d mycontainer touch /tmp/test.txt


6. 自定义退出快捷键（交互模式时）
docker exec --detach-keys="ctrl-x,x" -it mycontainer bash


✅ 查看帮助文档
你可以随时运行：

docker exec --help
查看更多详细选项说明。
```

# 2
```
1. 在容器中执行 ls
docker exec mycontainer ls


2. 执行 pwd 查看工作目录
docker exec mycontainer pwd


3. 想进入交互式终端执行命令
docker exec -it mycontainer sh

或如果容器支持 bash：
docker exec -it mycontainer bash
```
