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

```
docker build -t ... --target=s0
是用于 多阶段构建（multi-stage build） 中的一个选项，意思是：
只构建到名为 s0 的阶段为止，不会继续执行后面的阶段。
```

```
docker build -t
✅ -t <镜像名>:<标签>
用于给构建好的镜像命名
```

```
✅ docker build -t ... --cache-from <已有镜像>
 --cache-from <已有镜像>
用于加速构建过程，复用某个已有镜像的构建缓存。
默认情况下，docker build 每一步都会检查缓存（Docker 层），但如果你使用了 CI/CD 或构建环境变化频繁，Docker 可能无法直接命中缓存。

使用 --cache-from 可以显式指定某个镜像作为缓存来源。

例如：
docker build -t myapp:latest --cache-from myapp:latest .
表示：
如果 myapp:latest 这个镜像已经存在，那么构建时尽可能从这个镜像中复用层缓存。

✅ 实用场景
加速 CI 构建流程：如果你在 GitLab CI、GitHub Actions 中构建 Docker 镜像，并从远程仓库（如 Harbor、Docker Hub）拉取旧镜像作为缓存，加快构建速度。
多阶段构建优化：即使你使用了 --from=builder 等方式，也可以配合 --cache-from 保留构建依赖层的缓存。
```
