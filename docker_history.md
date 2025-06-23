```
docker history命令用于查看镜像的构建历史，包括每一层（layer）的创建信息、命令、大小等。

✅ 命令格式：

docker history nginx
或指定标签：
docker history nginx:latest

📌 示例输出：

[root@master01 ~]# docker history nginx
IMAGE          CREATED        CREATED BY                                       SIZE      COMMENT
1e5f3c5b981a   2 months ago   CMD ["nginx" "-g" "daemon off;"]                 0B        buildkit.dockerfile.v0
<missing>      2 months ago   STOPSIGNAL SIGQUIT                               0B        buildkit.dockerfile.v0
<missing>      2 months ago   EXPOSE map[80/tcp:{}]                            0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENTRYPOINT ["/docker-entrypoint.sh"]             0B        buildkit.dockerfile.v0
<missing>      2 months ago   COPY 30-tune-worker-processes.sh /docker-ent…   4.62kB    buildkit.dockerfile.v0
<missing>      2 months ago   COPY 20-envsubst-on-templates.sh /docker-ent…   3.02kB    buildkit.dockerfile.v0
<missing>      2 months ago   COPY 15-local-resolvers.envsh /docker-entryp…   389B      buildkit.dockerfile.v0
<missing>      2 months ago   COPY 10-listen-on-ipv6-by-default.sh /docker…   2.12kB    buildkit.dockerfile.v0
<missing>      2 months ago   COPY docker-entrypoint.sh / # buildkit           1.62kB    buildkit.dockerfile.v0
<missing>      2 months ago   RUN /bin/sh -c set -x     && groupadd --syst…   118MB     buildkit.dockerfile.v0
<missing>      2 months ago   ENV DYNPKG_RELEASE=1~bookworm                    0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV PKG_RELEASE=1~bookworm                       0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV NJS_RELEASE=1~bookworm                       0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV NJS_VERSION=0.8.10                           0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV NGINX_VERSION=1.27.5                         0B        buildkit.dockerfile.v0
<missing>      2 months ago   LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      2 months ago   # debian.sh --arch 'amd64' out/ 'bookworm' '…   74.8MB    debuerreotype 0.15

各列含义：
列名	含义
IMAGE	当前层的镜像 ID，如果是 <missing>，表示它被合并进其他层了
CREATED	创建时间
CREATED BY	构建该层使用的命令（Dockerfile 中的那一行）
SIZE	每一层的大小
COMMENT	构建时的注释（通常为空）

🚀 用途场景：
查看镜像是如何一步步构建的

分析镜像大小构成（哪个步骤占空间最多）

审计 Dockerfile 中每个命令对镜像的影响
```
