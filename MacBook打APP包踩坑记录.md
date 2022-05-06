# MacBook打APP包踩坑记录

### 一、Xcode打包



#### 报错情况一：

（仅M1电脑存在此问题）

![image-20211202095127580](/Users/anrui/Library/Application Support/typora-user-images/image-20211202095127580.png)

![image-20211202095157099](/Users/anrui/Library/Application Support/typora-user-images/image-20211202095157099.png)

解决办法：
![image-20211201152310775](/Users/anrui/Library/Application Support/typora-user-images/image-20211201152310775.png)

同时，修改podfile文件如下，添加红框部分代码：

![image-20211201152425083](/Users/anrui/Library/Application Support/typora-user-images/image-20211201152425083.png)

然后重新编译打包即可

参考文章：https://stackoverflow.com/questions/63607158/xcode-12-building-for-ios-simulator-but-linking-in-an-object-file-built-for-io

https://narlei.com/development/apple-m1-xcode-error-when-build-in-simulator/

https://www.reddit.com/r/swift/comments/mw8djk/do_anyone_know_what_is_the_reason_behind_exclude/



#### 报错情况二：

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308184535578.png" alt="image-20220308184535578" style="zoom:50%;" /> 

**原因分析：** 应该是 `main.jsbundle` 的加载路径出现了问题

在 `Build Phases` 中可以看到 `Bundle React Native code and images` 这一栏中

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308184742005.png" alt="image-20220308184742005" style="zoom:50%;" /> 

大概意思是通过 `node` 运行 `react-native-xcode.sh` 这个脚本来加载图片，但是由于 `M1` 的 `node` 所在的路径与之前的都不一样，导致了 `Xcode` 在运行时无法正确使用 `node` 指令

**解决办法：** 打开**终端**，输入：`which node`，得到一下路径：`/opt/homebrew/bin/node`
 将这个路径添加到 `Bundle React Native code and images` 中

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308184841274.png" alt="image-20220308184841274" style="zoom:50%;" /> 

重新运行即可



#### 报错情况三（报错信息基本同上）：

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308184535578.png" alt="image-20220308184535578" style="zoom:50%;" /> 

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220505210808769.png" alt="image-20220505210808769" style="zoom:40%;" />

**原因分析：** 应该是 `main.jsbundle` 的加载路径出现了问题

在 `Build Phases` 中可以看到 `Bundle React Native code and images` 这一栏中

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308184841274.png" alt="image-20220308184841274" style="zoom:50%;" />

我们虽然安装了watchman，且版本为最新的，但是M1芯片电脑的访问路径存在bug，导致xcode无法识别到watchman的路径，引发报错。

**解决办法：** 打开**终端**，输入：`which node`，得到一下路径：`/opt/homebrew/bin/node`
 将这个路径添加到 `Bundle React Native code and images` 中

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220505210635009.png" alt="image-20220505210635009" style="zoom:50%;" />

重新运行即可

注：https://stackoverflow.com/questions/58675179/error-emfile-too-many-open-files-react-native-cli

answered Dec 1, 2021 at 0:37









### 二、Android Studio打包



#### 报错情况一：

（仅M1电脑存在此问题）

#### Gradle project Sync Failed报错

Caused by: java.io.IOException: Cannot run program "node": error=2, No such file or directory ......

原因：gradle release版本与M1芯片不兼容，

可参考gradle官方release文档：https://docs.gradle.org/6.9/release-notes.html

解决办法：
升级gradle版本至6.9.0或以上

修改gradle script下的gradle-wrapper-properties文件中的gradle文档

![image-20211201153555482](/Users/anrui/Library/Application Support/typora-user-images/image-20211201153555482.png)

重启Android Studio后重新build即可



#### 报错情况二： 

#### <img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308193055483.png" alt="image-20220308193055483" style="zoom:50%;" />

解决办法：打开Android studio，点击右上角栏目中的“Sync project with Gradle Files” , 根据最新的gradle文件更新项目引用库

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308193327835.png" alt="image-20220308193327835" style="zoom:50%;" /> 

