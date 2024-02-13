---
title: SpringBoot运行多模块项目时出现异常：程序包xxx.xxx.xxx不存在
top_img: https://w.wallhaven.cc/full/rd/wallhaven-rdk7wm.jpg
cover: https://w.wallhaven.cc/full/pk/wallhaven-pkq3k9.jpg
abbrlink: SpringBoot运行多模块项目时出现异常：程序包xxx.xxx.xxx不存在
tags:
    - JAVA
    - SpringBoot
    - Maven
categories:
    - 杂七杂八

date: 2022/8/9 17:12:38 
---
# 问题

![](../../../img/posts_img/SpringBoot运行多模块项目时出现异常：程序包xxx.xxx.xxx不存在/2022-08-09-17-04-57.png)


# 解决方法

<font size=5>将项目的.idea和.imi文件删除，重新导入项目，重新生成.idea和.imi文件</font>

- <font size=4>删除文件</font>

    ![](../../../img/posts_img/SpringBoot运行多模块项目时出现异常：程序包xxx.xxx.xxx不存在/2022-08-09-17-05-07.png)


- <font size=4>重新导入项目</font>

    ![](../../../img/posts_img/SpringBoot运行多模块项目时出现异常：程序包xxx.xxx.xxx不存在/2022-08-09-17-08-43.png)


- <font size=4>选择Maven</font>

    ![](../../../img/posts_img/SpringBoot运行多模块项目时出现异常：程序包xxx.xxx.xxx不存在/2022-08-09-17-09-11.png)


- <font size=4>配置项目JDK</font>

    ![](../../../img/posts_img/SpringBoot运行多模块项目时出现异常：程序包xxx.xxx.xxx不存在/2022-08-09-17-12-17.png)


- <font size=4>**重新编译项目，编译成功**</font>

参考链接

- [项目maven依赖成功，但编译一直报错：引用项目的类路径找不到](https://blog.csdn.net/qq_38069453/article/details/78332992)