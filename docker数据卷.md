# 1
Docker 中的 数据卷（Volume） 是用来持久化和共享容器数据的最常用机制。数据卷存在于宿主机上，但不随着容器的删除而删除，适合保存数据库数据、配置文件、上传文件等<br>
```
[root@master01 ~]# docker volume ls
DRIVER    VOLUME NAME
local     4af90a62fd984c5e01e4bed666d1eab814e5aea01c57bf4ad0986107a98fe8cf
```
## 创建卷
<mark>docker volume create mydata</mark><br>
```
[root@master01 ~]# docker volume create mydata
mydata
[root@master01 ~]# docker volume ls
DRIVER    VOLUME NAME
local     4af90a62fd984c5e01e4bed666d1eab814e5aea01c57bf4ad0986107a98fe8cf
local     mydata

```
<br>
<br>
