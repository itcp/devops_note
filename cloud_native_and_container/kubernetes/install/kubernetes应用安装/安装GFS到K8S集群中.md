[TOC]

# 准备

为了保证Pod能够正常使用GFS作为后端存储，要每台Pod的节点上安装GFS的客户端工具

```
yum install -y glusterfs glusterfs-fuse
```



```
kubectl label node k8s-node01 storagenode=glusterfsnode/k8s-node01 labeled

kubectl label node k8s-master01 storagenode=glusterfsnode/k8s-master01 labeled
```



```
modprobe dm_snapshot
```





# 创建GFS集群

采用容器方式部署GFS集群

采用DaemonSet方式，同时保证已经打上标签的节点上都运行一个GFS服务，并且均有提供存储的磁盘

```
wget https://github.com/heketi/heketi/releases/download/v7.0.0/heketi-client-v7.0.0.linux.amd64.tar.gz
```



创建集群

```
pwd
kubectl create -f glusterfs-daemonset.json
created
pwd
```

创建的Namespace 为default ,可按需更改

可使用gluster/gluster-centos:gluster3ul2_centos7 镜像



查看GFS Pods：

```
kubectl get pods -l glusterfs-node-daemonset
```





# 创建Heketi服务





#



#



#



#