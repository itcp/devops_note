

[TOC]



## 官网下载安装包



官网地址：[https://prometheus.io](https://prometheus.io)



`wget https://github.com/prometheus/prometheus/releases/download/v2.17.1/prometheus-2.17.1.linux-amd64.tar.gz`



## 安装



```
tar -C /usr/local/ -xf prometheus-2.17.1.linux-amd64.tar.gz 
ln -s /usr/local/prometheus-2.17.1.linux-amd64 /usr/local/prometheus

# 创建运行用户
useradd -s /sbin/nologin prometheus

mkdir /data/prometheus
chown prometheus:prometheus /data/prometheus
```



启动命令

```
/usr/local/prometheus/prometheus --config.file=/usr/local/prometheus/prometheus.yml
```



编写systemd服务脚本

```
vi /usr/lib/systemd/system/prometheus.service

[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus  # 必须用该用户和相应的执行权限否则不能启动
Group=prometheus
Type=simple
ExecStart=/usr/local/prometheus/prometheus --config.file=/usr/local/prometheus/prometheus.yml --storage.tsdb.path=/data/prometheus  # 启动脚本

[Install]
WantedBy=multi-user.target

```



```
在运行./prometheus脚本之前可以进行一次配置文件检查
protool check config prometheus.yml


prometheus --config.file="/usr/local/prometheus-2.16.0.linux-amd64/prometheus.yml" --web.listen-address="0.0.0.0:9090"
--config.file="/usr/local/prometheus/prometheus.yml"  #指定配置文件路径
--web.listen-address="0.0.0.0:9090"  #指定服务端口
--storage.tsdb.path="/data/prometheus"  #指定数据存储路径
--storage.tsdb.retention.time=15d  #数据保留时间
--collector.systemd #开启服务状态监控，开启后在WEB上可以看到多出相关监控项
--collector.systemd.unit-whitelist=(sshd|nginx).service  #具体要监控的服务名
--web.enable-lifecycle  #开启热加载配置
```



