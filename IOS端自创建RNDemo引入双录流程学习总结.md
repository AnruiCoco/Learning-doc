## IOS端自创建RNDemo引入双录流程学习总结

<p style="text-align: right; font-style: italic">Author: coco  2022/04/01</p>



#### 背景：

应合规要求，在客户进行金融投资理财产品的过程中，应对关键环节同步录音、录像（简称双录）。当前电商APP对接的是恒生的双录企业服务，通过集成恒生双录SDK实现双录需求。由于我司电商APP的框架为React Native，而对方之前未有过RN集成经验，因此特开发一个RN Demo来与恒生开发同事对接sdk的集成过程。



以下内容为个人学习初始化RN项目并集成恒生SDK全过程的学习输出，末尾附带了学习期间遇到的问题总结，供大家参阅。



#### 一、配置环境

Xcode 13.1

MacOS Big Sur Version 11.5.2



#### 二、新建RNDemo项目

1、使用 React Native 内建的命令行工具来创建一个名为"RNCocoDemo"的新项目

```shell
npx react-native init 项目名称
```

2、编译并运行 RNCocoDemo应用

```shell
cd RNCocoDemo
yarn ios
```



#### 三、XCode工程设置

##### 1、将所需依赖包及资源文件(HCDR.bundle 、HCDRViewController.h 、libHCDR.a)导入到工程中

导入步骤：

（1）Xcode打开工程目录，即项目中的ios文件夹

（2）分别将 HCDR.bundle 、HCDRViewController.h 、libHCDR.a 这三个依赖包手动拖入到左侧目录中，勾选红框的选项，点击Finish

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220330092552997.png" alt="image-20220330092552997" style="zoom:30%;" />

##### 2、如同上述的导入步骤（2），将双录SDK所需的开源库AFNetworking 和 MBProgressHUD也手动导入到左侧目录下

##### 3、添加双录SDK所需的系统依赖库libz.tbd、libc++.tbd 、AudioToolbox.framework 、VideoToolbox.framework 、AVFoundation.framework 、Foundation.framework 、UIKit.framework

添加步骤：

（1）选择Target—Build Phases—Link Binary with Libraries，点击“+”号

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220330102425418.png" alt="image-20220330102425418" style="zoom:30%;" />

（2）分别搜索依赖库名称然后添加，如下图

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331150507064.png" alt="image-20220331150507064" style="zoom:30%;" />

（3）全部添加完成后，效果如下图

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331150713605.png" alt="image-20220331150713605" style="zoom:40%;" />



##### 4、设置权限

设置步骤：

（1）选择Target—Info—Custom IOS Target Properties

（2）添加App Transport Security Settings -> Allow Arbitrary Loads 为YES

（3）添加Privacy - Camera Usage Description 是否允许使用相机

（4）添加Privacy - Microphone Usage Description 是否允许使用⻨克⻛

（5）添加Privacy - Photo Library Usage Description 是否允许访问相册

（6）全部添加完成后，如下图

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331150049778.png" alt="image-20220331150049778" style="zoom:40%;" />



##### 5、设置项目签名信息

（1）选择Target— Signing & Capabilities—Signing, 由于当前项目是个人项目，所以Team选择自己的Apple Id，没有的先需要自行创建

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331151957920.png" alt="image-20220331151957920" style="zoom:50%;" />

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220330111000875.png" alt="image-20220330111000875" style="zoom:50%;" />



##### 6、进入项目下的ios文件夹，安装依赖包，确保所有依赖包安装完成

```shell
cd ./RNCocoDemo/ios
pod install
```



#### 四、XCode连接真机运行项目

（1）将ios手机通过数据线连接电脑，信任设备后在XCode设备选择处选择识别成功的设备，最后点击左边的▶️按钮运行

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331153117415.png" alt="image-20220331153117415" style="zoom:50%;" />



（2）运行成功后，会提示“Build Sucess”，在running on your device时会出现以下提醒

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331134009797.png" alt="image-20220331134009797" style="zoom:50%;" />

这种提示是告诉你，应用已经装到手机上了,但是点击应用,提示不受信任的开发者,提示你需要到设置中去设置。

此时，打开设置-->通用-->设备管理-->你的账户名称-->点击信任,就可以解决该问题了。



##### 8、玩转RNDemo

（1）手机桌面上点击应用进入页面

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331154043608.png" alt="image-20220331154043608" style="zoom:50%;" />



（2）就可以正常进入双录流程了

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331154708037.png" alt="image-20220331154708037" style="zoom:25%;" />





#### 五、问题记录

##### （1）未集成sdk前，新创建demo后执行yarn ios报错

报错信息：

![image-20220329170359449](/Users/anrui/Library/Application Support/typora-user-images/image-20220329170359449.png)

解决方案：https://www.cnblogs.com/elimsc/p/15844311.html

前往node_modules/react-native/scripts/find-node.sh, 注释掉文件里面的这部分内容

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220329171315917.png" alt="image-20220329171315917" style="zoom:50%;" /> 



##### （2）连接真机运行报错

报错信息

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331152655290.png" alt="image-20220331152655290" style="zoom:50%;" />

解决方案：删除pods文件夹，重新执行podinstall



##### （3）连接真机运行报错

![image-20220331164412667](/Users/anrui/Library/Application Support/typora-user-images/image-20220331164412667.png)

按照提示执行pod install后发现依旧报同样的错误

解决方案：

关闭当前xcode项目，使用finder打开项目文件夹，找到RNCocoDemo.xcworkspace文件，单机右键用xcode打开，然后运行项目即可

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331164627419.png" alt="image-20220331164627419" style="zoom:40%;" />