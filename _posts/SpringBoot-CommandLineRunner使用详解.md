---
title: SpringBoot-CommandLineRunner使用详解
top_img: https://w.wallhaven.cc/full/vq/wallhaven-vqml88.jpg
cover: https://w.wallhaven.cc/full/o5/wallhaven-o5dqq9.jpg
abbrlink: SpringBoot-CommandLineRunner使用详解
tags:
    - JAVA
    - SpringBoot
    - CommandLineRunner
    - 预加载
categories:
    - 杂七杂八

date: 2022/11/2 22:57:13 
---

# 问题背景

<font size=4>如果希望在SpringBoot应用启动时进行一些初始化操作，可以选择SpringBoot提供了一个简单的方式来实现此类需求：选择使用CommandLineRunner来进行处理</font>

<font size=4>只需要实现CommandLineRunner接口，并且把对应的bean注入容器。把相关初始化的代码重新到需要重新的方法中</font>

<font size=4>这样就会在应用启动的时候执行对应的代码</font>


# 代码实现
~~~java
@Component  // 注入容器
@Order(value = 1)   // 如果存在多个CommandLineRunner组件方法，可以使用 @Order() 注解指定加载顺序，如果不指定value参数，默认为：2147483647
public class TestCommandLineRunner implements CommandLineRunner {   // 不能放在test目录中，要放在启动类的同级目录下

    /**
     * CommandLineRunner：
     * 这是一个接口，用户可以自定义实现该接口，具体实现run方法
     * 任何在上下文容器之内的bean都可以实现run方法
     * 如果在上下文中，存在多个该接口实现类，可以通过@order注解，指定加载顺序
     */

    @Override
    public void run(String... args) throws Exception {
        System.out.println("这是一个测试CommandLineRunner的测试方法");
        System.out.println("程序初始化中......");
    }
}
~~~


# 注意

<font size=4>启动CommandLineRunner的执行其实是整个应用启动的一部分，**项目是在CommandLineRunner执行完成之后才启动完成的**</font>

<font size=4>如果CommandLineRunner执行中的操作影响到了主线程，可以**重新开启一个线程**，让CommandLineRunner单独去做我们想要做的操作</font>

~~~java
@Component
@Order(value = 1)
public class TestCommandLineRunner implements CommandLineRunner {
 
    @Override
    public void run(String... strings){
        new Thread(){
            public void run() {
                int i = 0;
                while (true) {
                    i++;
                    try {
                        Thread.sleep(10000);
                        System.out.println("过去了10秒钟……,i的值为：" + i);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (i == 4) { //第40秒时抛出一个异常
                        throw new RuntimeException();
                    }
                    continue;
                }
            }
        }.start();
    }
}
~~~


参考链接

- [使用 SpringBoot 的 CommandLineRunner 遇到的坑](https://zhuanlan.zhihu.com/p/366528471)


