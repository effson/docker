# docker是什么？<br>
## 1 Docker 是一个开源的容器化平台软件<br>
打包、分发和运行应用程序，可以把一个应用和它的<mark>所有依赖环境（代码、库、配置、运行时等）</mark>打包成一个轻量级、可移植的容器（Container），从而实现<mark> “一次构建，到处运行” </mark> 的目标。<br>
## 2 Docker 是一个进程管理软件<br>
Docker 是一个容器化平台，它利用 Linux 内核提供的机制（如 Namespace 和 Cgroup），来实现对应用<mark>进程的隔离、资源限制、文件系统封装、网络隔离</mark>等功能。<br>
Docker 底层是一个进程隔离和管理工具，但它构建了一整套用于软件构建、封装、交付、部署的完整容器生态系统，远远超出传统意义的“进程管理软件”<br>
## 3 Docker软件<br>
包括客户端和服务端：
```
Client: Docker Engine - Community
 Version:    26.1.4
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.14.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.27.1
    Path:     /usr/libexec/docker/cli-plugins/docker-compose
```
```
Server:
 Containers: 66
  Running: 32
  Paused: 0
  Stopped: 34
 Images: 24
 Server Version: 26.1.4
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: runc io.containerd.runc.v2
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 57f17b0a6295a39009d861b89e3b3b87b005ca27
 runc version: v1.2.0-0-g0b9fa21b
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
 Kernel Version: 3.10.0-1160.119.1.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 7.62GiB
 Name: master01
 ID: b5c87eec-d3c0-43dc-abd0-607510c49777
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Username: sjeffson
 Experimental: false
 Insecure Registries:
  192.168.23.150:5000
  127.0.0.0/8
 Live Restore Enabled: false

```
