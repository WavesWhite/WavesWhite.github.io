---
title: SpringBoot定时任务@EnableScheduling注解使用方法与cron表达式
top_img: https://w.wallhaven.cc/full/p9/wallhaven-p9273e.jpg
cover: https://w.wallhaven.cc/full/we/wallhaven-wey8zx.jpg
abbrlink: SpringBoot定时任务@EnableScheduling注解使用方法与cron表达式
tags:
    - JAVA
    - SpringBoot
    - EnableScheduling
    - cron
categories:
    - 杂七杂八

date: 2022/11/12 20:54:55 
---
<!-- <font size=4></font> -->

# 前言
## 定时任务的作用？
<font size=4>定时任务相当于闹钟，在什么时间做什么事情（执行什么命令/脚本）</font>

## 定时任务的实现？
<font size=4>定时任务的实现方式有很多，比如XXL-Job等。但是其实核心功能和概念都是类似的，很多情况下只是调用的API不同而已</font>

<font size=4>这里就先用SpringBoot为我们提供的定时任务API的@EnableScheduling注解来简单实现一下定时任务</font>


# @EnableScheduling注解使用
## 导入依赖
~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>2.5.0</version>
</dependency>
~~~

## 启动类里面使用 @EnableScheduling 注解开启功能，自动扫描
~~~java
@SpringBootApplication
@EnableScheduling   // 开启定时任务
public class QiNiuTestApplication {
    private static SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd / HH:mm:ss");

    public static void main(String[] args) {
        System.out.println("当前时间：\t" + simpleDateFormat.format(new Date()));    // 输出当前时间
        SpringApplication.run(QiNiuTestApplication.class, args);
    }
}
~~~

## 定时任务测试方法
- <font size=4>任务的类上写 @Component</font>
- <font size=4>任务方法上写 @Scheduled</font>
~~~java
@SpringBootTest
@Component  // 注入容器
public class SchedulingTest {

    // 初始延迟10秒，每隔3秒
    // @Scheduled(fixedRate = 3000, initialDelay = 10000)

    // 每次执行完延迟3秒
    // @Scheduled(fixedDelay = 3000)

    // 每次隔3秒执行一次
    // @Scheduled(fixedRate = 3000)

    // 从第0秒开始，每次隔5秒执行一次
    // @Scheduled(cron = "0/5 * * * * ?")

    // 每隔5秒执行一次
    // @Scheduled(cron = "*/5 * * * * ?")

    // 从第0分钟开始，每隔10分钟执行一次
    // @Scheduled(cron = "* 0/10 * * * ?")

    // 每小时的第10分钟到20分钟内，每分钟执行一次1·
    // @Scheduled(cron = "0 0-10 * * * ?")

    // 每个月 每日 0点与8点的 0分与30分的 0秒 各执行一次
    // @Scheduled(cron = "0 0,30 0,8 * * ?")
    /*
    执行结果：
    2022-11-05 00:00:00
    2022-11-05 00:30:00
    2022-11-05 08:00:00
    2022-11-05 08:30:00
    2022-11-06 00:00:00
    2022-11-06 00:30:00
    2022-11-06 08:00:00
    2022-11-06 08:30:00
    */

    // 每月1号凌晨2点执行一次
    @Scheduled(cron = "0 0 0,2 1 * ?")
    public void test(){
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd / HH:mm:ss");
        System.out.println("当前时间：\t" + simpleDateFormat.format(new Date()));    // 打印 当前日期时间
    }
}
~~~


# 定时任务的cron表达式

## cron表达式介绍
<font size=4>cron = 0 0 2 * * ?，这个表达式的含义是：每天2点执行一次任务。</font>

<font size=4>cron表达式是一个字符串，以5或者6个空格隔开（上面示例是被5个空格隔开）</font>

<font size=4>字符串被切割成 **6个或者7个域**，每个域都代表不同的含义</font>

<font size=4>从左到右依次分为：秒、分、时、日、月、周几、年（其中年不是必须的，所以cron表达式有两种形式）</font>

>{Seconds} {Minutes} {Hours} {DayofMonth} {Month} {DayofWeek} {Year}

<font size=4>或者</font>

>{Seconds} {Minutes} {Hours} {DayofMonth} {Month} {DayofWeek}

- <font size=4>各个域的含义:</font>
  ![](https://pic1.zhimg.com/v2-86ff6b9d188ae417753a0825d4f19e0c_r.jpg)


<font size=4>每个域都可以用数字表示，但是还可以出现如下特殊字符:</font>

  - <font size=4>* : 表示匹配该域的任意值。比如Minutes域使用*，就表示每分钟都会触发</font>
  - <font size=4>- : 表示范围。比如Minutes域使用 10-20，就表示从10分钟到20分钟每分钟都会触发一次</font>
  - <font size=4>, : 表示列出枚举值。比如Minutes域使用1,3，就表示1分钟和3分钟都会触发一次</font>
  - <font size=4>/ : 表示间隔时间触发(开始时间/时间间隔)。例如在Minutes域使用 5/10，就表示从第5分钟开始，每隔10分钟触发一次</font>
  - <font size=4>? : 表示不指定值。简单理解就是忽略该字段的值，直接根据另一个字段的值触发执行</font>
  - <font size=4># : 表示该月第n个星期x(x#n)，仅用星期域。如：星期：6#3，表示该月的第三个星期五</font>
  - <font size=4>L : 表示最后，是单词"last"的缩写（最后一天或最后一个星期几）；仅出现在日和星期的域中。用在日则表示该月的最后一天，用在星期则表示该月的最后一个星期。如：星期域上的值为5L，则表示该月最后一个星期的星期四。在使用'L'时，不要指定列表','或范围'-'，否则易导致出现意料之外的结果</font>
  - <font size=4>W: 仅用在日的域中，表示距离当月给定日期最近的工作日（周一到周五），是单词"weekday"的缩写</font>
  - <font size=4>**LW: 'L'和'W'可以一起组合在日字段使用。表示当月的最后一个工作日触发事件**</font>


<font size=4>比如："4W" 表示距离4号最近的工作日（当月的）触发</font>
  
  - <font size=4>当4号就是工作日时，则表示当天触发；当4号为周六时，则表示3号（周五）触发</font>
  - <font size=4>当4号为周日时，则表示在5号（周一）触发</font>


<font size=4>比如："1W" 表示距离1号最近的工作日触发，但是，该工作日只算当月的</font>
  
  - <font size=4>当月1号是周六，则"1W"表示在当月3号（周一）触发。就算上个月的最后一天是工作日，也不会触发</font>


## 取值说明

- <font size=4>**DayofMonth：**</font>
  - <font size=4>可以用数字1-31 中的任一个值，但要注意一些特别的月份</font>
  
- <font size=4>**Month：**</font>
  - <font size=4>一年中的月份，可以用 1-12 或用字符串 **"JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV and DEC"** 表示</font>
  
- <font size=4>**DayofWeek：**</font>
  - <font size=4>表示星期几，可以用数字 1-7**（1 = 星期日）**，或者用字符串 **"SUN, MON, TUE, WED, THU, FRI and SAT"** 来表示</font>


## 常用cron表达式

- <font size=4> \*/10 * * * * ?     每隔10秒执行一次</font>
- <font size=4> 0 \*/5 * * * ?      每隔5分钟执行一次</font>
- <font size=4> 0 2,22,32 * * * ?   每小时的 2分，22分，32分 执行一次</font>
- <font size=4> 0 0 4-8 * * ?       每日的 4-8点 每小时**整点**执行一次</font>
- <font size=4> 0 0 2 * * ?         每日的 2点 执行一次</font>
- <font size=4> 0 0 2 1 * ?         每月 1号2点 执行一次</font>


## cron表达式生成器

- [Cron表达式生成器](https://www.bejson.com/othertools/cron/)
- [在线Cron表达式生成器](https://cron.qqe2.com/)
- [Crontab表达式生成器](https://www.toolzl.com/tools/croncreate.html)


参考链接

- [SpringBoot定时任务@EnableScheduling](https://www.jianshu.com/p/9d18039c0f08)
- [Spring @EnableScheduling 注解解析](https://blog.csdn.net/sinat_32023305/article/details/81281282)
- [定时任务的cron表达式](https://zhuanlan.zhihu.com/p/163050320)
- [XXL-JOB的使用(详细教程)](https://blog.csdn.net/f2315895270/article/details/104714692)
- [3千字带你搞懂XXL-JOB任务调度平台](https://developer.aliyun.com/article/775305?utm_content=g_1000191671#slide-0)


