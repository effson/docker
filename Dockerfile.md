# 1
```
[root@master01 Docker]# vim Dockerfile

Dockerfile:
FROM golang:1.18
LABEL maintainer="jeff"
ENV env1=v1
ENV env2=v2
LABEL myhello 1.0.0
LABEL env prod
RUN git clone https://gitee.com/nickdemo/helloworld.git
WORKDIR helloworld
RUN CGO_ENABLE=0 GOOS=linux GOARCH=amd64 go build -o app . 
EXPOSE 80
CMD ["./app","--param1=p1","--param2=p2"]
```
```
docker build -t hello:1.0.0 .

[root@master01 Docker]# docker build -t hello:1.0.0 .
[+] Building 8.2s (8/8) FINISHED                                                                                                                                    docker:
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 327B
 => [internal] load metadata for docker.io/library/golang:1.18
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [1/4] FROM docker.io/library/golang:1.18@sha256:50c889275d26f816b5314fc99f55425fa76b18fcaf16af255f5d57f09e1f48da
 => CACHED [2/4] RUN git clone https://gitee.com/nickdemo/helloworld.git
 => [3/4] WORKDIR helloworld
 => [4/4] RUN CGO_ENABLE=0 GOOS=linux GOARCH=amd64 go build -o app .
 => exporting to image
 => => exporting layers
 => => writing image sha256:b95573d806c65dd7279e973a31c13bf54ec7b1947a909afe87438cae47c15748
 => => naming to docker.io/library/hello:1.0.0
```

```
[root@master01 Docker]# docker images
REPOSITORY                                TAG        IMAGE ID       CREATED          SIZE
hello                                     1.0.0      b95573d806c6   30 seconds ago   1.06GB
```
```
[root@master01 Docker]# docker run -d -p 81:80 --name myhello hello:1.0.0
cd5b82c71ae1d385af42014087835017acc745e72b091d5bda60a2a479887e81
```

```
[root@master01 Docker]# docker ps
CONTAINER ID   IMAGE                       COMMAND                   CREATED          STATUS          PORTS                                       NAMES
cd5b82c71ae1   hello:1.0.0                 "./app --param1=p1 -…"   26 seconds ago   Up 25 seconds   0.0.0.0:81->80/tcp, :::81->80/tcp           myhello
```

```
[root@master01 Docker]# curl http://localhost:81
hello world[root@master01 Docker]# curl http://localhost:81/print/env
env list : env1 = v1 and env2 = v2[root@master01 Docker]#
[root@master01 Docker]# curl http://localhost:81/print/startup
start up params:     param1 = p1 and param2 = p2
```
#### 唯一的问题：<br>
镜像太大<br>
```
REPOSITORY                                TAG        IMAGE ID       CREATED          SIZE
hello                                     1.0.0      b95573d806c6   30 seconds ago   1.06GB
```

优化版本：<br>
```
FROM golang:1.18 as s0
# RUN git clone https://gitee.com/nickdemo/helloworld.git
WORKDIR helloworld
ADD ./helloworld ./
#RUN CGO_ENABLE=0 GOOS=linux GOARCH=amd64 go build -o app . 
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags="-s -w -extldflags '-static'" -o app .

RUN pwd

FROM alpine
LABEL maintainer="jeff"
ENV env1=v1
ENV env2=v2
LABEL myhello 1.0.0
LABEL env prod
COPY  --from=s0 /go/helloworld/app ./
EXPOSE 80
CMD ["./app","--param1=p1","--param2=p2"]



[root@master01 gcc]# docker rm myhellov2
myhellov2
[root@master01 gcc]# docker run -d -p 81:80 --name myhellov2 myhellov2:latest
66c86621367824b5f04d9a4a5c22bc7adc1d079477ca636e9b60f77e02fa01b5
[root@master01 gcc]# curl http://localhost:81/print/ping
hello world



```
