

## 基本组件的安装



**安装docker**:

```
yum -y install docker

[root@iZ8vbi4t4rsi4kh859l777Z ~]# rpm -qa|grep docker
docker-client-1.13.1-109.gitcccb291.el7.centos.x86_64
docker-common-1.13.1-109.gitcccb291.el7.centos.x86_64
docker-1.13.1-109.gitcccb291.el7.centos.x86_64
[root@iZ8vbi4t4rsi4kh859l777Z ~]# 
```



**安装Kubernetes组件:**



用阿里云源

```
[root@iZ8vbi4t4rsi4kh859l777Z ~]# cat <<EOF > /etc/yum.repos.d/kubernetes.repo
> [kubernetes]
> name=Kubernetes
> baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
> enabled=1
> gpgcheck=1
> repo_gpgcheck=1
> gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
> 
> EOF
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum list kubeadm
kubernetes/signature                                                           |  454 B  00:00:00     
Retrieving key from https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
Importing GPG key 0xA7317B0F:
 Userid     : "Google Cloud Packages Automatic Signing Key <gc-team@google.com>"
 Fingerprint: d0bc 747f d8ca f711 7500 d6fa 3746 c208 a731 7b0f
 From       : https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
Is this ok [y/N]: yRetrieving key from https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
kubernetes/signature                                                           | 1.4 kB  00:00:30 !!! 
kubernetes/primary                                                             |  66 kB  00:00:00     
kubernetes                                                                                    481/481
Available Packages
kubeadm.x86_64                                   1.18.0-0                                   kubernetes
[root@iZ8vbi4t4rsi4kh859l777Z ~]#
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum list kubeadm-1.14*
Available Packages
kubeadm.x86_64                                  1.14.10-0                                   kubernetes
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum list kubectl-1.14*
Available Packages
kubectl.x86_64                                  1.14.10-0                                   kubernetes
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum list kubelet-1.14*
Available Packages
kubelet.x86_64                                  1.14.10-0                                   kubernetes
[root@iZ8vbi4t4rsi4kh859l777Z ~]#
```



```
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum -y install kubeadm-1.14.10
...
...
[root@iZ8vbi4t4rsi4kh859l777Z ~]# rpm -qa|grep kube*
kubectl-1.18.0-0.x86_64
kubernetes-cni-0.7.5-0.x86_64
kubeadm-1.14.10-0.x86_64
kubelet-1.18.0-0.x86_64
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum remove -y kubectl
....
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum remove -y kubelet
...
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum install -y kubelet-1.14.10
...
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum install -y kubectl-1.14.10
[root@iZ8vbi4t4rsi4kh859l777Z ~]# yum -y install kubeadm-1.14.10
```

可以看看 kubeadm、kubernetes、kubectl、kubelet都安装好了



启动docker

```
[root@iZ8vbi4t4rsi4kh859l777Z ~]# systemctl enable --now docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@iZ8vbi4t4rsi4kh859l777Z ~]#
[root@iZ8vbi4t4rsi4kh859l777Z ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2020-03-27 23:32:41 CST; 20min ago
     Docs: http://docs.docker.com
 Main PID: 1745 (dockerd-current)
   CGroup: /system.slice/docker.service
           ├─1745 /usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-run...
           └─1752 /usr/bin/docker-containerd-current -l unix:///var/run/docker/libcontainerd/docker...

Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.391663701..."
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.419263855..."
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.419894675..."
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.428086342..."
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.481093229..."
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.509068389..."
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.544523949..."
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.544568136...1
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z systemd[1]: Started Docker Application Container Engine.
Mar 27 23:32:41 iZ8vbi4t4rsi4kh859l777Z dockerd-current[1745]: time="2020-03-27T23:32:41.552578991..."
Hint: Some lines were ellipsized, use -l to show in full.
[root@iZ8vbi4t4rsi4kh859l777Z ~]# 
```



配置Kubelet:

```
DOCKER_CGROUPS=$(docker info | grep 'Cgroup' | cut -d' ' -f3)
echo $DOCKER_CGROUPS
cat >/etc/sysconfig/kubelet<<EOF
KUBELET_EXTRA_ARGS="--cgroup-driver=$DOCKER_CGROUPS 
--pod-infra-container-image=registry.cn-hangzhou.aliyuncs.com/google_containers/pause-amd64:3.1"
EOF
systemctl daemon-reload 
systemctl enable --now kubelet
```



```
[root@iZ8vbi4t4rsi4kh859l777Z ~]# DOCKER_CGROUPS=$(docker info | grep 'Cgroup' | cut -d' ' -f3)
  WARNING: You're not using the default seccomp profile
WARNING: bridge-nf-call-ip6tables is disabled
[root@iZ8vbi4t4rsi4kh859l777Z ~]#
[root@iZ8vbi4t4rsi4kh859l777Z ~]# echo $DOCKER_CGROUPS
systemd
[root@iZ8vbi4t4rsi4kh859l777Z ~]# cat >/etc/sysconfig/kubelet<<EOF
> KUBELET_EXTRA_ARGS="--cgroup-driver=$DOCKER_CGROUPS 
> --pod-infra-container-image=registry.cn-hangzhou.aliyuncs.com/google_containers/pause-amd64:3.1"
> EOF
[root@iZ8vbi4t4rsi4kh859l777Z ~]# systemctl daemon-reload 
[root@iZ8vbi4t4rsi4kh859l777Z ~]# systemctl enable --now kubelet
Created symlink from /etc/systemd/system/multi-user.target.wants/kubelet.service to /usr/lib/systemd/system/kubelet.service.
[root@iZ8vbi4t4rsi4kh859l777Z ~]# 
[root@iZ8vbi4t4rsi4kh859l777Z ~]# systemctl status kubelet
● kubelet.service - kubelet: The Kubernetes Node Agent
   Loaded: loaded (/usr/lib/systemd/system/kubelet.service; enabled; vendor preset: disabled)
  Drop-In: /usr/lib/systemd/system/kubelet.service.d
           └─10-kubeadm.conf
   Active: activating (auto-restart) (Result: exit-code) since Fri 2020-03-27 23:53:06 CST; 5s ago
     Docs: https://kubernetes.io/docs/
  Process: 2147 ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS (code=exited, status=255)
 Main PID: 2147 (code=exited, status=255)

Mar 27 23:53:06 iZ8vbi4t4rsi4kh859l777Z systemd[1]: Unit kubelet.service entered failed state.
Mar 27 23:53:06 iZ8vbi4t4rsi4kh859l777Z systemd[1]: kubelet.service failed.
[root@iZ8vbi4t4rsi4kh859l777Z ~]#
```

```
[root@k8s-master01 ~]# systemctl status kubelet
● kubelet.service - kubelet: The Kubernetes Node Agent
   Loaded: loaded (/usr/lib/systemd/system/kubelet.service; enabled; vendor preset: disabled)
  Drop-In: /usr/lib/systemd/system/kubelet.service.d
           └─10-kubeadm.conf
   Active: active (running) since Tue 2020-03-31 12:13:46 CST; 17s ago
     Docs: https://kubernetes.io/docs/
 Main PID: 17110 (kubelet)
   CGroup: /system.slice/kubelet.service
           └─17110 /usr/bin/kubelet --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf --co...

Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.099927   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.200121   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.300314   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.400489   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.500614   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.600777   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.700912   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.801088   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:03 k8s-master01 kubelet[17110]: E0331 12:14:03.901259   17110 kubelet.go:2246] node "k8s-master01" not found
Mar 31 12:14:04 k8s-master01 kubelet[17110]: E0331 12:14:04.001430   17110 kubelet.go:2246] node "k8s-master01" not found
[root@k8s-master01 ~]#
```





报错：

failed to run Kubelet: failed to create kubelet: misconfiguration: kubelet cgroup driver: "$DOCKER_CGROUPS" is different from docker cgroup driver: "systemd"



```
[root@K8S-master0003 ~]# cat /etc/sysconfig/kubelet
KUBELET_EXTRA_ARGS="--cgroup-driver=systemd --pod-infra-container-image=registry.cn-hangzhou.aliyuncs.com/google_containers/pause-amd64:3.1"
```



重启

systemctl daemon-reload

systemctl restart kubelet



到这一步搞好的主机镜像就可以克隆为各个master和node节点使用



## 集群初始化



**HAProxy+Keepalived**



所有Master节点配置HAProxy

```
yum install -y keepalived haproxy
```



安装好时的原配置

```
global
    log         127.0.0.1 local2

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    stats socket /var/lib/haproxy/stats

defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 3
    timeout http-request    10s
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout http-keep-alive 10s
    timeout check           10s
    maxconn                 3000

frontend  main *:5000
    acl url_static       path_beg       -i /static /images /javascript /stylesheets
    acl url_static       path_end       -i .jpg .gif .png .css .js

    use_backend static          if url_static
    default_backend             app

backend static
    balance     roundrobin
    server      static 127.0.0.1:4331 check

backend app
    balance     roundrobin
    server  app1 127.0.0.1:5001 check
    server  app2 127.0.0.1:5002 check
    server  app3 127.0.0.1:5003 check
    server  app4 127.0.0.1:5004 check
```



修改配置为

```
global
  maxconn  2000
  ulimit-n  16384
  log 127.0.0.1 local0 err
  stats timeout 30s

defaults
  log global
  mode http
  option httplog
  timeout connect 5000
  timeout client 50000
  timeout server 50000
  timeout http-request 155
  timeout http-keep-alive 15s

frontend monitor-in
  bind *:33305
  mode http
  option httplog
  monitor-uri /monitor

listen stats
  bind *:8006
  mode http
  stats enable
  stats hide-version
  stats uri /stats
  stats refresh 30s
  stats realm Haproxy\ Statistics
  stats auth admin:admin
 
frontend K8S-master
  bind 0.0.0.0:16443
  bind 127.0.0.1:16443
  mode tcp
  option tcplog
  tcp-request inspect-delay 5s
  default_backend K8S-master

backend K8S-master
  mode tcp
  option tcplog
  option tcp-check
  balance roundrobin
  default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 250 maxqueue 256 weight 100
  server K8S-master01 172.26.130.52:6443 check
  server K8S-master02 172.26.130.53:6443 check
  server K8S-master03 172.26.130.54:6443 check
```



Keepalived的配置

```
[root@K8S-master0001 ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:00:94:00 brd ff:ff:ff:ff:ff:ff
    inet 172.26.130.52/20 brd 172.26.143.255 scope global dynamic eth0
       valid_lft 315347637sec preferred_lft 315347637sec
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:5a:be:0a:70 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 scope global docker0
       valid_lft forever preferred_lft forever
[root@K8S-master0001 ~]# 
```



```
cat /etc/keepalived/keepalived.conf
! Configuration File for keepalived

global_defs {
   router_id LVS_DEVEL
}

vrrp_script chk_apiserver {
    script "/etc/keepalived/check_apiserver.sh"
    interval 2
    weight -5
    fall 3
    rise 2
}

vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    mcast_src_ip 172.26.130.52
    virtual_router_id 51
    priority 100
    advert_int 2
    authentication {
        auth_type PASS
        auth_pass K8SHA_KA_AUTH
    }
    virtual_ipaddress {
        172.26.130.49
    }
#    track_script {
#        chk_apiserver
#    }
}
```



```
cat /etc/keepalived/check_apiserver.sh
#!/bin/bash

function check_apiserver() {
  for ((i=0;i<5;i++));do
    apiserver_job_id=$(pgrep kube-apiserver)
    if [[ ! -z $apiserver_job_id ]];then
      return
    else
      sleep 2
    fi
    apiserver_job_id=0
  done
}

# 1: running 0: stopped
check_apiserver
if [[ $apiserver_job_id -eq 0 ]]; then
  /usr/bin/systemctl stop keepalived
  exit 1
else
  exit 0
fi
```



```
systemctl enable --now haproxy
systemctl enable --now keepalived
```

全部master节点启动后看看虚拟IP在那，是否可访问

```
[root@K8S-master02 ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:0d:ce:70 brd ff:ff:ff:ff:ff:ff
    inet 172.26.130.53/20 brd 172.26.143.255 scope global dynamic eth0
       valid_lft 315335075sec preferred_lft 315335075sec
    inet 172.26.130.51/32 scope global eth0
       valid_lft forever preferred_lft forever
```



```
[root@k8s-master01 ~]# ping 172.26.130.51
PING 172.26.130.51 (172.26.130.51) 56(84) bytes of data.
64 bytes from 172.26.130.51: icmp_seq=1 ttl=64 time=0.052 ms
64 bytes from 172.26.130.51: icmp_seq=2 ttl=64 time=0.048 ms
64 bytes from 172.26.130.51: icmp_seq=3 ttl=64 time=0.049 ms
^C
--- 172.26.130.51 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2049ms
rtt min/avg/max/mdev = 0.048/0.049/0.052/0.008 ms
[root@k8s-master01 ~]# 
```

如果访问不到，下一步会报错

systemctl status kubelet

node "k8s-master02" not found



kubeadm-config 配置

```
apiVersion: kubeadm.k8s.io/v1beta1
kind: ClusterConfiguration 
kubernetesVersion: v1.14.1 
imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers 
apiServer:
  certSANs:
  - 172.26.130.51
controlPlaneEndpoint: "172.26.130.51:16443" 
networking:
   This CIDR is a Calico default. Substitute or remove for your CNI provider. 
  podSubnet: "172.168.0.0/16"
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration 
ipvs:
  minSyncPeriod: 1s
  scheduler: rr
  syncPeriod: 10s
mode: ipvs
```



```
[root@k8s-master01 ~]# kubeadm config images pull --config /root/kubeadm-config.yaml
[config/images] Pulled registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.14.1
[config/images] Pulled registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.14.1
[config/images] Pulled registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.14.1
[config/images] Pulled registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.14.1
[config/images] Pulled registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.1
[config/images] Pulled registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.3.10
[config/images] Pulled registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.3.1
[root@k8s-master01 ~]# 
```



Master 节点初始化:

```
kubeadm init --config=/root/kubeadm-config.yaml --experimental-upload-certs
```

kubernetes 1.14.x版，在初始化时加入--experimental-upload-certs参数，使集群初始化更加简单，无须再复制证书至其他节点，之后join时添加--certificate-key参数即可自动加入集群



```
[root@k8s-master01 ~]# kubeadm init --config=/root/kubeadm-config.yaml --experimental-upload-certs
[init] Using Kubernetes version: v1.14.1
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master01 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.26.130.52 172.26.130.49 172.26.130.49]
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master01 localhost] and IPs [172.26.130.52 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master01 localhost] and IPs [172.26.130.52 127.0.0.1 ::1]
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "admin.conf" kubeconfig file
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[kubelet-check] Initial timeout of 40s passed.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.

Unfortunately, an error has occurred:
        timed out waiting for the condition

This error is likely caused by:
        - The kubelet is not running
        - The kubelet is unhealthy due to a misconfiguration of the node in some way (required cgroups disabled)

If you are on a systemd-powered system, you can try to troubleshoot the error with the following commands:
        - 'systemctl status kubelet'
        - 'journalctl -xeu kubelet'

Additionally, a control plane component may have crashed or exited when started by the container runtime.
To troubleshoot, list all containers using your preferred container runtimes CLI, e.g. docker.
Here is one example how you may list all Kubernetes containers running in docker:
        - 'docker ps -a | grep kube | grep -v pause'
        Once you have found the failing container, you can inspect its logs with:
        - 'docker logs CONTAINERID'
error execution phase wait-control-plane: couldn't initialize a Kubernetes cluster
[root@k8s-master01 ~]# 
```





如果初始化失败，重置后再次初始化：

```
kubeadm reset

[root@k8s-master01 ~]# kubeadm reset
[reset] Reading configuration from the cluster...
[reset] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
W0331 11:41:48.436202    9921 reset.go:73] [reset] Unable to fetch the kubeadm-config ConfigMap from cluster: failed to get config map: Get https://172.26.130.49:16443/api/v1/namespaces/kube-system/configmaps/kubeadm-config: dial tcp 172.26.130.49:16443: i/o timeout
[reset] WARNING: Changes made to this host by 'kubeadm init' or 'kubeadm join' will be reverted.
[reset] Are you sure you want to proceed? [y/N]: y
[preflight] Running pre-flight checks
W0331 11:42:28.741079    9921 reset.go:234] [reset] No kubeadm config, using etcd pod spec to get data directory
[reset] Stopping the kubelet service
[reset] unmounting mounted directories in "/var/lib/kubelet"
[reset] Deleting contents of stateful directories: [/var/lib/etcd /var/lib/kubelet /etc/cni/net.d /var/lib/dockershim /var/run/kubernetes]
[reset] Deleting contents of config directories: [/etc/kubernetes/manifests /etc/kubernetes/pki]
[reset] Deleting files: [/etc/kubernetes/admin.conf /etc/kubernetes/kubelet.conf /etc/kubernetes/bootstrap-kubelet.conf /etc/kubernetes/controller-manager.conf /etc/kubernetes/scheduler.conf]

The reset process does not reset or clean up iptables rules or IPVS tables.
If you wish to reset iptables, you must do so manually.
For example:
iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X

If your cluster was setup to utilize IPVS, run ipvsadm --clear (or similar)
to reset your system's IPVS tables.

[root@k8s-master01 ~]# 
[root@k8s-master01 ~]# tail /var/log/messages 
Mar 31 11:52:05 K8S-master01 kubelet: I0331 11:52:05.797821   12127 docker_service.go:238] Hairpin mode set to "hairpin-veth"
Mar 31 11:52:05 K8S-master01 kubelet: W0331 11:52:05.797940   12127 cni.go:213] Unable to update cni config: No networks found in /etc/cni/net.d
Mar 31 11:52:05 K8S-master01 kubelet: W0331 11:52:05.800325   12127 cni.go:213] Unable to update cni config: No networks found in /etc/cni/net.d
Mar 31 11:52:05 K8S-master01 kubelet: I0331 11:52:05.800366   12127 docker_service.go:253] Docker cri networking managed by cni
Mar 31 11:52:05 K8S-master01 kubelet: W0331 11:52:05.800439   12127 cni.go:213] Unable to update cni config: No networks found in /etc/cni/net.d
Mar 31 11:52:05 K8S-master01 kubelet: I0331 11:52:05.809070   12127 docker_service.go:258] Docker Info: &{ID:E3HC:U7XG:UDZF:BGZL:MAC2:ZXID:JGYL:WA3R:XZDO:WDAT:6R5M:WFKI Containers:0 ContainersRunning:0 ContainersPaused:0 ContainersStopped:0 Images:7 Driver:overlay2 DriverStatus:[[Backing Filesystem extfs] [Supports d_type true] [Native Overlay Diff true]] SystemStatus:[] Plugins:{Volume:[local] Network:[bridge host macvlan null overlay] Authorization:[] Log:[]} MemoryLimit:true SwapLimit:true KernelMemory:true CPUCfsPeriod:true CPUCfsQuota:true CPUShares:true CPUSet:true IPv4Forwarding:true BridgeNfIptables:true BridgeNfIP6tables:true Debug:false NFd:15 OomKillDisable:true NGoroutines:22 SystemTime:2020-03-31T11:52:05.802379237+08:00 LoggingDriver:journald CgroupDriver:systemd NEventsListener:0 KernelVersion:4.19.0-1.el7.elrepo.x86_64 OperatingSystem:CentOS Linux 7 (Core) OSType:linux Architecture:x86_64 IndexServerAddress:https://index.docker.io/v1/ RegistryConfig:0xc000676150 NCPU:2 MemTotal:3940204544 GenericResources:[] DockerRootDir:/var/lib/docker HTTPProxy: HTTPSProxy: NoProxy: Name:k8s-master01 Labels:[] ExperimentalBuild:false ServerVersion:1.13.1 ClusterStore: ClusterAdvertise: Runtimes:map[docker-runc:{Path:/usr/libexec/docker/docker-runc-current Args:[]} runc:{Path:docker-runc Args:[]}] DefaultRuntime:docker-runc Swarm:{NodeID: NodeAddr: LocalNodeState:inactive ControlAvailable:false Error: RemoteManagers:[] Nodes:0 Managers:0 Cluster:0xc0004988c0} LiveRestoreEnabled:false Isolation: InitBinary:/usr/libexec/docker/docker-init-current ContainerdCommit:{ID: Expected:aa8187dbd3b7ad67d8e5e3a15115d3eef43a7ed1} RuncCommit:{ID:66aedde759f33c190954815fb765eedc1d782dd9 Expected:9df8b306d01f59d3a8029be411de015b7304dd8f} InitCommit:{ID:fec3683b971d9c3ef73f284f176672c44b448662 Expected:949e6facb77383876aeff8a6944dde66b3089574} SecurityOptions:[name=seccomp,profile=/etc/docker/seccomp.json]}
Mar 31 11:52:05 K8S-master01 kubelet: F0331 11:52:05.809163   12127 server.go:266] failed to run Kubelet: failed to create kubelet: misconfiguration: kubelet cgroup driver: "$DOCKER_CGROUPS" is different from docker cgroup driver: "systemd"
Mar 31 11:52:05 K8S-master01 systemd: kubelet.service: main process exited, code=exited, status=255/n/a
Mar 31 11:52:05 K8S-master01 systemd: Unit kubelet.service entered failed state.
Mar 31 11:52:05 K8S-master01 systemd: kubelet.service failed.
[root@k8s-master01 ~]# 
```



```
[root@k8s-master01 ~]# kubeadm reset
[reset] Reading configuration from the cluster...
[reset] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
W0331 12:17:35.018038   17809 reset.go:73] [reset] Unable to fetch the kubeadm-config ConfigMap from cluster: failed to get config map: Get https://172.26.130.49:16443/api/v1/namespaces/kube-system/configmaps/kubeadm-config: dial tcp 172.26.130.49:16443: i/o timeout
[reset] WARNING: Changes made to this host by 'kubeadm init' or 'kubeadm join' will be reverted.
[reset] Are you sure you want to proceed? [y/N]: y
[preflight] Running pre-flight checks
W0331 12:17:49.784704   17809 reset.go:234] [reset] No kubeadm config, using etcd pod spec to get data directory
[reset] Stopping the kubelet service
[reset] unmounting mounted directories in "/var/lib/kubelet"
[reset] Deleting contents of stateful directories: [/var/lib/etcd /var/lib/kubelet /etc/cni/net.d /var/lib/dockershim /var/run/kubernetes]
[reset] Deleting contents of config directories: [/etc/kubernetes/manifests /etc/kubernetes/pki]
[reset] Deleting files: [/etc/kubernetes/admin.conf /etc/kubernetes/kubelet.conf /etc/kubernetes/bootstrap-kubelet.conf /etc/kubernetes/controller-manager.conf /etc/kubernetes/scheduler.conf]

The reset process does not reset or clean up iptables rules or IPVS tables.
If you wish to reset iptables, you must do so manually.
For example:
iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X

If your cluster was setup to utilize IPVS, run ipvsadm --clear (or similar)
to reset your system's IPVS tables.

[root@k8s-master01 ~]# iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X
[root@k8s-master01 ~]# 
```

只在 k8s-master01 节点上初始化

```
[root@k8s-master01 ~]# kubeadm init --config=/root/kubeadm-config.yaml --experimental-upload-certs
[init] Using Kubernetes version: v1.14.1
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master01 localhost] and IPs [172.26.130.52 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master01 localhost] and IPs [172.26.130.52 127.0.0.1 ::1]
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master01 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.26.130.52 172.26.130.51 172.26.130.51]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "admin.conf" kubeconfig file
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 24.508440 seconds
[upload-config] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.14" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Storing the certificates in ConfigMap "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
61a6a8a85fb121ab54b9cccb64bdfd451eb5a4ff3c134b1e46b3a9115afce266
[mark-control-plane] Marking the node k8s-master01 as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node k8s-master01 as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: 4nw01b.2r25swzzjugb4gjl
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 172.26.130.51:16443 --token 4nw01b.2r25swzzjugb4gjl \
    --discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2 \
    --experimental-control-plane --certificate-key 61a6a8a85fb121ab54b9cccb64bdfd451eb5a4ff3c134b1e46b3a9115afce266

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use 
"kubeadm init phase upload-certs --experimental-upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.26.130.51:16443 --token 4nw01b.2r25swzzjugb4gjl \
    --discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2 
[root@k8s-master01 ~]# 
```

成功！



然后在其他节点执行初始化后的语句加入集群

看pod状态：

kubectl get pods -n kube-system -o wide

```

[root@k8s-master01 ~]# kubectl get pods -n kube-system -o wide
NAME                                   READY   STATUS    RESTARTS   AGE    IP              NODE           NOMINATED NODE   READINESS GATES
coredns-6547f8b698-p66kr               0/1     Pending   0          72m    <none>          <none>         <none>           <none>
coredns-6547f8b698-qbs6g               0/1     Pending   0          72m    <none>          <none>         <none>           <none>
etcd-k8s-master01                      1/1     Running   0          71m    172.26.130.52   k8s-master01   <none>           <none>
etcd-k8s-master02                      1/1     Running   0          10m    172.26.130.53   k8s-master02   <none>           <none>
etcd-k8s-master03                      1/1     Running   0          6m2s   172.26.130.54   k8s-master03   <none>           <none>
kube-apiserver-k8s-master01            1/1     Running   0          71m    172.26.130.52   k8s-master01   <none>           <none>
kube-apiserver-k8s-master02            1/1     Running   0          10m    172.26.130.53   k8s-master02   <none>           <none>
kube-apiserver-k8s-master03            1/1     Running   0          26m    172.26.130.54   k8s-master03   <none>           <none>
kube-controller-manager-k8s-master01   1/1     Running   1          71m    172.26.130.52   k8s-master01   <none>           <none>
kube-controller-manager-k8s-master02   1/1     Running   0          10m    172.26.130.53   k8s-master02   <none>           <none>
kube-controller-manager-k8s-master03   1/1     Running   0          6m8s   172.26.130.54   k8s-master03   <none>           <none>
kube-proxy-g6wd2                       1/1     Running   0          66m    172.26.130.53   k8s-master02   <none>           <none>
kube-proxy-gzvln                       1/1     Running   0          72m    172.26.130.52   k8s-master01   <none>           <none>
kube-proxy-m5nkt                       1/1     Running   0          26m    172.26.130.54   k8s-master03   <none>           <none>
kube-scheduler-k8s-master01            1/1     Running   1          71m    172.26.130.52   k8s-master01   <none>           <none>
kube-scheduler-k8s-master02            1/1     Running   0          10m    172.26.130.53   k8s-master02   <none>           <none>
kube-scheduler-k8s-master03            1/1     Running   0          26m    172.26.130.54   k8s-master03   <none>           <none>
[root@k8s-master01 ~]# 
```





## Calico组件的安装

参考

https://www.cnblogs.com/ding2016/p/10784620.html



POD_CIDR为kubeadm-config 配置中的podSubnet:

```
POD_CIDR="<your-pod-cidr>" \
sed -i -e "s?172.26.130.0/24?$POD_CIDR?g" calico/v3.6.1/calico.yaml
kubectl apply -f calico/v3.6.1/calico.yaml
```



只需要在master01节点上部署了，就会同步到其他master节点

```
[root@k8s-master01 ~]# POD_CIDR="172.168.0.0/16"
[root@k8s-master01 ~]# wget https://docs.projectcalico.org/v3.6/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml
...
[root@k8s-master01 ~]# sed -i -e "s?192.168.0.0/16?$POD_CIDR?g" calico.yaml
...
[root@k8s-master01 ~]# kubectl apply -f calico.yaml 
configmap/calico-config created
customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamblocks.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/blockaffinities.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamhandles.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamconfigs.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
clusterrole.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrolebinding.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrole.rbac.authorization.k8s.io/calico-node created
clusterrolebinding.rbac.authorization.k8s.io/calico-node created
daemonset.extensions/calico-node created
serviceaccount/calico-node created
deployment.extensions/calico-kube-controllers created
serviceaccount/calico-kube-controllers created
[root@k8s-master01 ~]# 
```



再次查看Pod状态

```
[root@k8s-master01 ~]# kubectl get pods -n kube-system -o wide
NAME                                       READY   STATUS    RESTARTS   AGE    IP                NODE           NOMINATED NODE   READINESS GATES
calico-kube-controllers-7567777df7-tfchf   1/1     Running   0          15m    172.168.195.2     k8s-master03   <none>           <none>
calico-node-7hkrb                          1/1     Running   0          2m8s   172.26.130.52     k8s-master01   <none>           <none>
calico-node-h5fsw                          1/1     Running   0          2m8s   172.26.130.54     k8s-master03   <none>           <none>
calico-node-rw5v2                          1/1     Running   0          2m8s   172.26.130.53     k8s-master02   <none>           <none>
coredns-6547f8b698-p66kr                   1/1     Running   0          165m   172.168.195.1     k8s-master03   <none>           <none>
coredns-6547f8b698-qbs6g                   1/1     Running   0          165m   172.168.122.129   k8s-master02   <none>           <none>
etcd-k8s-master01                          1/1     Running   0          164m   172.26.130.52     k8s-master01   <none>           <none>
etcd-k8s-master02                          1/1     Running   0          103m   172.26.130.53     k8s-master02   <none>           <none>
etcd-k8s-master03                          1/1     Running   0          99m    172.26.130.54     k8s-master03   <none>           <none>
kube-apiserver-k8s-master01                1/1     Running   0          164m   172.26.130.52     k8s-master01   <none>           <none>
kube-apiserver-k8s-master02                1/1     Running   0          103m   172.26.130.53     k8s-master02   <none>           <none>
kube-apiserver-k8s-master03                1/1     Running   0          119m   172.26.130.54     k8s-master03   <none>           <none>
kube-controller-manager-k8s-master01       1/1     Running   1          164m   172.26.130.52     k8s-master01   <none>           <none>
kube-controller-manager-k8s-master02       1/1     Running   0          103m   172.26.130.53     k8s-master02   <none>           <none>
kube-controller-manager-k8s-master03       1/1     Running   0          99m    172.26.130.54     k8s-master03   <none>           <none>
kube-proxy-g6wd2                           1/1     Running   0          159m   172.26.130.53     k8s-master02   <none>           <none>
kube-proxy-gzvln                           1/1     Running   0          165m   172.26.130.52     k8s-master01   <none>           <none>
kube-proxy-m5nkt                           1/1     Running   0          119m   172.26.130.54     k8s-master03   <none>           <none>
kube-scheduler-k8s-master01                1/1     Running   1          164m   172.26.130.52     k8s-master01   <none>           <none>
kube-scheduler-k8s-master02                1/1     Running   0          103m   172.26.130.53     k8s-master02   <none>           <none>
kube-scheduler-k8s-master03                1/1     Running   0          119m   172.26.130.54     k8s-master03   <none>           <none>
[root@k8s-master01 ~]# 
```

会看到 coredns-6547f8b698-qbs6g  和 coredns-6547f8b698-qbs6g 都是有分配IP了



增加了几个172.168.0.0 网段的pod

```
calico-kube-controllers-7567777df7-tfchf   1/1     Running   0          15m    172.168.195.2     k8s-master03   <none>           <none>
calico-node-7hkrb                          1/1     Running   0          2m8s   172.26.130.52     k8s-master01   <none>           <none>
calico-node-h5fsw                          1/1     Running   0          2m8s   172.26.130.54     k8s-master03   <none>           <none>
calico-node-rw5v2
```







## 高可用Master

master 节点是

```
kubeadm join 172.26.130.51:16443 --token 4nw01b.2r25swzzjugb4gjl \
--discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2 \
--experimental-control-plane --certificate-key 61a6a8a85fb121ab54b9cccb64bdfd451eb5a4ff3c134b1e46b3a9115afce266

```



node节点是

```
kubeadm join 172.26.130.51:16443 --token 4nw01b.2r25swzzjugb4gjl \
    --discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2
```



```
[root@K8S-master0003 ~]# kubeadm join 172.26.130.51:16443 --token 4nw01b.2r25swzzjugb4gjl --discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2 --experimental-control-plane --certificate-key 61a6a8a85fb121ab54b9cccb64bdfd451eb5a4ff3c134b1e46b3a9115afce266
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
        [WARNING RequiredIPVSKernelModulesAvailable]: 

The IPVS proxier may not be used because the following required kernel modules are not loaded: [ip_vs_wrr ip_vs_sh ip_vs_rr]
or no builtin kernel IPVS support was found: map[ip_vs:{} ip_vs_rr:{} ip_vs_sh:{} ip_vs_wrr:{} nf_conntrack:{}].
However, these modules may be loaded automatically by kube-proxy if they are available on your system.
To verify IPVS support:

   Run "lsmod | grep 'ip_vs|nf_conntrack'" and verify each of the above modules are listed.

If they are not listed, you can use the following methods to load them:

1. For each missing module run 'modprobe $modulename' (e.g., 'modprobe ip_vs', 'modprobe ip_vs_rr', ...)
2. If 'modprobe $modulename' returns an error, you will need to install the missing module support for your kernel.

[preflight] Running pre-flight checks before initializing the new control plane instance
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[download-certs] Downloading the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master03 localhost] and IPs [172.26.130.54 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master03 localhost] and IPs [172.26.130.54 127.0.0.1 ::1]
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master03 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.26.130.54 172.26.130.51 172.26.130.51]
[certs] Valid certificates and keys now exist in "/etc/kubernetes/pki"
[certs] Using the existing "sa" key
[kubeconfig] Generating kubeconfig files
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[check-etcd] Checking that the etcd cluster is healthy
[kubelet-start] Downloading configuration for the kubelet from the "kubelet-config-1.14" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Activating the kubelet service
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...
[kubelet-check] Initial timeout of 40s passed.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
[kubelet-check] It seems like the kubelet isn't running or healthy.
[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get http://localhost:10248/healthz: dial tcp 127.0.0.1:10248: connect: connection refused.
error execution phase kubelet-start: error uploading crisocket: timed out waiting for the condition
```

kubeadm reset 

再一次 kubeadm join



这种才是成功加入集群的：

```
[root@K8S-master0002 ~]# kubeadm join 172.26.130.51:16443 --token 4nw01b.2r25swzzjugb4gjl \
> --discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2 \
> --experimental-control-plane --certificate-key 61a6a8a85fb121ab54b9cccb64bdfd451eb5a4ff3c134b1e46b3a9115afce266
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[preflight] Running pre-flight checks before initializing the new control plane instance
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[download-certs] Downloading the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master02 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.26.130.53 172.26.130.51 172.26.130.51]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master02 localhost] and IPs [172.26.130.53 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master02 localhost] and IPs [172.26.130.53 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Valid certificates and keys now exist in "/etc/kubernetes/pki"
[certs] Using the existing "sa" key
[kubeconfig] Generating kubeconfig files
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[endpoint] WARNING: port specified in controlPlaneEndpoint overrides bindPort in the controlplane address
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[check-etcd] Checking that the etcd cluster is healthy
[kubelet-start] Downloading configuration for the kubelet from the "kubelet-config-1.14" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Activating the kubelet service
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...
[etcd] Announced new etcd member joining to the existing etcd cluster
[etcd] Wrote Static Pod manifest for a local etcd member to "/etc/kubernetes/manifests/etcd.yaml"
[etcd] Waiting for the new etcd member to join the cluster. This can take up to 40s
[upload-config] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[mark-control-plane] Marking the node k8s-master02 as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node k8s-master02 as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]

This node has joined the cluster and a new control plane instance was created:

* Certificate signing request was sent to apiserver and approval was received.
* The Kubelet was informed of the new secure connection details.
* Control plane (master) label and taint were applied to the new node.
* The Kubernetes control plane instances scaled up.
* A new etcd member was added to the local/stacked etcd cluster.

To start administering your cluster from this node, you need to run the following as a regular user:

        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config

Run 'kubectl get nodes' to see this node join the cluster.

[root@K8S-master0002 ~]# 
```



解决方案

https://blog.csdn.net/peishucai/article/details/101172708?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task



配置环境变量查看节点状态：

```
[root@K8S-master03 ~]# cat <<EOF >> ~/.bashrc
> export KUBECONFIG=/etc/kubernetes/admin.conf
> EOF
[root@K8S-master03 ~]# source .bashrc 
[root@K8S-master03 ~]# kubectl get nodes
NAME           STATUS     ROLES    AGE   VERSION
k8s-master01   NotReady   master   69m   v1.14.10
k8s-master02   NotReady   master   63m   v1.14.10
k8s-master03   NotReady   master   23m   v1.14.10
[root@K8S-master03 ~]# 
```



资源使用情况查看

```
[root@k8s-master01 ~]#
top - 19:42:53 up  9:44,  1 user,  load average: 0.14, 0.23, 0.20
Tasks: 123 total,   1 running,  82 sleeping,   0 stopped,   0 zombie
%Cpu(s):  3.1 us,  6.2 sy,  0.0 ni, 90.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  3847856 total,  1286724 free,   700980 used,  1860152 buff/cache
KiB Swap:        0 total,        0 free,        0 used.  2987328 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                        
31373 root      20   0  475048 310808  69628 S   6.7  8.1   3:43.45 kube-apiserver                                                                 
31385 root      20   0   10.1g  90912  18260 S   6.7  2.4   4:00.22 etcd                                                                           
    1 root      20   0   43668   5324   3876 S   0.0  0.1   0:09.65 systemd                                                                        
    2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd                                                                       
    3 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 rcu_gp                                                                         
    4 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 rcu_par_gp                                                                     
    6 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 kworker/0:0H-kb                                                                
    8 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 mm_percpu_wq                                                                   
    9 root      20   0       0      0      0 S   0.0  0.0   0:00.73 ksoftirqd/0                                                                    
   10 root      20   0       0      0      0 I   0.0  0.0   0:10.08 rcu_sched                                                                      
   11 root      20   0       0      0      0 I   0.0  0.0   0:00.00 rcu_bh                                                                         
   12 root      rt   0       0      0      0 S   0.0  0.0   0:00.18 migration/0                                                                    
   14 root      20   0       0      0      0 S   0.0  0.0   0:00.00 cpuhp/0                                                                        
   15 root      20   0       0      0      0 S   0.0  0.0   0:00.00 cpuhp/1                                                                        
   16 root      rt   0       0      0      0 S   0.0  0.0   0:00.17 migration/1                                                                    
   17 root      20   0       0      0      0 S   0.0  0.0   0:00.44 ksoftirqd/1                                                                    
   19 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 kworker/1:0H-kb                                                                
   20 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kdevtmpfs                                                                      
   21 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 netns                                                                          
   22 root      20   0       0      0      0 S   0.0  0.0   0:00.18 kauditd                                                                        
   23 root      20   0       0      0      0 S   0.0  0.0   0:00.01 khungtaskd                                                                     
   24 root      20   0       0      0      0 S   0.0  0.0   0:00.00 oom_reaper                                                                     
   25 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 writeback                                                                      
   26 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kcompactd0                                                                     
   27 root      25   5       0      0      0 S   0.0  0.0   0:00.00 ksmd                                                                           
   28 root      39  19       0      0      0 S   0.0  0.0   0:00.11 khugepaged                                                                     
   29 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 crypto                                                                         
   30 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 kintegrityd                                                                    
   31 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 kblockd                                                                        
   32 root       0 -20       0      0      0 I   0.0  0.0   0:00.00 md      
   
[root@k8s-master01 ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        683M        1.2G        2.0M        1.8G        2.8G
Swap:            0B          0B          0B
[root@k8s-master01 ~]# 
```



## Node节点的配置



```
systemctl status docker

systemctl status kubelet
```



这里的master节点IP要换成原来自己的，不能是HA的虚拟IP

```
kubeadm join 172.26.130.52:16443 --token 4nw01b.2r25swzzjugb4gjl \
    --discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2
```

```
[root@k8s-node01 ~]# kubeadm join 172.26.130.52:16443 --token 4nw01b.2r25swzzjugb4gjl     --discovery-token-ca-cert-hash sha256:03394db9f98fd926f9ee0d40ef7bd6cf51331989432c7bf9e4b2c79557cf24c2
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
error execution phase preflight: unable to fetch the kubeadm-config ConfigMap: failed to get config map: Get https://172.26.130.51:16443/api/v1/namespaces/kube-system/configmaps/kubeadm-config: dial tcp 172.26.130.51:16443: i/o timeout
[root@k8s-node01 ~]# 
```

