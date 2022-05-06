### 电商APP的CICD流程与操作说明

------

#### 一、背景说明

(1) ***RN项目发包现状***

​      耗时长，占用人力和时间，经常导致电脑卡死

(2) ***CICD是什么？***

​		  CICD=CI（持续集成）+ CD（持续交付&持续部署）

(3) ***了解一下DevOps***

​          DevOps 强调的是高效组织团队之间如何通过自动化的工具协作和沟通来完成软件的生命周期管理，从而更快、更频繁地交付更稳定的软件。

  <span style="font-size: 12px; color: #999">DevOps突出重视软件开发人员和运维人员的沟通合作，通过自动化流程来使得软件构建、测试、发布更加快捷、频繁和可靠。DevOps 其实包含了三个部分：开发、测试和运维。换句话 DevOps 希望做到的是软件产品交付过程中IT工具链的打通，使得各个团队减少时间损耗，更加高效地协同工作。</span>

<img src="https://pic3.zhimg.com/80/v2-702b05c2dad9d5626bf4f747dfa51406_1440w.jpg?source=1940ef5c" alt="img" style="zoom:50%;" />

#####   <span style="font-size: 12px; color: #999">随着DevOps的兴起，软件开发与交付方式也发生了巨大改变，传统的方法已经无法适应和满足如今的产品快速业务迭代的需求，持续集成、持续交付、持续部署（简称CICD）应运而生。</span>

(4) ***CI、CD、DevOps关系***

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220305195331492.png" alt="image-20220305195331492" style="zoom:40%;" />

<p style="font-size: 12px; color: #999;text-align:center">软件开发的管理流程</p>

关系总结：DevOps是理论、CICD是实践





#### 二、前期架构设计

涉及到的技术点：

1、Mac电脑与jenkins服务器的网络调度

2、Anroid、IOS不依赖IDE的命令行打包方案设计

3、Mac的打包进度向jenkins服务器反馈，jenkins日志输出与jenkins显示进度联通

4、两端Node服务器以及API连接与脚本设计

5、Mac电脑与开发服务器上传包网络及方案

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220228194007031.png" alt="image-20220228194007031" style="zoom:50%;" /> 

#### 三、实现流程

实现总流程如下图：

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220221095720344.png" alt="image-20220221095720344" style="zoom:50%;" /> 

具体流程介绍：

\-    Jenkins与本机共享一套代码，代码存放于gitlab仓库中

\-    Jenkins与本机分别启动一个Node Server服务

\-    Jenkins向本机发送start开始请求，本机接收请求后开始执行打包脚本

\-    本地在执行打包脚本过程中会在每一步生成日志文件的同时向jenkins端发送日志信息

\-    Jenkins接收日志信息后在构建进度页面中展示日志内容

\-    本机打包完毕后，会对包以环境命名，然后上传至远程的开发服务器上，供分发网站下载使用

\-    上传完成后，脚本执行结束，向jenkins端发送打包结束的请求

\-    Jenkins接收到结束请求后关闭node服务

\-    结束



#### 四、核心配置过程

**1、配置项目的jenkins**

（1）General、源码管理、构建触发器、构建环境配置同其他项目

（2）点击“构建”选项，再点击“增加构建步骤”，选择“Execute NodeJS script”，配置构建命令

（3）再点击“增加构建步骤”，选择“Execute shell”，继续配置构建命令，执行安装node服务的依赖包、启动node 服务

（4）点击“保存/应用”

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220221102322127.png" alt="image-20220221102322127" style="zoom:50%;" /> 



**2、编写打包脚本**

android打包脚本：

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220221103356736.png" alt="image-20220221103356736" style="zoom:50%;" />



ios打包脚本：

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220221103817449.png" alt="image-20220221103817449" style="zoom:100%;" /> 



#### 五、操作与结果截图

实现效果（发布APP包）：本地提交代码 —> Gitlab合并代码（ jenkins点击部署） —> 分发地址下载APP测试

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220221100104302.png" alt="image-20220221100104302" style="zoom:50%;" />



<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220221100428980.png" alt="image-20220221100428980" style="zoom:50%;" />



<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220221100827035.png" alt="image-20220221100827035" style="zoom:40%;" />



#### 六、后续优化

（1）分发下载网站页面优化，显示包更新时间（4月中旬）

（2）日志展示优化（4月底）

（3）版本不需要更改配置（3月底）

（4）版本可以同步更新所有的代码中关于版本号的代码（3月底）

（5）除生产外，在包内登录页/首页显示包环境（3月底）

| 代办事项                                     | 完成情况                          |
| -------------------------------------------- | --------------------------------- |
| 分发下载网站页面优化，显示包更新时间         | ✅ 90%，需要配置公司的打包电脑验证 |
| 日志展示优化                                 | 居家无法使用公司打包机器          |
| 版本不需要更改配置                           | ✅                                 |
| 版本可以同步更新所有的代码中关于版本号的代码 | ✅                                 |
| 除生产外，在包内登录页/首页显示包环境        | ✅                                 |





#### 七、jenkins参数化配置

每个版本迭代，版本号不同，如果每次都需要去修改配置来变动版本号，相对比较麻烦，但是如果能在构建时直接输入版本号，就会简单很多，下面将介绍具体配置过程

第一步：打开配置，选择General选项，勾选“This project is parameterized”，点击“添加参数”，选择“String Paramter”

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308211645939.png" alt="image-20220308211645939" style="zoom:50%;" />



第二步：输入你需要参数化的信息，如下图

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308211452270.png" alt="image-20220308211452270" style="zoom:50%;" /> 



第三步：点击“构建”选项，将原先版本号的地方写成动态参数，参数名为第二步中自己定义的参数名称，注意，由于这儿我们执行的是node脚本，所以我们使用process.env.xx来获取动态参数

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220310171327463.png" alt="image-20220310171327463" style="zoom:50%;" />



第四步，构建时点击“Build with Parameters”，此时右边会出现version的一个输入框，默认值是你之前设置的，可以修改，然后点击“开始构建”即可开始构建

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220308211741639.png" alt="image-20220308211741639" style="zoom:50%;" />