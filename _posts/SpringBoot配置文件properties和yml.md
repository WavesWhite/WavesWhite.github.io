---
title: SpringBoot配置文件——.properties文件和.yml文件
top_img: https://w.wallhaven.cc/full/zy/wallhaven-zyxvqy.jpg
cover: https://w.wallhaven.cc/full/zy/wallhaven-zygeko.jpg
abbrlink: SpringBoot配置文件——.properties文件和.yml文件
tags:
    - JAVA
    - SpringBoot
categories:
    - 杂七杂八

date: 2022/10/27 22:16:21 
---
<font size=4>  </font> 

<font size=4>

</font> 

# 前言：配置文件的作用
<font size=4>
通常情况下，一个项目中的一些重要信息都会放在配置文件中，比如：**数据库的连接信息**、**项目的启动端口**、一些**第三方的秘钥**、**记录信息的普通日志**和**异常日志**等。
</font> 

# SpringBoot的配置文件 （.properties 和 .yml）
<font size=4>
Spring Boot的配置文件主要有两种格式：.properties文件和.yml文件
</font> 
![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-27-23-05-16.png)

<font size=4>
这两种格式的配置文件可以共同存在于一个项目中，但一般情况下我们会统一格式，只使用其中的一种，来降低故障率。
</font> 

<font size=4>
.properties是最早期的SpringBoot配置文件的格式，也是现在默认的配置文件格式，出现的时间早于.yml
</font> 

<font size=4>
**当这两个配置文件中出现相同的配置时，会优先执行.properties中的配置，然后再执行.yml中的配置，即.properties的优先级大于.yml**
</font> 

- <font size=4>**properties**</font> 
  
  <font size=4>
  创建SpringBoot项目后会自动生成一个配置文件，在该文件中，信息以键值对的形式进行储存
  </font> 
  ~~~properties
  # 配置项目的信息
  # key=value
  server.port=8080
  spring.datasource.url=jdbc:mysql://127.0.0.1:3306/testdb?
  characterEncoding=utf8
  spring.datasource.username=root
  spring.datasource.password=root
  ~~~
  - <font size=4>缺点</font> 
  
    <font size=4>对于同一个对象的多个属性进行设置时很麻烦，需要多次重复，存在冗余配置项；当我们想要设置同一个对象很多的信息时，需要多次重复这个信息：</font> 
    ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-27-23-17-55.png)


- <font size=4>**yml**</font> 
  
  <font size=4>
  yml是YMAL的缩写，全称是 Yet Another Markup Language，另一种**标记语言**
  </font> 

  <font size=4>
  YMAL是一个可读性高、易于理解、用来表达数据序列化的格式。
  </font> 

  <font size=4>
  YMAL的作用是可以做到跨语言使用，不仅Java中可以使用，Go和python中都可以使用
  </font> 

  - <font size=4>语法格式：</font> 
    - <font size=4>**key: value** 表示键值对关系，冒号后面必须加一个空格</font> 
    - <font size=4>大小写敏感</font> 
    - <font size=4>缩进时不允许使用tab键，只能用空格</font> 
    - <font size=4>松散表示，java使用驼峰命名，而用原名或者-代替都可以正确映射，比如java中的名称是lastName，yml中使用lastName或者last-name都可以映射到</font> 

  - <font size=4>键值关系</font> 
  ~~~yml
  # 通常写法
  student:
    id: 1
    name: 张三
    age: 20

  # 另一种写法
  student: { id: 1, name: 张三, age: 20}
  ~~~

  <font size=4>
   yml中，字符串默认不需要加上单引号或者双引号
  </font> 

  <font size=4>
  **使用双引号**：不会转义字符串中的特殊字符，特殊字符会作为本身想要表达的意思：比如 name: “zhangsan \n 23”,会输出：zhangsan 换行 23 (下面使用**@ConfigurationProperties**注解演示)
  </font> 
  ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-03-17.png)
  ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-03-51.png)
    
  <font size=4>
  **使用单引号**：会转义特殊字符，特殊字符失去特殊作用变成普通字符
  </font> 
  ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-04-41.png)
  <font size=4>
  **使用YML连接数据库：**
  </font> 
  ~~~yml
  spring:
  datasource:
    url: jdbc:mysql://localhost:3306/bailang_blog?characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
  ~~~
  <font size=4>
  表示常用的字面量：
  </font> 
  ~~~yml
  # 普通字面量
  name: zhangsan
  age: 18

  # 布尔类型
  flag: true

  # 日期
  date: 2022/10/27

  # 同一个对象的多个属性
  people:
        name: lisi
        age: 20
  # 数组
  # 用 -表示数组中的一个元素
  pets:
    - cat
    - dog
    - pig
  # 另一种写法
  petss: [dog,cat,pig]

  # list对象、set对象、数组对象
  students:
        - name: zhangsan
          age: 22
        - name: lisi
          age: 20
        - {name: wangwu,age: 18}
  ~~~

# 读取配置文件的方法

## @Value注解读取单个配置项

<font size=4>
如果我们想要主动的读取配置文件中的信息，可以使用注解**@Value**，使用格式：
</font> 
~~~java
@Value("${server.port}")
private String port;
~~~
<font size=4>
在属性前加上@Value注解，后面的括号写入要读取的配置中哪个key，比如读取下面的server.port，注意格式：双引号+${}
</font>
~~~yml
# 配置项目端口
server:
  port: 7777
~~~
![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-10-48.png)

## @ConfigurationProperties读取配置信息

- <font size=4>读取对应前缀的后面的属性</font> 
  ~~~java
  @Data
  // 将类定义为一个bean的注解，比如 @Component,@Service,@Controller,@Repository
  // 或者 @Configuration
  @Component
  // 表示使用配置文件中前缀为datatest的属性的值初始化该bean定义产生的的bean实例的同名属性
  // 在使用时这个定义产生的bean时，其属性test会是：这是另一个测试属性
  @ConfigurationProperties(prefix = "datatest")
  public class ConfigurationPropertiesTest {
    private String test;
  }
  ~~~
  <font size=4>对应application.yml配置文件内容如下：</font>
  ~~~yml
  datatest:
    test: 这是另一个测试属性
  ~~~
  ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-16-01.png)
  <font size=4>读取配置属性的 **list**</font>
  ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-19-38.png)

- <font size=4>读取实体类 （将配置文件的一组对象映射到实体类上）</font>
  <font size=4>使用格式：</font>
  ~~~java
  @Component //spring 启动时直接将配置文件映射到当前类属性上
  @ConfigurationProperties("test") // test是在配置好的key
  ~~~
  <font size=4>在yml配置文件中，我们创建一个student对象：</font>
  ~~~yml
  # 配置信息
  test:
    name: WhiteWaves
    id: 33
    app:
      field: This is Field
      count: 99
      users:
        - JBDC
        - Spring
        - Boot
  ~~~
  <font size=4>将这个对象映射到User类中，也就是从配置文件里读取到类中</font>
  ~~~java
  @Data
  @Component
  @ConfigurationProperties("test")
  public class User{

      private String name;

      private Integer id;

      private App app = new App();

      @Data
      public static class App{
          private String field;
          private Integer count;
          private List<String> users = new ArrayList<>();
      }

  }
  ~~~
  <font size=4>**使用注入的方式将对象注入到一个类中**</font>
  ~~~java
  @SpringBootTest
  @Data
  public class ConfigurationPropertiesTest {

      @Autowired // 属性注入
      private User user;

      @Test
      public void test(){
          System.out.println(this);
      }
  }
  ~~~
  ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-27-54.png)
  <font size=4>注意包路径配置的问题！！！**启动类和要注入的类包必须放在同级目录下，否则读取不到，会显示启动失败Application**</font>
  ![](../../../img/posts_img/SpringBoot配置文件properties和yml/2022-10-28-17-29-19.png)

# 总结
- <font size=4>yml语法更简洁，可以解决数据冗余问题</font>
- <font size=4>yml跨语言的通用性更好，它不支持java语言还支持golang 和python</font>
- <font size=4>yml 支持更多的数据类型</font>
- <font size=4>yml格式的配置文件在写的时候更容易出错（冒号后面需要加一个空格），而properties虽然写法更复杂但是不容易出错</font>
- <font size=4>yml虽然可以和properties共存，但一个项目中最好统一格式，只用其中的一个</font>


参考链接

- [SpringBoot~配置文件properties和yml.](https://blog.csdn.net/Merciful_Lion/article/details/124149012)
