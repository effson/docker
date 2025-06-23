# 1
```
docker rm `docker ps -f status=exited -q`

✅ 批量删除所有已退出（exited）状态的容器
🔍 分解解释：
docker ps -f status=exited -q
列出所有 已退出的容器 ID（-q 是只显示 ID）：

[root@master01 ~]# docker ps -f status=exited -q
fe676dad0473
01c8b06b6482
2d233c5dfbfd
313191b7139e
fcea0c33aa8f
01602aad17b9
cecd2bb03d11
c3cc59482374
3f5bd0e78e50
755d28ff75e4
6a48902ef4a1
a96beed6c58d
ce8532205f5f
7adf41bf8fd0
71a55db418bb
b46aa7fa2e80
fee964bc5b27
d69ccab47d2c
4f4fed0c142c
5fd6e1b7d0ea
b78cf3e288fe
548d774c464f
adb3f531f58e
12d219e0b20b
9524ef0122b7
b16c492e67c6
b0de5a1fc425
ee5d24212f8f
c922acce317b
760bda0e8612
ce4b6e156ae5
3a255b7f78ae
14ec6aed58ce

docker ps -a -f status=exited -q
docker rm ...
用于删除一个或多个容器，后面跟的就是要删除的容器 ID 列表。

🧠 举例说明：
假设有以下容器：

docker ps -a
CONTAINER ID   STATUS
abc123         Exited (0) 1 hour ago
def456         Exited (1) 10 minutes ago
ghi789         Up 5 minutes
执行：

docker rm `docker ps -f status=exited -q`
将会删除 abc123 和 def456 两个退出状态的容器。
```
