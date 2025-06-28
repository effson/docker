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
# 2
```
FROM golang:1.18 as s0
# RUN git clone https://gitee.com/nickdemo/helloworld.git
ONBUILD ARG wd src dst
ONBUILD WORKDIR $wd
#ADD ./helloworld ./
ONBUILD ADD $src $dst
#RUN CGO_ENABLE=0 GOOS=linux GOARCH=amd64 go build -o app . 
ONBUILD RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags="-s -w -extldflags '-static'" -o app .

✅ 含义：
ONBUILD 指令会延迟执行。它本身在当前 Dockerfile 构建时 不会生效，而是会被记录下来。当这个镜像被用作其它
Dockerfile 的基础镜像（即 FROM <这个镜像>）时，才会自动触发执行这些指令。

[root@master01 gcc]# docker build -t mygolang:1.0.0 -f Dockerfilev3 .
[+] Building 2.8s (6/6) FINISHED                                                                                                                            docker:default
 => [internal] load build definition from Dockerfilev3                                                                                                                0.0s
 => => transferring dockerfile: 376B                                                                                                                                  0.0s
 => [internal] load metadata for docker.io/library/golang:1.18                                                                                                        2.8s
 => [auth] library/golang:pull token for registry-1.docker.io                                                                                                         0.0s
 => [internal] load .dockerignore                                                                                                                                     0.0s
 => => transferring context: 2B                                                                                                                                       0.0s
 => CACHED [1/1] FROM docker.io/library/golang:1.18@sha256:50c889275d26f816b5314fc99f55425fa76b18fcaf16af255f5d57f09e1f48da                                           0.0s
 => exporting to image                                                                                                                                                0.0s
 => => exporting layers                                                                                                                                               0.0s
 => => writing image sha256:ec587782c2748f8fee81a93f41331603152bb4d258914ff5b8d7c148fd08fdd9                                                                          0.0s
 => => naming to docker.io/library/mygolang:1.0.0                                                                                                                     0.0s

[root@master01 gcc]# docker images
REPOSITORY                                TAG        IMAGE ID       CREATED         SIZE
myhellov2                                 latest     d4d7a0ffdbdb   26 hours ago    14.4MB
<none>                                    <none>     be0f947e22aa   26 hours ago    17.2MB
192.168.23.150:5000/nginx                 latest     1e5f3c5b981a   2 months ago    192MB
jeff/nginx                                latest     1e5f3c5b981a   2 months ago    192MB
nginx                                     latest     1e5f3c5b981a   2 months ago    192MB
registry                                  latest     3dec7d02aaea   2 months ago    57.7MB
quay.io/tigera/operator                   v1.36.5    dc4a8a56c133   4 months ago    71.5MB
calico/typha                              v3.29.2    1d6f9d005866   4 months ago    73.8MB
calico/node-driver-registrar              v3.29.2    09a5a6ea58a4   4 months ago    31.8MB
calico/csi                                v3.29.2    0fae09f861e3   4 months ago    18.9MB
calico/pod2daemon-flexvol                 v3.29.2    441bf8ace5b7   4 months ago    13.9MB
calico/node                               v3.29.2    048bf7af1f8c   4 months ago    372MB
calico/kube-controllers                   v3.29.2    f6a228558381   4 months ago    82.4MB
calico/cni                                v3.29.2    cda13293c895   4 months ago    220MB
calico/apiserver                          v3.29.2    d27fc480d1ad   4 months ago    104MB
quay.io/metallb/speaker                   v0.14.9    dacabb789863   6 months ago    127MB
registry.k8s.io/kube-apiserver            v1.28.15   9dc6939e7c57   8 months ago    125MB
registry.k8s.io/kube-scheduler            v1.28.15   9d3465f8477c   8 months ago    59.3MB
registry.k8s.io/kube-proxy                v1.28.15   ba6d7f8bc25b   8 months ago    81.8MB
registry.k8s.io/kube-controller-manager   v1.28.15   10541d8af03f   8 months ago    121MB
registry.k8s.io/etcd                      3.5.15-0   2e96e5913fc0   11 months ago   148MB
quay.io/prometheus/node-exporter          v1.8.2     71dc9668b154   11 months ago   23.3MB
registry                                  2          26b2eb03618e   21 months ago   25.4MB
registry.k8s.io/coredns/coredns           v1.10.1    ead0a4a53df8   2 years ago     53.6MB
mygolang                                  1.0.0      ec587782c274   2 years ago     965MB
registry.k8s.io/pause                     3.9        e6f181688397   2 years ago     744kB
可以看到已经构建mygolang:1.0.0：
mygolang                                  1.0.0      ec587782c274   2 years ago     965MB



FROM alpine
LABEL maintainer="jeff"
ENV env1=v1
ENV env2=v2
LABEL myhello 1.0.0
LABEL env prod
COPY  --from=s0 /go/helloworld/app ./
EXPOSE 80
CMD ["./app","--param1=p1","--param2=p2"]
```
