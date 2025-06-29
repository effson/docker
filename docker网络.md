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
### 3.1 客户端和服务端通讯<br>
#### ✅ 通信方式：<br>
**Unix Domain Socket（本地通信）**<br>
**TCP Socket（远程通信）**<br>

#### ✅ 默认情况：使用 Unix Socket<br>
在 Linux 上，Docker 默认使用：<br>
```
/var/run/docker.sock
```
这是一个 Unix 域套接字 文件，客户端通过它与 dockerd 守护进程进行本地通信。<br>

#### 🔍 示例：<br>
```
docker ps
```
这条命令实际上会通过 HTTP 请求（如 GET /containers/json）发给 dockerd，只不过是通过 unix:///var/run/docker.sock 而不是 TCP。<br>

可用 curl 验证：<br>
```
curl --unix-socket /var/run/docker.sock http://localhost/version

[root@master01 gcc]# curl --unix-socket /var/run/docker.sock http://localhost/version
{"Platform":{"Name":"Docker Engine - Community"},"Components":[{"Name":"Engine","Version":"26.1.4","Details":{"ApiVersion":"1.45","Arch":"amd64","BuildTime":"2024-06-05T11:31:02.000000000+00:00","Experimental":"false","GitCommit":"de5c9cf","GoVersion":"go1.21.11","KernelVersion":"3.10.0-1160.119.1.el7.x86_64","MinAPIVersion":"1.24","Os":"linux"}},{"Name":"containerd","Version":"v1.7.23","Details":{"GitCommit":"57f17b0a6295a39009d861b89e3b3b87b005ca27"}},{"Name":"runc","Version":"1.2.0","Details":{"GitCommit":"v1.2.0-0-g0b9fa21b"}},{"Name":"docker-init","Version":"0.19.0","Details":{"GitCommit":"de40ad0"}}],"Version":"26.1.4","ApiVersion":"1.45","MinAPIVersion":"1.24","GitCommit":"de5c9cf","GoVersion":"go1.21.11","Os":"linux","Arch":"amd64","KernelVersion":"3.10.0-1160.119.1.el7.x86_64","BuildTime":"2024-06-05T11:31:02.000000000+00:00"}
```

#### ✅ 远程情况：使用 TCP Socket<br>
Docker 也支持通过 TCP 网络 来远程控制 Docker 守护进程。<br>
```
docker -H tcp://192.168.1.100:2375 ps
```
这会连接到远程主机 192.168.1.100 上的 Docker 守护进程（监听 TCP 端口 2375）。<br>

⚠️ 安全提示：<br>
tcp://0.0.0.0:2375 是明文通信，默认不安全！<br>

正确做法是使用 tcp://host:2376 并启用 TLS 加密通信。<br>

✅ 总结对比<br>
```
通信方式	描述	默认？	是否加密
Unix Socket	本地使用 /var/run/docker.sock	✅ 默认	❌ 不加密（但本地安全）
TCP Socket	可远程连接 Docker 引擎	❌ 需配置	✅ 支持 TLS 加密
```
✅ 配置远程监听（补充）<br>
编辑 /etc/docker/daemon.json：<br>
```
{
  "hosts": ["unix:///var/run/docker.sock", "tcp://0.0.0.0:2375"]
}
```
然后重启 Docker 服务：<br>
```
systemctl daemon-reexec
systemctl restart docker
```
# docker网络<br>
Docker 的网络管理为容器提供了多种灵活的网络连接方式，使容器之间、容器与外部网络之间能够通信<br>
## 1 Docker 网络的分类<br>
Docker 默认提供以下几种网络模式（可以通过 docker network ls 查看）：<br>
```
网络模式	      作用	                        是否默认	              特点
bridge	       Docker 默认的容器间通信网络	   ✅	                容器间可以通信，宿主机访问需端口映射
host	      使用宿主机网络栈	                   ❌               没有隔离，性能高，但端口可能冲突
none	      完全不配置网络	                   ❌	               容器无法联网，用于自定义网络
overlay	      跨主机容器通信（Swarm 模式）	  ❌	                用于多主机集群
macvlan	      给容器分配真实网卡 IP	           ❌	               容器像物理主机一样接入局域网
```
```
[root@master01 gcc]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
c273e32d6ea9   bridge    bridge    local
648ddf46838f   host      host      local
5115630d493c   none      null      local
```
## 2 容器的网络连接机制<br>
### 2.1 默认 bridge 模式工作原理<br>
每次启动容器时，Docker 会：<br>
给容器分配一个虚拟网卡（veth pair）<br>
一端连接容器内部，另一端接入 docker0 网桥<br>
容器默认通过 NAT 上网（和宿主机共享 IP）<br>
宿主机中有个叫 docker0 的虚拟网桥，就是 Docker 默认创建的 bridge 网络。<br>

### 2.2 创建自定义 bridge 网络<br>
```
docker network create my-net

[root@master01 gcc]# docker network create my-net
618fdf2bb876deeb8943e57373e5b01220edfbcadcd7d02254e28a008accc53b
[root@master01 gcc]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
c273e32d6ea9   bridge    bridge    local
648ddf46838f   host      host      local
618fdf2bb876   my-net    bridge    local
5115630d493c   none      null      local

```
```
[root@master01 gcc]# docker network inspect 618fdf2bb876
[
    {
        "Name": "my-net",
        "Id": "618fdf2bb876deeb8943e57373e5b01220edfbcadcd7d02254e28a008accc53b",
        "Created": "2025-06-29T22:20:29.296097048+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```
```
[root@master01 gcc]# ifconfig
br-618fdf2bb876: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.18.0.1  netmask 255.255.0.0  broadcast 172.18.255.255
        ether 02:42:89:06:87:36  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 227 overruns 0  carrier 0  collisions 0
```
查看当前 Linux 系统中虚拟网桥（Bridge）配置情况:<br>
```
[root@master01 gcc]# brctl show
bridge name     bridge id               STP enabled     interfaces
br-618fdf2bb876         8000.024289068736       no
docker0         8000.0242c2ebb1f8       no              vethee37109
virbr0          8000.52540001dd45       yes             virbr0-nic

bridge name	网桥的名字（相当于一个虚拟交换机）
bridge id	网桥的唯一标识（MAC 相关）
STP enabled	是否启用了 STP（生成树协议，用于防环）
interfaces	接入该网桥的接口（网卡、veth、虚拟接口等）
```
### 2.3 容器连接:<br>
容器网络连接:<br>
```
docker network connect my-net <container1>
docker network connect my-net <container2>
```
把两个已经运行的容器连接到名为 my-net 的 Docker 自定义网络上，从而使它们可以直接通信<br>
<br>
进入容器 1 测试：<br>
```
docker exec -it container1 sh
ping container2
```
可以 ping 通,现在都在 my-net 网络中<br>
### 2.4 子网隔离:<br>
```
docker run -d --name mynginx nginx:latest
docker network disconnect bridge <container1>
docker network disconnect bridge <container2>
```
```
docker exec -it mynginx bash
ping container1
```
ping不通！<br>
