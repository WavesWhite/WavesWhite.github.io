---
title: Maven安装教程
top_img: https://w.wallhaven.cc/full/l8/wallhaven-l8qmlp.jpg
cover: https://w.wallhaven.cc/full/gp/wallhaven-gpzve7.png
abbrlink: Maven安装教程
tags:
    - JAVA
    - SpringBoot
    - Maven
categories:
    - 杂七杂八

date: 2022/10/26 20:30:51 
---

><font size=4>本文须知：安装maven环境之前要先安装java jdk环境 (Maven 3.3+ require JDK 1.7 及以上)</font> 

><font size=4>本文环境：JDK：1.8.0、Maven：3.6.3</font> 

# 第一步：下载 Maven
><font size=4>官方下载链接：https://maven.apache.org/download.cgi</font> 

<font size=4>Binary是可执行版本，已经编译好可以直接使用</font> 

<font size=4>Source是源代码版本，需要自己编译成可执行软件才可使用</font> 


><font size=4>官网经常上不去，下载不成功，可以在下面的百度云盘获取：(Maven：3.6.3)</font> 

><font size=4>链接：https://pan.baidu.com/s/1a9UkvEgwiucwP7oTK3HV-Q?pwd=9ng3    提取码：9ng3</font> 


<font size=4>选择已经编译好的windows版本进行安装：选择zip版本(如下图):</font>
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-20-55-22.png)

<font size=4>解压完后：</font>
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-20-56-31.png)


# 第二步：Maven 环境变量配置
<font size=4>（这里我没有这个需求，就没有去配置环境变量~~绝对不是因为懒~~，所以用了网上的截图）</font>

![](../../../img/posts_img/Maven安装教程/2022-10-26-21-00-50.png)

![](../../../img/posts_img/Maven安装教程/2022-10-26-21-00-55.png)

- <font size=4>开始配置环境变量（点击系统变量，新建按钮）：</font>
  
    ><font size=4>新建系统变量：MAVEN_HOME=D:\\maven\\apache-maven-3.8.4 (以**自己安装的路径和版本**为准)</font>
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-02-01.png)

- <font size=4>编辑变量Path：</font>
  
    ><font size=4>添加变量值：%MAVEN_HOME%\\bin</font>
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-03-37.png)
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-03-42.png)

# 第三步：验证Maven环境配置是否成功

> <font size=4>终端输入命令：输入命令：mvn -version</font>

- <font size=4>安装成功</font>
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-07-03.png)



# 第四步：配置Maven仓库以及相关设置

- <font size=4>1：在 maven同级目录下建一个maven仓库（找一个适合管理的位置）</font>
  ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-11-04.png)

- <font size=4>2：在路径 C:\\Users\\User\\Desktop\\Maven\\apache-maven-3.6.3\\conf (自己安装maven的目录路径) 找到 settings.xml配置文件</font>
  ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-14-39.png)
  - <font size=4>找到节点**localRepository**，在注释外添加**自己仓库的地址**</font>
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-16-02.png)

  - <font size=4>配置镜像（采用国内阿里云的镜像下载依赖会快很多）：</font>
  
    ><font size=4>在settings.xml配置文件中找到mirrors节点,添加如下配置（注意要添加在<mirrors>和</mirrors>两个标签之间，其它配置同理）,放在默认节点的前面</font>
    ~~~xml
    <!-- 阿里云仓库 -->
    <mirror>
        <id>alimaven</id>
        <mirrorOf>central</mirrorOf>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
    </mirror>
    ~~~
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-21-35.png)

  - <font size=4>配置JDK：</font>
  
    ><font size=4>在settings.xml配置文件中找到profiles节点，添加如下配置：</font>
    ~~~xml
    <!-- java jdk1.8版本 -->
    <profile>
      <id>jdk-1.8</id>
      <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>        
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
    </profile>
    ~~~
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-32-06.png)
    
<font size=4>配置完成，进入终端，输入命令：mvn help:system 测试，配置成功则本地仓库如下图显示：</font>
![](../../../img/posts_img/Maven安装教程/2022-10-26-21-33-36.png)

><font size=4>首次执行 mvn help:system 命令，Maven相关工具自动帮我们到Maven中央仓库下载缺省的或者Maven中央仓库更新的各种配置文件和类库（jar包) 到Maven本地仓库中。</font>

><font size=4>下载完各种文件后， mvn help:system 命令会打印出所有的Java系统属性和环境变量，这些信息对我们日常的编程工作很有帮助。</font>

# 添加Maven到IDEA中

><font size=4>本地的Maven文件一般是配套IDEA一起使用，如何让每一次新建项目都选中自己的maven，本文推荐以下方案解决（避免每一次都要手动修改maven配置）：</font>

- <font size=4>**新建一个Maven项目**，进入设置可以发现该新建项目没有指向我们本地的maven地址</font>
  ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-42-33.png)

- <font size=4>配置新建项目设置</font>
  ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-44-37.png)

- <font size=4>找到Maven设置，选择自己的Maven目录地址、配置setting文件及仓库地址如下图：</font>
  ![](../../../img/posts_img/Maven安装教程/2022-10-26-21-49-43.png)



# 注意事项
<font size=4>如果Maven的版本高于IDEA的版本，有可能会出现这样的报错：</font>
    ![](../../../img/posts_img/Maven安装教程/2022-10-26-22-04-31.png)
<font size=4>出现这种错误最好的解决办法就是：**用当前IDEA自带的Maven插件和自带的Maven**，因为这样最不容易出现版本问题</font>

<font size=4>手动降低Maven版本也行，Maven版本最好和IDEA同一年发布的，这样不容易出错</font>

    


参考链接

- [史上最详细的Maven安装教程](https://blog.csdn.net/weixin_44080187/article/details/122933194)
- [IDEA Maven工程出现org.codehaus.plexus.component.repository.exception.ComponentLookupException错误](https://blog.csdn.net/xuruilll/article/details/125655668)