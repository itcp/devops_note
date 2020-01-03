

[TOC]

参考:

https://www.ifeegoo.com/using-jenkins-to-set-up-the-continuous-integration-environment-of-android-and-ios-on-macos.html



## 必要应用安装

系统参数

```
opserdeMac-mini:~ opser$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.14.6
BuildVersion:	18G95
opserdeMac-mini:~ opser$ 
```





1. 安装Xcode

   去除解压验证 
   `xattr -d com.apple.quarantine Xcode_11_beta.xip`

   双击解压,使用系统自带的“实用归档工具”

   

2.安装cocoapods

`sudo gem install cocoapods`



## 配置开发者帐户及签名

jenkins用户登录进入mac

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025145322.png)

生成秘钥

```
opserdeMac-mini:~ jenkins$ ssh-keygen -t rsa -C "hjtcios@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/Shared/Jenkins/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/Shared/Jenkins/.ssh/id_rsa.
Your public key has been saved in /Users/Shared/Jenkins/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:G76m9oibZBArqr0HMb+S2L3wEx9xG/RN+XLw9EDPCjA hjtcios@gmail.com
The key's randomart image is:
+---[RSA 2048]----+
|          E  o.  |
|        .  o+ oo |
|  .    . . o.= oo|
|  oo  . o . o.+..|
|. o+   oSo   o.  |
|....o ...o       |
|.o.+o+ .o        |
|o.+==oo...       |
|. o+*=o+o        |
+----[SHA256]-----+
opserdeMac-mini:~ jenkins$ 
```



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025151532.png)



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025151720.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025151814.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025151843.png)

\--





然后登录开发者官网 https://developer.apple.com/

进入证书\ID栏目



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025150409.png)

\--

生成开发证书



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025150738.png)



\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025150847.png)



\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025150926.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025152005.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025152055.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025152138.png)





![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025152323.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025152537.png)

\--

生成发布证书

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025205447.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025205720.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025210110.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025210143.png)

------



变量

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025152752.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025152855.png)

\-

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025153018.png)

\---

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025153115.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025153155.png)

\--

选择测试设备

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025153321.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025153424.png)

\--

下载生成的证书(打包时所用)

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025153508.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025153659.png)

\--

![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025154151.png)



iOS Developer开发者证书(Certificates)，可通过以下命令在终端查看。

security find-identity -v -p codesigning

```
opserdeMac-mini:~ jenkins$ security find-identity -v -p codesigning
  1) 4F41F442C964FAF69C4AEDA***********8 "Apple Development: ios HJ (D6H****S)"
     1 valid identities found
opserdeMac-mini:~ jenkins$ 
```



\--



![](/home/boeving/gitnote/diary_note/fj/2019/build/ios/imgs/DeepinScreenshot_select-area_20191025154530.png)

