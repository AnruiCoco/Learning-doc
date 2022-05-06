## Android端自创建RNDemo引入双录流程学习总结

<p style="text-align: right; font-style: italic">Author: coco  2022/04/11</p>



#### 背景：

应合规要求，在客户进行金融投资理财产品的过程中，应对关键环节同步录音、录像（简称双录）。当前电商APP对接的是恒生的双录企业服务，通过集成恒生双录SDK实现双录需求。由于我司电商APP的框架为React Native，而对方之前未有过RN集成经验，因此特开发一个RN Demo来与恒生开发同事对接sdk的集成过程。



以下内容为个人学习初始化RN项目并集成恒生SDK全过程的学习输出，末尾附带了学习期间遇到的问题总结，供大家参阅。





#### 一、配置环境

Android Studio Arctic Fox (2020.3.1) 稳定版 

MacOS Big Sur Version 11.5.2





#### 二、新建RNDemo项目

1、使用 React Native 内建的命令行工具来创建一个名为"RNCocoDemo"的新项目

```shell
npx react-native init 项目名称
```

2、编译并运行 RNCocoDemo应用

```shell
cd RNCocoDemo
yarn android
```





#### 三、Android Studio工程设置

##### 1、修改 app 目录下的 build.gradle 文件配置

（1）给 CPU 架构指定 ABI

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406185218821.png" alt="image-20220406185218821" style="zoom:50%;" />

（2）在 android 代码块中声明本地库路径

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406185303950.png" alt="image-20220406185303950" style="zoom:50%;" />



##### 2、引入 jar 包或 aar 包依赖

（1）添加依赖包

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406185715660.png" alt="image-20220406185715660" style="zoom:50%;" />

（2）引入依赖包

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406185430778.png" alt="image-20220406185430778" style="zoom:50%;" />



另：手动添加依赖包，如下图

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220401160204264.png" alt="image-20220401160204264" style="zoom:40%;" />



##### 3、创建原生模块

原生代码，在此不做过多介绍，可参考《Android双录SDK集成开发文档_ReactNative桥接.pdf—2.2、2.3》

##### 4、注册原生模块

原生代码，在此不做过多介绍，可参考《Android双录SDK集成开发文档_ReactNative桥接.pdf—2.4》

##### 5、将原生模块与注册模块添加到下图目录中

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406190455356.png" alt="image-20220406190455356" style="zoom:50%;" />

##### 6、修改MainApplication文件

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406190856140.png" alt="image-20220406190856140" style="zoom:30%;" />





#### 四、Android Studio模拟器运行项目

（1）执行yarn android

（2）点击按钮

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406191118992.png" alt="image-20220406191118992" style="zoom:30%;" />

（3）就可以正常进入双录流程了

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220331154708037.png" alt="image-20220331154708037" style="zoom:25%;" />





#### 五、问题记录

#####  （1）项目gradle与当前的AndroidStudio 版本冲突

报错信息：

Unable to determine application id: com.android.tools.idea.run.ApkProvisionException: The currently selected variant "debug" uses split APKs, but none of the 1 split apks are compatible with the current device with ABIs "arm64-v8a".

解决办法：

https://blog.csdn.net/u010632547/article/details/105724003



##### （2）内存不足报错

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406191835909.png" alt="image-20220406191835909" style="zoom:50%;" />

解决办法：

清空模拟器内存，如若仍不生效，执行以下步骤

- Turn off **Enable Hot-swap agent for Groovy code ** feature from settings(File ~> Settings ~> Build, Execution, Deployment ~> Hot Debugger).
- Perform a **clean build**.
- Turn on **Enable Hot-swap agent for Groovy code ** feature from settings.
- Perform a clean build.

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406192331515.png" alt="image-20220406192331515" style="zoom:30%;" />

（3）包名添加白名单

报错信息：打Release包，打开没问题，开始流程后，如有下面弹框

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220406192702500.png" alt="image-20220406192702500" style="zoom:40%;" />

解决办法：联系恒生将你的安卓包名com.xxx.m加入白名单