---
title: Unsatisfied dependency expressed through field 'baseMapper'解决方法
top_img: https://w.wallhaven.cc/full/6o/wallhaven-6ox89q.png
cover: https://w.wallhaven.cc/full/e7/wallhaven-e7gvpo.png
abbrlink: Unsatisfied dependency expressed through field 'baseMapper'解决方法
tags:
    - JAVA
    - SpringBoot
    - Maven
categories:
    - 杂七杂八

date: 2022/8/9 18:15:13
---
# 问题

<font size=4>项目报异常：</font>

```
Unsatisfied dependency expressed through field 'articleService';
Unsatisfied dependency expressed through field 'baseMapper';
```

# 解决方法

<font size=5>应该是Mapper没有被扫描到</font>

- <font size=4>方法一：添加@MapperScan("xxx.xxx.xxx")注解, xxx.xxx.xxx就是你Mapper所在的包</font>

    ```java
    @SpringBootApplication
    @MapperScan("org.bailang.mapper")
    public class BaiLangBlogApplication {
        public static void main(String[] args) {
            SpringApplication.run(BaiLangBlogApplication.class, args);
        }
    }
    ```

- <font size=4>方法二：在mapper接口上面添加@Mapper注解</font>

    ```java
    @Mapper
    public interface ArticleMapper extends BaseMapper<Article> {

    }
    ```

- <font size=4>**重新编译项目，编译成功**</font>

参考链接

- [https://blog.csdn.net/zjwl199802/article/details/103713864/](https://blog.csdn.net/zjwl199802/article/details/103713864/)