# 1
```
docker ps 是用于查看当前 运行中的容器 的命令，支持很多选项，可以灵活地查看不同状态或筛选特定容器。

✅ 常用选项一览：
选项	全写	作用
-a	--all	显示 所有容器，包括已停止的
-q	--quiet	只显示容器 ID，常用于脚本
-l	--latest	只显示最近创建的一个容器
-n N	--last N	显示最近创建的 N 个容器
-f	--filter	根据条件过滤容器（比如状态、名称、镜像等）
--no-trunc		不截断输出内容（如容器名、命令等）
--format		自定义输出格式，适合脚本处理

✅ 常用 --filter 子选项（-f）
筛选键	说明
status=running	正在运行的容器（默认）
status=exited	已退出的容器
name=<字符串>	按容器名筛选
ancestor=<镜像名>	根据镜像名筛选容器
before=<容器名/ID>	启动时间早于指定容器的
since=<容器名/ID>	启动时间晚于指定容器的
label=<标签>	根据标签筛选容器

docker ps -a -f status=exited
列出所有退出的容器。

✅ --format 自定义输出（Go 模板语法）
比如只显示容器名和状态：

docker ps --format "table {{.Names}}\t{{.Status}}"

[root@master01 ~]# docker ps --format "table {{.Names}}\t{{.Status}}"
NAMES                                                                                                                         STATUS
k8s_speaker_speaker-vcrs8_metallb-system_179aa787-77ca-46fe-bafe-72d215403abb_315                                             Up 9 hours
k8s_csi-node-driver-registrar_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_61                     Up 9 hours
k8s_calico-csi_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_61                                    Up 9 hours
k8s_POD_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_62                                           Up 9 hours
k8s_calico-apiserver_calico-apiserver-8497bc86-8rvrj_calico-apiserver_acdbe531-9630-4098-9be4-a62642c03ecf_60                 Up 9 hours
k8s_POD_calico-apiserver-8497bc86-8rvrj_calico-apiserver_acdbe531-9630-4098-9be4-a62642c03ecf_60                              Up 9 hours
k8s_coredns_coredns-5dd5756b68-td8cg_kube-system_d5474822-48de-48ab-8af0-2987e979238b_61                                      Up 9 hours
k8s_calico-kube-controllers_calico-kube-controllers-689749b985-vxc45_calico-system_cdd871cf-381c-47be-91cc-ff54410d471e_257   Up 9 hours
k8s_calico-apiserver_calico-apiserver-8497bc86-85dwd_calico-apiserver_ce1608a8-1678-4a02-a741-62e297c3be29_48                 Up 9 hours
k8s_coredns_coredns-5dd5756b68-gxfxb_kube-system_26629fba-90c5-4b8b-89e9-845e5eea8a26_61                                      Up 9 hours
k8s_POD_calico-kube-controllers-689749b985-vxc45_calico-system_cdd871cf-381c-47be-91cc-ff54410d471e_62                        Up 9 hours
k8s_POD_coredns-5dd5756b68-td8cg_kube-system_d5474822-48de-48ab-8af0-2987e979238b_62                                          Up 9 hours
k8s_POD_calico-apiserver-8497bc86-85dwd_calico-apiserver_ce1608a8-1678-4a02-a741-62e297c3be29_48                              Up 9 hours
k8s_POD_coredns-5dd5756b68-gxfxb_kube-system_26629fba-90c5-4b8b-89e9-845e5eea8a26_62                                          Up 9 hours
k8s_calico-node_calico-node-ccvgm_calico-system_20c15e9a-b2a9-48db-a05a-d3f934a40fb1_61                                       Up 9 hours
k8s_kube-proxy_kube-proxy-4hplh_kube-system_8dccc6c7-dcfc-4692-a177-42b2a0477950_61                                           Up 9 hours
k8s_tigera-operator_tigera-operator-c78c656bf-f2nwt_tigera-operator_2438353e-18f2-4e73-b888-b2bd543880fa_74                   Up 9 hours
k8s_calico-typha_calico-typha-7b58689bb-b526h_calico-system_94008bb0-b268-48f8-863d-7b6ccaee3be2_61                           Up 9 hours
k8s_POD_kube-proxy-4hplh_kube-system_8dccc6c7-dcfc-4692-a177-42b2a0477950_61                                                  Up 9 hours
k8s_POD_tigera-operator-c78c656bf-f2nwt_tigera-operator_2438353e-18f2-4e73-b888-b2bd543880fa_60                               Up 9 hours
k8s_POD_calico-node-ccvgm_calico-system_20c15e9a-b2a9-48db-a05a-d3f934a40fb1_61                                               Up 9 hours
k8s_POD_calico-typha-7b58689bb-b526h_calico-system_94008bb0-b268-48f8-863d-7b6ccaee3be2_61                                    Up 9 hours
k8s_POD_speaker-vcrs8_metallb-system_179aa787-77ca-46fe-bafe-72d215403abb_60                                                  Up 9 hours
k8s_etcd_etcd-master01_kube-system_e26532cb1ed94764dadaa2579e8a74d8_312                                                       Up 9 hours
k8s_POD_etcd-master01_kube-system_e26532cb1ed94764dadaa2579e8a74d8_65                                                         Up 9 hours
k8s_kube-apiserver_kube-apiserver-master01_kube-system_56bcccf587f78f3d406131a1b6d80ccf_474                                   Up 9 hours
k8s_kube-scheduler_kube-scheduler-master01_kube-system_c9f81dbfd8699cf5ae2e019d9d45887d_79                                    Up 9 hours
k8s_kube-controller-manager_kube-controller-manager-master01_kube-system_def8515f36b69e03d1ba61d31b78d09f_81                  Up 9 hours
k8s_POD_kube-controller-manager-master01_kube-system_def8515f36b69e03d1ba61d31b78d09f_65                                      Up 9 hours
k8s_POD_kube-scheduler-master01_kube-system_c9f81dbfd8699cf5ae2e019d9d45887d_64                                               Up 9 hours
k8s_POD_kube-apiserver-master01_kube-system_56bcccf587f78f3d406131a1b6d80ccf_64                                               Up 9 hours

常用字段包括：
字段	含义
.ID	容器 ID
.Image	镜像名
.Command	启动命令
.CreatedAt	创建时间
.RunningFor	运行时长
.Ports	端口映射
.Status	状态
.Names	容器名

✅ 常见组合命令示例：

列出所有已停止容器的 ID：
docker ps -a -f status=exited -q

只显示最新启动的容器信息：
docker ps -l
[root@master01 ~]# docker ps -l
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS                     PORTS     NAMES
fe676dad0473   nginx     "sh"      8 hours ago   Exited (137) 8 hours ago             laughing_fermi


显示最近启动的 5 个容器：
docker ps -n 5
[root@master01 ~]# docker ps -n 5
CONTAINER ID   IMAGE          COMMAND                   CREATED       STATUS                     PORTS     NAMES
fe676dad0473   nginx          "sh"                      8 hours ago   Exited (137) 8 hours ago             laughing_fermi
01c8b06b6482   nginx          "/docker-entrypoint.…"   9 hours ago   Exited (0) 8 hours ago               mynginx
c4c84756c3b2   dacabb789863   "/speaker --port=747…"   9 hours ago   Up 9 hours                           k8s_speaker_speaker-vcrs8_metallb-system_179aa787-77ca-46fe-bafe-72d215403abb_315
fd3e014461e6   09a5a6ea58a4   "/usr/bin/node-drive…"   9 hours ago   Up 9 hours                           k8s_csi-node-driver-registrar_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_61
3dd0b922ff69   0fae09f861e3   "/usr/bin/csi-driver…"   9 hours ago   Up 9 hours                           k8s_calico-csi_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_61


显示带端口映射的容器：
docker ps --format "table {{.Names}}\t{{.Ports}}"
[root@master01 ~]# docker ps --format "table {{.Names}}\t{{.Ports}}"
NAMES                                                                                                                         PORTS
k8s_speaker_speaker-vcrs8_metallb-system_179aa787-77ca-46fe-bafe-72d215403abb_315
k8s_csi-node-driver-registrar_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_61
k8s_calico-csi_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_61
k8s_POD_csi-node-driver-q5vc7_calico-system_179165d8-c5dd-4095-98ac-1c40cb73d4ae_62
k8s_calico-apiserver_calico-apiserver-8497bc86-8rvrj_calico-apiserver_acdbe531-9630-4098-9be4-a62642c03ecf_60
k8s_POD_calico-apiserver-8497bc86-8rvrj_calico-apiserver_acdbe531-9630-4098-9be4-a62642c03ecf_60
k8s_coredns_coredns-5dd5756b68-td8cg_kube-system_d5474822-48de-48ab-8af0-2987e979238b_61
k8s_calico-kube-controllers_calico-kube-controllers-689749b985-vxc45_calico-system_cdd871cf-381c-47be-91cc-ff54410d471e_257
k8s_calico-apiserver_calico-apiserver-8497bc86-85dwd_calico-apiserver_ce1608a8-1678-4a02-a741-62e297c3be29_48
k8s_coredns_coredns-5dd5756b68-gxfxb_kube-system_26629fba-90c5-4b8b-89e9-845e5eea8a26_61
k8s_POD_calico-kube-controllers-689749b985-vxc45_calico-system_cdd871cf-381c-47be-91cc-ff54410d471e_62
k8s_POD_coredns-5dd5756b68-td8cg_kube-system_d5474822-48de-48ab-8af0-2987e979238b_62
k8s_POD_calico-apiserver-8497bc86-85dwd_calico-apiserver_ce1608a8-1678-4a02-a741-62e297c3be29_48
k8s_POD_coredns-5dd5756b68-gxfxb_kube-system_26629fba-90c5-4b8b-89e9-845e5eea8a26_62
k8s_calico-node_calico-node-ccvgm_calico-system_20c15e9a-b2a9-48db-a05a-d3f934a40fb1_61
k8s_kube-proxy_kube-proxy-4hplh_kube-system_8dccc6c7-dcfc-4692-a177-42b2a0477950_61
k8s_tigera-operator_tigera-operator-c78c656bf-f2nwt_tigera-operator_2438353e-18f2-4e73-b888-b2bd543880fa_74
k8s_calico-typha_calico-typha-7b58689bb-b526h_calico-system_94008bb0-b268-48f8-863d-7b6ccaee3be2_61
k8s_POD_kube-proxy-4hplh_kube-system_8dccc6c7-dcfc-4692-a177-42b2a0477950_61
k8s_POD_tigera-operator-c78c656bf-f2nwt_tigera-operator_2438353e-18f2-4e73-b888-b2bd543880fa_60
k8s_POD_calico-node-ccvgm_calico-system_20c15e9a-b2a9-48db-a05a-d3f934a40fb1_61
k8s_POD_calico-typha-7b58689bb-b526h_calico-system_94008bb0-b268-48f8-863d-7b6ccaee3be2_61
k8s_POD_speaker-vcrs8_metallb-system_179aa787-77ca-46fe-bafe-72d215403abb_60
k8s_etcd_etcd-master01_kube-system_e26532cb1ed94764dadaa2579e8a74d8_312
k8s_POD_etcd-master01_kube-system_e26532cb1ed94764dadaa2579e8a74d8_65
k8s_kube-apiserver_kube-apiserver-master01_kube-system_56bcccf587f78f3d406131a1b6d80ccf_474
k8s_kube-scheduler_kube-scheduler-master01_kube-system_c9f81dbfd8699cf5ae2e019d9d45887d_79
k8s_kube-controller-manager_kube-controller-manager-master01_kube-system_def8515f36b69e03d1ba61d31b78d09f_81
k8s_POD_kube-controller-manager-master01_kube-system_def8515f36b69e03d1ba61d31b78d09f_65
k8s_POD_kube-scheduler-master01_kube-system_c9f81dbfd8699cf5ae2e019d9d45887d_64
k8s_POD_kube-apiserver-master01_kube-system_56bcccf587f78f3d406131a1b6d80ccf_64
```
