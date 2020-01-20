

[TOC]



参考:

https://www.cnblogs.com/zz0412/p/jenkins_jj_12.html

https://www.jianshu.com/p/f445983512b7



实验例子版本为: Jenkins ver. 2.190.1

MacOS 节点要确认已安装 Xcode\git





## 1.登录Jenkins，添加MacOSX节点







![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025104301.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025104831.png)

\--

## 2.输入节点名称，勾选PermanentAgent



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025110824.png)



\--





------



## 3.开启要绑定OSX系统电脑的远程登录

在mac 电脑上操作

系统偏好设置->共享->勾选->远程登录



获取远程登录的用户名和IP

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025111243.png)



\--



\# of executors：最大同时构建数量(根据机器的性能定，单颗四核cpu建议不要超过4)【必须为数字】

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025111959.png)



我这台机器是6核心的,所以我填写6



若没有Launch slave agents on Unix machines via SSH选项，需要安装SSH Slaves plugin插件
若没有Keychains and Provisioning Profiles Management选项，需要安装kpp-management-plugin插件



添加远程连接用户,点一下Add 那里,会有Jenkins出现

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025135128.png)

/Users/Shared/Jenkins/



先在mac机上创建jenkins用户

dscl . -create /Users/jenkins
dscl . -create /Users/jenkins UserShell /bin/bash
修改密码:
dscl . -passwd /Users/jenkins

stu@build



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025141000.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191026105012.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025141720.png)



```
sudo mkdir -p /data/jenkins/worksqace
sudo chmod -R 777 /data/jenkins/worksqace
```



\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191026105957.png)



\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025142022.png)

\----

没有成功运行agent 节点

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025142415.png)

点击"See log for more details",查看这个日志

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025142512.png)

```
/var/lib/jenkins/.ssh/known_hosts [SSH] No Known Hosts file was found at /var/lib/jenkins/.ssh/known_hosts. Please ensure one is created at this path and that Jenkins can read it.
```

这是说 

```
[root@jenkins_master ~]# mkdir /var/lib/jenkins/.ssh
[root@jenkins_master ~]# ssh jenkins@10.70.128.30
The authenticity of host '10.70.128.30 (10.70.128.30)' can't be established.
ECDSA key fingerprint is SHA256:OgbtiIZ9frWpiJlR6M4fq2OhkVHG7BAa4nE0j73CJuQ.
ECDSA key fingerprint is MD5:6d:6d:17:4d:17:17:de:4a:f4:95:78:d0:be:f8:ce:60.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.70.128.30' (ECDSA) to the list of known hosts.
Password:
Last login: Fri Oct 25 14:05:30 2019 from 10.70.128.96
opserdeMac-mini:~ jenkins$ exit
logout
Connection to 10.70.128.30 closed.
[root@jenkins_master ~]#
[root@jenkins_master ~]# cp .ssh/known_hosts /var/lib/jenkins/.ssh/.
[root@jenkins_master ~]# chown -R jenkins:jenkins /var/lib/jenkins
```

点 "Relaunch agent"重新启动



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025142936.png)

\--

这样就正常启动了agent节点

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025144315.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025144425.png)

------

