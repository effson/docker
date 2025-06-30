# 1
Docker 中的 **<mark>数据卷（Volume）</mark>** 是用来持久化和共享容器数据的最常用机制。数据卷存在于宿主机上，但不随着容器的删除而删除，适合保存数据库数据、配置文件、上传文件等<br>
```
[root@master01 ~]# docker volume ls
DRIVER    VOLUME NAME
local     4af90a62fd984c5e01e4bed666d1eab814e5aea01c57bf4ad0986107a98fe8cf
```


## 创建卷
**<mark>docker volume create mydata</mark>**<br>
```
[root@master01 ~]# docker volume create mydata
mydata
[root@master01 ~]# docker volume ls
DRIVER    VOLUME NAME
local     4af90a62fd984c5e01e4bed666d1eab814e5aea01c57bf4ad0986107a98fe8cf
local     mydata
```
使用数据卷：<br>
```
[root@master01 ~]# docker run -d --rm -v /root/ngx -v mydata:/data --name ngx_vol  nginx:latest
373821d61f3bdda66c3266e34d3a79d35f73a2264c0ab2ace8673479013fc015

[root@master01 ~]# docker inspect ngx_vol
"Mounts": [
            {
                "Type": "volume",
                "Name": "mydata",
                "Source": "/var/lib/docker/volumes/mydata/_data",
                "Destination": "/data",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "9fef72dd0debdc0f8a38ac34cffb3555dc4811e1262834067b8c45dc6ecc1f4c",
                "Source": "/var/lib/docker/volumes/9fef72dd0debdc0f8a38ac34cffb3555dc4811e1262834067b8c45dc6ecc1f4c/_data",
                "Destination": "/root/ngx",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }

[root@master01 ~]# docker volume ls
DRIVER    VOLUME NAME
local     4af90a62fd984c5e01e4bed666d1eab814e5aea01c57bf4ad0986107a98fe8cf
local     9fef72dd0debdc0f8a38ac34cffb3555dc4811e1262834067b8c45dc6ecc1f4c
local     mydata

[root@master01 ~]# docker ps
CONTAINER ID   IMAGE                       COMMAND                   CREATED             STATUS             PORTS                                       NAMES
373821d61f3b   nginx:latest                "/docker-entrypoint.…"   About an hour ago   Up About an hour   80/tcp                                      ngx_vol

[root@master01 ~]# docker stop 373821d61f3b
373821d61f3b

[root@master01 ~]# docker volume ls
DRIVER    VOLUME NAME
local     4af90a62fd984c5e01e4bed666d1eab814e5aea01c57bf4ad0986107a98fe8cf
local     mydata
```
9fef72dd0debdc0f是临时volume，容器停止后(因为有--rm所以被删除了)该volume就被删除了<br>
