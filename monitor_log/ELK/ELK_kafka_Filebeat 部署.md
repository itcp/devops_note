# 官网资料：

[https://www.elastic.co/cn/](https://www.elastic.co/cn/)

[安装logstash](https://www.elastic.co/guide/en/logstash/7.3/installing-logstash.html)

------

# 主机参数：

mnl-office-pub-ops-es    # 部署elasticsearch6.7.0和kibina6.7.0

```
[root@mnl-office-pub-ops-elasticsearch ~]# cat /etc/centos-release
CentOS Linux release 7.6.1810 (Core) 
[root@mnl-office-pub-ops-elasticsearch ~]# lscpu 
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                2
On-line CPU(s) list:   0,1
Thread(s) per core:    1
Core(s) per socket:    1
Socket(s):             2
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 45
Model name:            Intel(R) Xeon(R) CPU E5-2670 0 @ 2.60GHz
Stepping:              7
CPU MHz:               2599.999
BogoMIPS:              5199.99
Hypervisor vendor:     VMware
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              20480K
NUMA node0 CPU(s):     0,1
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx hypervisor lahf_lm ssbd ibrs ibpb stibp tsc_adjust arat spec_ctrl intel_stibp flush_l1d arch_capabilities
[root@mnl-office-pub-ops-elasticsearch ~]# 
[root@mnl-office-pub-ops-elasticsearch ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        2.2G        168M         24M        1.3G        1.1G
Swap:          2.0G          0B        2.0G
[root@mnl-office-pub-ops-elasticsearch ~]# 
```

mnl-office-pub-ops-logetl            部署 logstash6.7.0  及插件

```
[root@mnl-office-pub-ops-logetl ~]# cat /etc/centos-release
CentOS Linux release 7.6.1810 (Core) 
[root@mnl-office-pub-ops-logetl ~]# 
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                2
On-line CPU(s) list:   0,1
Thread(s) per core:    1
Core(s) per socket:    1
Socket(s):             2
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 45
Model name:            Intel(R) Xeon(R) CPU E5-2670 0 @ 2.60GHz
Stepping:              7
CPU MHz:               2600.000
BogoMIPS:              5200.00
Hypervisor vendor:     VMware
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              20480K
NUMA node0 CPU(s):     0,1
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx rdtscp lm constant_tsc arch_perfmon nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx hypervisor lahf_lm ssbd ibrs ibpb stibp tsc_adjust arat spec_ctrl intel_stibp flush_l1d arch_capabilities
[root@mnl-office-pub-ops-logetl ~]#
[root@mnl-office-pub-ops-logetl ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        2.2G        783M        9.1M        706M        1.2G
Swap:          2.0G          0B        2.0G
[root@mnl-office-pub-ops-logetl ~]# 
```

各个日志源主机部署 fileBeat 6.7.0

------

# 准备

### 安装jdk

在部署 elasticsearch和logstash的主机中要先安装好jdk

```
[root@mnl-office-pub-ops-logetl ~]# javac -version
javac 1.8.0_211
[root@mnl-office-pub-ops-logetl ~]#

ln -s /usr/local/jdk/bin/java /usr/local/sbin/java
```

\---

### 安装包下载

elasticsearch要下载运行包，源码包是不能安装使用的

wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.7.0.tar.gz

https://github.com/elastic/kibana/releases
curl -O https://codeload.github.com/elastic/kibana/tar.gz/v6.7.0

https://artifacts.elastic.co/downloads/logstash/logstash-6.7.0.tar.gz

https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.7.0-linux-x86_64.tar.gz

------

## 安装 logstash

```
tar -C /usr/local/ -xf /usr/local/src/logstash-6.7.0.tar.gz
ln -s /usr/local/logstash-6.7.0 /usr/local/logstash
```

### 安装插件

先查看已经有那些插件了

```
[root@mnl-office-pub-ops-logetl logstash]# ls -la vendor/bundle/jruby/2.5.0/gems/
total 16
drwxrwxr-x 208 root root 8192 Mar 22 00:26 .
drwxrwxr-x 9 root root 111 Mar 22 00:26 ..
drwxrwxr-x 7 root root 183 Mar 22 00:24 addressable-2.3.8
drwxrwxr-x 3 root root 117 Mar 22 00:24 arr-pm-0.0.10
drwxrwxr-x 6 root root 141 Mar 22 00:24 atomic-1.1.99-java
drwxrwxr-x 5 root root 59 Mar 22 00:24 avl_tree-1.2.1
....
.....
drwxrwxr-x 4 root root 168 Mar 22 00:25 logstash-codec-cef-5.0.6-java
drwxrwxr-x 5 root root 187 Mar 22 00:25 logstash-codec-collectd-3.0.8
drwxrwxr-x 4 root root 169 Mar 22 00:25 logstash-codec-dots-3.0.6
drwxrwxr-x 4 root root 168 Mar 22 00:25 logstash-codec-edn-3.0.6
....
...
drwxrwxr-x 4 root root 191 Mar 22 00:25 logstash-filter-aggregate-2.9.0
drwxrwxr-x 4 root root 175 Mar 22 00:25 logstash-filter-anonymize-3.0.6
drwxrwxr-x 4 root root 170 Mar 22 00:25 logstash-filter-cidr-3.1.2-java
drwxrwxr-x 4 root root 171 Mar 22 00:25 logstash-filter-clone-3.0.6
drwxrwxr-x 4 root root 169 Mar 22 00:25 logstash-filter-csv-3.0.8
drwxrwxr-x 5 root root 184 Mar 22 00:25 logstash-filter-date-3.1.9
...
...
drwxrwxr-x 4 root root 180 Mar 22 00:25 logstash-input-azure_event_hubs-1.1.0
drwxrwxr-x 5 root root 218 Mar 22 00:25 logstash-input-beats-5.1.8-java
drwxrwxr-x 5 root root 231 Mar 22 00:25 logstash-input-dead_letter_queue-1.1.4
drwxrwxr-x 4 root root 178 Mar 22 00:25 logstash-input-elasticsearch-4.3.0
drwxrwxr-x 4 root root 169 Mar 22 00:25 logstash-input-exec-3.3.2
...
...
drwxrwxr-x 3 root root 156 Mar 22 00:25 logstash-mixin-aws-4.3.0
drwxrwxr-x 3 root root 146 Mar 22 00:25 logstash-mixin-http_client-6.0.1
drwxrwxr-x 4 root root 145 Mar 22 00:25 logstash-mixin-rabbitmq_connection-5.0.2-java
drwxrwxr-x 4 root root 176 Mar 22 00:25 logstash-output-cloudwatch-3.0.8
drwxrwxr-x 4 root root 169 Mar 22 00:25 logstash-output-csv-3.0.7
drwxrwxr-x 4 root root 188 Mar 22 00:25 logstash-output-elastic_app_search-1.0.0.beta1
drwxrwxr-x 4 root root 179 Mar 22 00:25 logstash-output-elasticsearch-9.4.0-java
drwxrwxr-x 4 root root 171 Mar 22 00:25 logstash-output-email-4.1.1
...
....
[root@mnl-office-pub-ops-logetl logstash]# 
```

过滤查看我们平时常用的插件是否已经有

```
[root@mnl-office-pub-ops-logetl logstash]# ls -la vendor/bundle/jruby/2.5.0/gems/ | grep -E 'elasticsearch|grok|redis|rabbitmq|beat'
drwxrwxr-x 4 root root 125 Mar 22 00:25 elasticsearch-5.0.5
drwxrwxr-x 5 root root 142 Mar 22 00:25 elasticsearch-api-5.0.5
drwxrwxr-x 4 root root 135 Mar 22 00:25 elasticsearch-transport-5.0.5
drwxrwxr-x 3 root root 32 Mar 22 00:25 jls-grok-0.11.5
drwxrwxr-x 4 root root 179 Mar 22 00:25 logstash-filter-elasticsearch-3.6.0
drwxrwxr-x 4 root root 170 Mar 22 00:25 logstash-filter-grok-4.0.4
drwxrwxr-x 5 root root 218 Mar 22 00:25 logstash-input-beats-5.1.8-java
drwxrwxr-x 4 root root 178 Mar 22 00:25 logstash-input-elasticsearch-4.3.0
drwxrwxr-x 4 root root 194 Mar 22 00:25 logstash-input-heartbeat-3.0.7
drwxrwxr-x 4 root root 173 Mar 22 00:25 logstash-input-rabbitmq-6.0.3
drwxrwxr-x 4 root root 170 Mar 22 00:25 logstash-input-redis-3.4.1
drwxrwxr-x 4 root root 145 Mar 22 00:25 logstash-mixin-rabbitmq_connection-5.0.2-java
drwxrwxr-x 4 root root 179 Mar 22 00:25 logstash-output-elasticsearch-9.4.0-java
drwxrwxr-x 4 root root 174 Mar 22 00:25 logstash-output-rabbitmq-5.1.1-java
drwxrwxr-x 4 root root 171 Mar 22 00:25 logstash-output-redis-4.0.4
drwxrwxr-x 6 root root 169 Mar 22 00:25 redis-3.3.5
[root@mnl-office-pub-ops-logetl logstash]#
```

\===

如果要安装插件
用最大成功率的离线安装
打开Logstash Plugins地址: https://github.com/logstash-plugins
查找需求的插件

先把安装包下载到logstash 的部署主机中
下载到logstash服务器上，解压

gunzip logstash-filter-fingerprint-3.1.1.tar.gz
tar -xvf logstash-filter-fingerprint-3.1.1.tar

进入Logstash的目录，编辑Gemfile文件，在文件开头添加
```gem "logstash-filter-fingerprint", :path => "/home/elastic/logstash-filter-fingerprint-3.1.1"```

保存退出，执行命令./bin/logstash-plugin install --no-verify安装，提示如下信息

```
[elastic@escluster logstash-5.5.0]$ ./bin/logstash-plugin install --no-verify
Installing...
LogStash::GemfileError: duplicate gem logstash-filter-fingerprint
             add_gem at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/gemfile.rb:109
                 gem at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/gemfile.rb:207
              (eval) at (eval):52
       instance_eval at org/jruby/RubyBasicObject.java:1598
               parse at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/gemfile.rb:195
                load at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/gemfile.rb:19
             gemfile at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/command.rb:4
  install_gems_list! at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/install.rb:146
             execute at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/install.rb:61
                 run at /home/elastic/elasticsearch/logstash-5.5.0/vendor/bundle/jruby/1.9/gems/clamp-0.6.5/lib/clamp/command.rb:67
             execute at /home/elastic/elasticsearch/logstash-5.5.0/vendor/bundle/jruby/1.9/gems/clamp-0.6.5/lib/clamp/subcommand/execution.rb:11
                 run at /home/elastic/elasticsearch/logstash-5.5.0/vendor/bundle/jruby/1.9/gems/clamp-0.6.5/lib/clamp/command.rb:67
                 run at /home/elastic/elasticsearch/logstash-5.5.0/vendor/bundle/jruby/1.9/gems/clamp-0.6.5/lib/clamp/command.rb:132
              (root) at /home/elastic/elasticsearch/logstash-5.5.0/lib/pluginmanager/main.rb:48
```

如果有报错
这是说在Gemfile中存在重复的logstash-filter-fingerprint，因为在原始的Gemfile中，存在通过联网进行安装的logstash-filter-fingerprint，打开Gemfile，找到并注释掉即可

`# gem "logstash-filter-fingerprint"`

再次执行安装命令，即可安装成功

```
[elastic@escluster logstash-5.5.0]$ ./bin/logstash-plugin install --no-verify
Installing...
Installation successful
```

\===

创建用户

```
useradd logstash
chown -R logstash:logstash /usr/local/logstash
```

------

## 安装 elasticsearch

主机
10.70.116.178  mnl-office-pub-ops-elasticsearch

```
# 创建不能登录的系统用户

useradd -s /sbin/nologin elasticsearch

tar -C /usr/local/ -xf /usr/local/src/elasticsearch-v6.7.0.tar.gz
ln -s /usr/local/elasticsearch-6.7.0 /usr/local/elasticsearch

mkdir /data/es-data
mkdir /data/es-log
```

编辑配置文件
vi config/elasticsearch.yml

```
path.data: /data/es-data
path.logs: /data/es-log
```

编辑 /etc/sysctl.conf，追加以下内容：
vm.max_map_count=655360

`echo "vm.max_map_count=655360" >> /etc/sysctl.conf`

保存后，执行：
sysctl -p

/etc/security/limits.conf

```
cat /etc/security/limits.conf | grep -vE '^$|#'
* soft nofile 65535
* hard nofile 65535
* soft nproc 65535
* hard nproc 65535
```

修改相关目录的属主为elasticsearch

```
chown -R elasticsearch:elasticsearch /data/es*
chown -R elasticsearch:elasticsearch /usr/local/elasticsearch*
su elasticsearch

mkdir /var/run/elasticsearch
chown -R elasticsearch:elasticsearch /var/run/elasticsearch
```

配置 serivce 文件

vim /usr/lib/systemd/system/elasticsearch.service

```
[Unit]
Description=Elasticsearch
Documentation=http://www.elastic.co
Wants=network-online.target
After=network-online.target

[Service]
RuntimeDirectory=elasticsearch
Environment=ES_HOME=/usr/local/elasticsearch
Environment=ES_PATH_CONF=/usr/local/elasticsearch/config
Environment=PID_DIR=/var/run/elasticsearch
#EnvironmentFile=-/etc/sysconfig/elasticsearch

WorkingDirectory=/usr/local/elasticsearch

User=elasticsearch
Group=elasticsearch

ExecStart=/usr/local/elasticsearch/bin/elasticsearch -p ${PID_DIR}/elasticsearch.pid --quiet

# StandardOutput is configured to redirect to journalctl since
# some error messages may be logged in standard output before
# elasticsearch logging system is initialized. Elasticsearch
# stores its logs in /var/log/elasticsearch and does not use
# journalctl by default. If you also want to enable journalctl
# logging, you can simply remove the "quiet" option from ExecStart.
StandardOutput=journal
StandardError=inherit

# Specifies the maximum file descriptor number that can be opened by this process
LimitNOFILE=65536

# Specifies the maximum number of processes
LimitNPROC=4096

# Specifies the maximum size of virtual memory
LimitAS=infinity

# Specifies the maximum file size
LimitFSIZE=infinity

# Disable timeout logic and wait until process is stopped
TimeoutStopSec=0

# SIGTERM signal is used to stop the Java process
KillSignal=SIGTERM

# Send the signal only to the JVM rather than its control group
KillMode=process

# Java process is never killed
SendSIGKILL=no

# When a JVM receives a SIGTERM signal it exits with code 143
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target

# Built for distribution-6.7.0 (distribution)
```

------

# 安装 kibana

用源码安装包太麻烦了，后来是使用 rpm 安装成功的

```
wget https://artifacts.elastic.co/downloads/kibana/kibana-6.7.0-x86_64.rpm
rpm -i kibana-6.7.0-x86_64.rpm

vim config/kibana.yml


server.port: 5601



server.host: "10.70.116.168"

elasticsearch.url: "http://10.70.116.178:9200"

kibana.index: ".kibana" 

```

vim /etc/default/kibana

```
user="kibana"
group="kibana"
chroot="/"
chdir="/"
nice=""


# If this is set to 1, then when `stop` is called, if the process has
# not exited within a reasonable time, SIGKILL will be sent next.
# The default behavior is to simply log a message "program stop failed; still running"
KILL_ON_STOP_TIMEOUT=0
```

配置 service文件

vim /etc/systemd/system/kibana.service

```
[Unit]
Description=Kibana

[Service]
Type=simple
User=kibana
Group=kibana
# Load env vars from /etc/default/ and /etc/sysconfig/ if they exist.
# Prefixing the path with '-' makes it try to load, but if the file doesn't
# exist, it continues onward.
EnvironmentFile=-/etc/default/kibana
EnvironmentFile=-/etc/sysconfig/kibana
ExecStart=/usr/local/kibana/bin/kibana "-c /usr/local/kibana/config/kibana.yml"
Restart=always
WorkingDirectory=/

[Install]
WantedBy=multi-user.target
```

------

# 安装 kafka

参考：https://blog.csdn.net/qq_41907991/article/details/94666992

从官网获取装包，https://kafka.apache.org/downloads

```
wget http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.3.0/kafka_2.11-2.3.0.tgz
tar -C /usr/local/ -xf kafka_2.11-2.3.0.tgz
ln -s /usr/local/kafka_2.11-2.3.0 /usr/local/kafka
```

vim /usr/local/kafka/config/zookeeper.properties 

```
dataDir=/tmp/zookeeper
# the port at which the clients will connect
clientPort=2181
# disable the per-ip limit on the number of connections since this is a non-production config
maxClientCnxns=4
tickTime=2000
intLimit=20
syncLimit=10
server.1=10.70.116.168:2888:3888

```

```
cat /usr/local/kafka/config/zookeeper.properties | grep -v '#'
dataDir=/tmp/zookeeper
clientPort=2181
maxClientCnxns=4
tickTime=2000
initLimit=20
syncLimit=10
server.1=10.70.116.168:2888:3888
server.2=10.70.116.178:2888:3888

cat /usr/local/kafka/config/server.properties | grep -vE '#|^$'
broker.id=0
num.network.threads=3
num.io.threads=8
socket.send.buffer.bytes=102400
socket.receive.buffer.bytes=102400
socket.request.max.bytes=104857600
log.dirs=/tmp/kafka-logs
num.partitions=1
num.recovery.threads.per.data.dir=1
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1
log.retention.hours=168
log.segment.bytes=1073741824
log.retention.check.interval.ms=300000
zookeeper.connect=localhost:2181
zookeeper.connection.timeout.ms=6000
group.initial.rebalance.delay.ms=0
[root@mnl-office-pub-ops-elasticsearch src]#
```

```
mkdir /tmp/zookeeper
touch /tmp/zookeeper/myid
echo 1 > /tmp/zookeeper/myid
```

启动
先启动zookeeper

```
/usr/local/kafka/bin/zookeeper-server-start.sh /usr/local/kafka/config/zookeeper.properties > /data/logs/zookeeper/zk.log 2>&1 &
```

再启动 kafka

```
/usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties > /data/logs/kafka/kafka.log 2>&1 &
```

------

# 安装 filebeat 

```
wget -P /usr/local/src https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.7.0-linux-x86_64.tar.gz
tar -C /usr/local/ -xf filebeat-6.7.0-linux-x86_64.tar.gz
ln -s /usr/local/filebeat-6.7.0-linux-x86_64 /usr/local/filebeat
```

配置 service 文件
vim /usr/lib/systemd/system/filebeat.service

```
[Unit]
Description=filebeat server daemon
Documentation=/usr/local/filebeat/filebeat -help
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Environment="BEAT_CONFIG_OPTS=-c /usr/local/filebeat/filebeat.yml"
ExecStart=/usr/local/filebeat/filebeat $BEAT_CONFIG_OPTS
Restart=always

[Install]
WantedBy=multi-user.target
```

------

启动顺序  zookeeper -> kafka -> elasticsearch -> filebeat -> logstash -> kibana

