---
title: 一个完整的SpringBoot项目所需要导入的依赖与设置 与 SpringBoot项目中pom.xml文件各标签详解（持续更新中）
top_img: https://w.wallhaven.cc/full/pk/wallhaven-pkqj3p.jpg
cover: https://w.wallhaven.cc/full/yx/wallhaven-yxm8g7.jpg
abbrlink: 一个完整的SpringBoot项目所需要导入的依赖与设置 与 SpringBoot项目中pom.xml文件各标签详解（持续更新中）
    - JAVA
    - SpringBoot
    - Maven
categories:
    - 杂七杂八

date: 2022/11/15 14:25:54
---
<font size=4></font>

# pom.xml文件各标签详解
<font size=4>Maven项目中有一个最核心的文件：**pom.xml** 文件</font>

<font size=4>pom.xml文件是Maven项目中的核心项目管理文件，主要用于描述项目的：项目描述、配置文件、依赖管理、构建信息管理、组织信息管理和licenses、开发者需要遵循的规则、缺陷管理系统、项目的url、项目的依赖性，以及其他所有的项目相关因素。pom.xml 文件中包含了许多标签，下来介绍一些pom.xml文件常用的标签</font>
~~~xml
<?xml version="1.0" encoding="UTF-8"?>  <!--    （xml的version与encoding声明必须放在第一行，否则会报错）-->
<!--    xml的版本和编码-->
<!--    xml 可扩展标记语言（EXtensible Markup Language）-->
<!--    xml 被设计用来传输和存储数据  html  被设计用来显示数据-->

<!--    xmlns-命名空间，类似包名，因为xml的标签可以自定义，所以需要命名空间来区分-->
<!--    xmlns:xsi-xml遵循的标签规范-->
<!--    xsi:schemaLocation-用来定义xmlschema的地址，也就是xml书写时需要遵循的语法-->
<!--    xsi:schemaLocation由于两部分组成：前面部分就是命名空间的名字，后面是xsd（xmlschema）的地址-->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!--pom作为项目对象模型。通过xml表示maven项目，使用pom.xml来实现
    主要描述了项目的：配置文件，开发者需要遵循的规则，缺陷管理系统，组织和licenses，项目的url，项目的依赖性，以及其他所有的项目相关因素-->

    <!--pom模型的版本-->
    <modelVersion>4.0.0</modelVersion>

    <!--项目的信息-->

    <!--groupId是项目的唯一标识，也是项目组织的唯一标识符：对应JAVA的包的结构，是main目录里java的目录结构-->
    <groupId>org.bailang</groupId>

    <!--artifactId是项目的唯一标志：项目名称-->
    <artifactId>SpringBoot-dependency-test</artifactId>

    <!--version是项目的版本号-->
    <version>1.0-SNAPSHOT</version>

    <!--项目打包的机制  默认为jar-->
    <packaging>jar</packaging>

    <!--项目的名称,Maven产生的文档用-->
    <name>SpringBoot-dependency-test</name>

    <!--项目的描述,Maven产生的文档用-->
    <description>This is a SpringBootDependencyTest</description>

    <!--父级项目，此项目继承的项目，parent的引用是固定的
    当我们创建一个 Spring Boot 工程时，可以继承自一个 spring-boot-starter-parent
    父项目中定义了依赖的版本，我们继承了它，所以依赖 dependence 可以不写版本号
    若不用父项目中的版本号则 自己用<version>标签指定-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.0</version>
        <relativePath/> <!-- lookup parent from repository（从存储库中查找父级项目） -->
    </parent>

    <!--项目的属性配置-->
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <!--编码格式-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--JDK的版本-->
        <java.version>1.8</java.version>
    </properties>

    <!--<groupId>-主要用来唯一标识一个项目或者一组项目,通常是java包名的全称,例如上面的：org.bailang-->
    <!--<artifactId>-用来标识同一groupId下不同的项目,例如：spring-boot-starter-thymeleaf,都是这种格式的-->
    <!--<version>-用来标识一个artifact的版本,格式有：8.0.21  0.0.1-SNAPSHOT  1.0-SNAPSHOT 等-->
    <!--<scope>-用来表示当前的这个依赖（通过pom加载进来的包）所作用的场景,就是说应该把它添加到哪个环境当中,例如：只在测试时此jar包生效
            取值主要有compile（编译范围）（若未指定则为该默认值）、provided（已提供范围）、 runtime（运行时范围）、test（测试范围）、system（系统范围）等-->
    <!--<optional>-标记依赖是否可以传递，默认值是false,可以用来减少项目之间jar包的冲突-->

    <!--项目的依赖关系-->
    <dependencies>

        <!--springboot 启动类依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

    </dependencies>


    <!--构建设置，主要包含两个部分：plugins设置构建的插件 和 resources排除或者删除资源文件-->
    <!--<configuration>-指定插件配置-->
    <!--<excludes>-指定哪些文件将被忽略-->
    <!--<plugins>-用于指定使用的插件-->
    <build>
        <!--使用的插件列表，此处直接用父项目的plugins-->
        <plugins>
            <!--打包跳过测试-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>

            <!--指定项目源码的 jdk 版本，编译后的 jdk 版本，以及编码-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
~~~


# 项目依赖导入与设置

## springboot 启动类依赖
~~~xml
<!--springboot 启动类依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
~~~

## springboot 测试类依赖
~~~xml
<!--springboot 测试类依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
</dependency>
~~~

## springboot Web开发依赖（包括Tomcat和spring-webmvc）
~~~xml
<!--springboot Web开发依赖，包括Tomcat和spring-webmvc-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

## Lombok依赖（用来简化对类的操作包括：set方法、get方法以及构造函数等，只需要一个注解）
~~~xml
<!--Lombok依赖-->
<!--导入lombok依赖后还需要进行一步操作，下载lombok插件，方法：点击File—>Setting—>Plugins
然后再搜索框搜索Lombok，安装插件即可-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
~~~

## Mybatis-Plus依赖
~~~xml
<!--Mybatis-Plus依赖-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
~~~

### application.yml文件 配置Mybatis-Plus
~~~yml
mybatis-plus:
  configuration:
    # mybatis-plus日志 控制台输出
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      # 逻辑删除
      logic-delete-field: delFlag
      logic-delete-value: 1
      logic-not-delete-value: 0
      # id生成规则：数据库id自增
      id-type: auto
~~~

## MySql依赖
~~~xml
<!--MySql依赖-->
<!--导入了mysql依赖后需要连接数据库
在application.yml配置文件中配置连入数据库的参数：
url：自己数据库的地址 （例如：jdbc:mysql://数据库IP地址:数据库端口号/mybatis-plus-crud-test?characterEncoding=utf-8&serverTimezone=Asia/Shanghai）
数据库名字：mybatis-plus-crud-test
driver：com.mysql.cj.jdbc.Driver
username：数据库用户名
password：数据库密码-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
~~~

### application.yml文件 配置数据库
~~~yml
spring:
  # 数据库配置
  datasource:
    url: jdbc:mysql://localhost:3306/mybatis-plus-crud-test?characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: 数据库用户名
    password: 数据库密码
    driver-class-name: com.mysql.cj.jdbc.Driver
~~~

## redis依赖
~~~xml
<!--redis依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

## fastjson依赖（可以将 Java对象 转换为 JSON格式，当然它也可以将 JSON字符串 转换为 Java对象）
~~~xml
<!--fastjson依赖-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.33</version>
</dependency>
~~~

## jwt依赖
~~~xml
<!--jwt依赖-->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.0</version>
</dependency>
~~~

## springboot devtools热部署依赖
~~~xml
<!--springboot devtools 的主要作用是热部署，引入devtools，这样每次在保存代码的时候都会自动重新加载-->
<!--所谓热部署,就是在应用正在运行的时候升级软件,却不需要重新启动应用,在运行时更新Java类文件-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
    <scope>runtime</scope>
</dependency>
~~~

## SpringSecurity启动器
~~~xml
<!--SpringSecurity启动器-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
~~~

## 打包跳过测试 插件
~~~xml
<!--打包跳过测试-->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <skip>true</skip>
    </configuration>
</plugin>
~~~

## 指定项目源码的 jdk 版本，编译后的 jdk 版本，以及编码 插件
~~~xml
<!--指定项目源码的 jdk 版本，编译后的 jdk 版本，以及编码-->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <source>${java.version}</source>
        <target>${java.version}</target>
        <encoding>${project.build.sourceEncoding}</encoding>
    </configuration>
</plugin>
~~~

## swagger依赖
~~~xml
<!--swagger依赖-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.swagger</groupId>
    <artifactId>swagger-models</artifactId>
    <version>1.5.21</version>
</dependency>
~~~

## 完整 pom.xml文件
~~~xml
<?xml version="1.0" encoding="UTF-8"?>  <!--    （xml的version与encoding声明必须放在第一行，否则会报错）-->
<!--    xml的版本和编码-->
<!--    xml 可扩展标记语言（EXtensible Markup Language）-->
<!--    xml 被设计用来传输和存储数据  html  被设计用来显示数据-->

<!--    xmlns-命名空间，类似包名，因为xml的标签可以自定义，所以需要命名空间来区分-->
<!--    xmlns:xsi-xml遵循的标签规范-->
<!--    xsi:schemaLocation-用来定义xmlschema的地址，也就是xml书写时需要遵循的语法-->
<!--    xsi:schemaLocation由于两部分组成：前面部分就是命名空间的名字，后面是xsd（xmlschema）的地址-->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!--pom作为项目对象模型。通过xml表示maven项目，使用pom.xml来实现
    主要描述了项目的：配置文件，开发者需要遵循的规则，缺陷管理系统，组织和licenses，项目的url，项目的依赖性，以及其他所有的项目相关因素-->

    <!--pom模型的版本-->
    <modelVersion>4.0.0</modelVersion>

    <!--父级项目，此项目继承的项目，parent的引用是固定的
    当我们创建一个 Spring Boot 工程时，可以继承自一个 spring-boot-starter-parent
    父项目中定义了依赖的版本，我们继承了它，所以依赖 dependence 可以不写版本号
    若不用父项目中的版本号则 自己用<version>标签指定-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.0</version>
        <relativePath/> <!-- lookup parent from repository（从存储库中查找父级项目） -->
    </parent>

    <!--项目的信息-->

    <!--groupId是项目的唯一标识，也是项目组织的唯一标识符：对应JAVA的包的结构，是main目录里java的目录结构-->
    <groupId>org.bailang</groupId>
    <!--artifactId是项目的唯一标志：项目名称-->
    <artifactId>SpringBoot-dependency-test</artifactId>
    <!--version是项目的版本号-->
    <version>1.0-SNAPSHOT</version>
    <!--项目打包的机制  默认为jar-->
    <packaging>jar</packaging>
    <!--项目的名称,Maven产生的文档用-->
    <name>SpringBoot-dependency-test</name>
    <!--项目的描述,Maven产生的文档用-->
    <description>This is a SpringBootDependencyTest</description>

    <!--项目的属性配置-->
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <!--编码格式-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--JDK的版本-->
        <java.version>1.8</java.version>
    </properties>

    <!--<groupId>-主要用来唯一标识一个项目或者一组项目,通常是java包名的全称,例如上面的：org.bailang-->
    <!--<artifactId>-用来标识同一groupId下不同的项目,例如：spring-boot-starter-thymeleaf,都是这种格式的-->
    <!--<version>-用来标识一个artifact的版本,格式有：8.0.21  0.0.1-SNAPSHOT  1.0-SNAPSHOT 等-->
    <!--<scope>-用来表示当前的这个依赖（通过pom加载进来的包）所作用的场景,就是说应该把它添加到哪个环境当中,例如：只在测试时此jar包生效
            取值主要有compile（编译范围）（若未指定则为该默认值）、provided（已提供范围）、 runtime（运行时范围）、test（测试范围）、system（系统范围）等-->
    <!--<optional>-标记依赖是否可以传递，默认值是false,可以用来减少项目之间jar包的冲突-->

    <!--项目的依赖关系-->
    <dependencies>
        <!--springboot 启动类依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <!--springboot 测试类依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>

        <!--springboot Web开发依赖，包括Tomcat和spring-webmvc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--Lombok依赖-->
        <!--导入lombok依赖后还需要进行一步操作，下载lombok插件，方法：点击File—>Setting—>Plugins
        然后再搜索框搜索Lombok，安装插件即可-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <!--Mybatis-Plus依赖-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.3</version>
        </dependency>

        <!--MySql依赖-->
        <!--导入了mysql依赖后需要连接数据库
        在application.yml配置文件中配置连入数据库的参数：
        url：自己数据库的地址 （例如：jdbc:mysql://数据库IP地址:数据库端口号/mybatis-plus-crud-test?characterEncoding=utf-8&serverTimezone=Asia/Shanghai）
        数据库名字：mybatis-plus-crud-test
        driver：com.mysql.cj.jdbc.Driver
        username：数据库用户名
        password：数据库密码-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!--redis依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <!--fastjson依赖-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.33</version>
        </dependency>

        <!--jwt依赖-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.0</version>
        </dependency>

        <!--springboot devtools 的主要作用是热部署，引入devtools，这样每次在保存代码的时候都会自动重新加载-->
        <!--所谓热部署,就是在应用正在运行的时候升级软件,却不需要重新启动应用,在运行时更新Java类文件-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
            <scope>runtime</scope>
        </dependency>

        <!--SpringSecurity启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <!--swagger依赖-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.swagger</groupId>
            <artifactId>swagger-models</artifactId>
            <version>1.5.21</version>
        </dependency>

    </dependencies>


    <!--构建设置，主要包含两个部分：plugins设置构建的插件 和 resources排除或者删除资源文件-->
    <!--<configuration>-指定插件配置-->
    <!--<excludes>-指定哪些文件将被忽略-->
    <!--<plugins>-用于指定使用的插件-->
    <build>
        <!--使用的插件列表，此处直接用父项目的plugins-->
        <plugins>
            <!--打包跳过测试-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>

            <!--指定项目源码的 jdk 版本，编译后的 jdk 版本，以及编码-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
~~~

## 完整 application.yml文件
~~~yml
# 服务端口（不设置默认：8080）
server:
  port: 8070

spring:
  # 数据库配置
  datasource:
    url: jdbc:mysql://localhost:3306/mybatis-plus-crud-test?characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: 数据库用户名
    password: 数据库密码
    driver-class-name: com.mysql.cj.jdbc.Driver
  servlet:
    # 文件上传大小限制
    multipart:
      max-file-size: 2MB
      max-request-size: 5MB
  # redis配置      
  redis:
    host: redis的地址
    port: redis的端口（redis默认端口：6379）

mybatis-plus:
  configuration:
    # mybatis-plus日志 控制台输出
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      # 逻辑删除
      logic-delete-field: delFlag
      logic-delete-value: 1
      logic-not-delete-value: 0
      # id生成规则：数据库id自增
      id-type: auto

# 七牛云上传凭证 配置
oss:
  accessKey: 七牛云的上传凭证 access key
  secretKey: 七牛云的上传凭证 secret key
  bucket: 存储数据使用的仓库名 bucket name
  qiniu_domain: 自定义域名
~~~


# 项目细节配置

## 设置主键自动增长
- <font size=4>创建数据表的时候**设置主键自增**</font>
- <font size=4>实体字段中配置主键字段 增加注解@TableId(value = "id",type = IdType.AUTO) （如果上面application.yml文件中的mybatis-plus配置设置了id自增，则实体类可以不用设置字段自动增长，mybatis-plus自带雪花算法）</font>

~~~java
@SuppressWarnings("serial")
@TableName("user")  // 标识表名
public class User  {
    @TableId // 标识主键ID
    //实体类可以不用设置字段自动增长，mybatis-plus自带雪花算法
    private Long id;
}
~~~

## 设置新增、更新自动填充功能（MP字段自动填充）
<font size=4>在实体类中，对需要进行自动填充的字段添加注解</font>

~~~java
@SuppressWarnings("serial")
@Data
@AllArgsConstructor
@NoArgsConstructor
@TableName("user")  // 标识表名
public class User  {
    @TableField(fill = FieldFill.INSERT)
    //创建人的用户ID
    private Long createBy;
    @TableField(fill = FieldFill.INSERT)
    //创建时间
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    //更新人的用户ID
    private Long updateBy;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    //更新时间
    private Date updateTime;
}
~~~

<font size=4>自定义类MyMetaObjectHandler，实现 MetaObjectHandler 接口，重写相应方法即刻</font>

~~~java
/**
 * 配置MP字段自动填充：实现 MetaObjectHandler 接口，重写相应方法即刻
 *
 * 如需实现字段自动填充，请给需要填充的字段添加注解，例：@TableField(fill = FieldFill.INSERT_UPDATE) 、 @TableField(fill = FieldFill.INSERT)
 */
@Component
public class MyMetaObjectHandler implements MetaObjectHandler { // 配置MP字段自动填充：实现 MetaObjectHandler 接口，重写相应方法即刻

    /**
     * 新增数据自动填充方法
     * @param metaObject
     */
    @Override
    public void insertFill(MetaObject metaObject) {
        System.out.println("进入MP自动填充 新增数据");
        this.setFieldValByName("createBy", (long)3, metaObject);
        this.setFieldValByName("createTime", new Date(), metaObject);
        this.setFieldValByName("updateBy", (long)3, metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }

    /**
     * 更新数据自动填充方法
     * @param metaObject
     */
    @Override
    public void updateFill(MetaObject metaObject) {
        System.out.println("进入MP自动填充 更新数据");
        this.setFieldValByName("updateBy", (long)3, metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
~~~

## 配置逻辑删除（实体类）
- <font size=4>数据库中的**逻辑删除字段** 要与 application.yml中配置的逻辑删除字段一致</font>

- <font size=4>在实体类中的**逻辑删除字段**添加注解 @TableLogic</font>

~~~java
@SuppressWarnings("serial")
@Data
@AllArgsConstructor
@NoArgsConstructor
@TableName("user")  // 标识表名
public class User  {
    //删除标志（0代表未删除，1代表已删除）
    @TableLogic // 逻辑删除 注释
    private Integer delFlag;
}
~~~


## 配置分页查询功能
~~~java
@Configuration
public class MybatisPlusConfig {
    // MP支持 分页配置
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        // 添加分页插件
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mybatisPlusInterceptor;
    }
}
~~~

## 自定义 统一响应类与响应code枚举类

### 响应code枚举类
~~~java
public enum AppHttpCodeEnum {

    SUCCESS(200, "操作成功"),   // 成功

    NEED_LOGIN(401, "需要登陆后操作"),    // 登录
    NO_OPERATOR_AUTH(403, "无权限操作"),
    SYSTEM_ERROR(500, "出现错误"),
    REQUIRE_USERNAME(504, "必须填写用户名"),
    LOGIN_ERROR(505, "用户名或密码错误"),
    CONTENT_NOT_NULL(506, "内容不能为空"),
    FILE_TYPE_ERROR(507, "文件格式错误，请上传正确格式的文件"),
    FILE_TOO_LARGE(413, "上传失败，文件过大"),
    USERNAME_NOT_NULL(508, "用户名不能为空"),
    NICKNAME_NOT_NULL(509, "昵称不能为空"),
    PASSWORD_NOT_NULL(510, "密码不能为空"),
    EMAIL_NOT_NULL(511, "邮箱不能为空"),
    USERNAME_EXIST(501, "用户名已存在"),
    PHONENUMBER_EXIST(502, "手机号已存在"),
    EMAIL_EXIST(503, "邮箱已存在"),
    NICKNAME_EXIST(512, "昵称已存在");


    private final int code;
    private final String msg;

    AppHttpCodeEnum(int code, String errorMsg) {
        this.code = code;
        this.msg = errorMsg;
    }

    public int getCode() {
        return code;
    }

    public String getMsg() {
        return msg;
    }
}
~~~

### 统一响应类
~~~java
@JsonInclude(JsonInclude.Include.NON_NULL)
public class ResponseResult<T> implements Serializable {
    private Integer code;
    private String msg;
    private T data;


    public ResponseResult() {
        this.code = AppHttpCodeEnum.SUCCESS.getCode();
        this.msg = AppHttpCodeEnum.SUCCESS.getMsg();
    }

    public ResponseResult(Integer code, T data) {
        this.code = code;
        this.data = data;
    }

    public ResponseResult(Integer code, String msg, T data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }

    public ResponseResult(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }


    public static ResponseResult okResult(){
        ResponseResult result = new ResponseResult();
        return result;
    }

    public static ResponseResult okResult(int code, String msg){
        ResponseResult result = new ResponseResult();
        return result.ok(code, msg, null);
    }

    public static ResponseResult okResult(Object data){
        ResponseResult result = setAppHttpCodeEnum(AppHttpCodeEnum.SUCCESS, AppHttpCodeEnum.SUCCESS.getMsg());
        if(data!=null){
            result.setData(data);
        }
        return result;
    }

    public static ResponseResult errorResult(int code, String msg){
        ResponseResult result = new ResponseResult();
        return result.error(code, msg);
    }

    public static ResponseResult errorResult(AppHttpCodeEnum enums){
        ResponseResult result = setAppHttpCodeEnum(enums, enums.getMsg());
        return result;
    }

    public static ResponseResult errorResult(AppHttpCodeEnum enums, String msg){
        ResponseResult result = setAppHttpCodeEnum(enums, msg);
        return result;
    }

    public static ResponseResult setAppHttpCodeEnum(AppHttpCodeEnum enums){
        return okResult(enums.getCode(), enums.getMsg());
    }

    public static ResponseResult setAppHttpCodeEnum(AppHttpCodeEnum enums, String msg){
        return okResult(enums.getCode(), msg);
    }


    public ResponseResult<?> error(Integer code, String msg){
        this.code = code;
        this.msg = msg;
        return this;
    }

    public ResponseResult<?> ok(Integer code, T data){
        this.code = code;
        this.data = data;
        return this;
    }

    public ResponseResult<?> ok(Integer code, String msg, T data){
        this.code = code;
        this.msg = msg;
        this.data = data;
        return this;
    }

    public ResponseResult<?> ok(T data){
        this.data = data;
        return this;
    }


    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

}
~~~


## 统一异常处理 与 自定义异常处理
<font size=4>需要在 handler处理包 下新建一个 GlobalExceptionHandler全局异常统一处理配置类，否侧出现异常前端不会直接提示**500错误代码与错误信息**</font>


### 自定义异常
~~~java
/**
 * 自定义系统异常
 */
public class SystemException extends RuntimeException{

    private int code;

    private String msg;

    public int getCode() {
        return code;
    }

    public String getMsg() {
        return msg;
    }

    public SystemException(AppHttpCodeEnum appHttpCodeEnum){
        super(appHttpCodeEnum.getMsg());

        this.code = appHttpCodeEnum.getCode();
        this.msg = appHttpCodeEnum.getMsg();
    }

    public SystemException(AppHttpCodeEnum appHttpCodeEnum, String msg){
        super(msg);

        this.code = appHttpCodeEnum.getCode();
        this.msg = msg;
    }

    public SystemException(int code, String msg){
        super(msg);

        this.code = code;
        this.msg = msg;
    }
}
~~~

### 全局异常统一处理配置类
~~~java
/**
 * 全局异常统一处理配置类
 *
 */

//@ControllerAdvice
//@ResponseBody
@RestControllerAdvice   // @ControllerAdvice+@ResponseBody 复合注解
@Slf4j  // 日志
public class GlobalExceptionHandler {   // 统一异常处理

    // 自定义异常处理
    @ExceptionHandler(SystemException.class)
    public ResponseResult systemExceptionHandler(SystemException e){
        System.out.println("进入系统异常: "+e.toString());

        // 打印异常信息
        log.error("出现了异常！ {}", e.toString(), e);
        // 从异常对象中获取信息封装result返回
        ResponseResult result = ResponseResult.errorResult(e.getCode(), e.getMsg());

        return result;
    }

    // 统一异常处理
    @ExceptionHandler(Exception.class)
    public ResponseResult exceptionHandler(Exception e){
        System.out.println("进入全局Exception异常: "+e.toString());

        // 打印异常信息
        log.error("出现了异常！ {}", e.toString(), e);
        // 从异常对象中获取信息封装result返回
        ResponseResult result = ResponseResult.errorResult(AppHttpCodeEnum.SYSTEM_ERROR, e.getMessage());

        return result;
    }
}
~~~


## 配置七牛云OSS服务
<font size=4>在 **application.yml文件** 中设置 **上传凭证 与 仓库名**（非必要），**上传代码和工具类的代码实现**参考下列的博客：</font>

- [<font size=4>七牛云文件上传代码模板（Java）</font>](https://heyzqf.com/articles/%E4%B8%83%E7%89%9B%E4%BA%91%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E4%BB%A3%E7%A0%81%E6%A8%A1%E6%9D%BF%EF%BC%88Java%EF%BC%89/)


## redis配置
<font size=4>导入依赖后，在 **application.yml文件** 中配置 **redis地址与端口信息**，工具类代码实现如下：</font>

~~~java
@SuppressWarnings(value = { "unchecked", "rawtypes" })
@Component
public class RedisCache {
    @Autowired
    public RedisTemplate redisTemplate;

    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     */
    public <T> void setCacheObject(final String key, final T value) {
        redisTemplate.opsForValue().set(key, value);
    }

    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     * @param timeout 时间
     * @param timeUnit 时间颗粒度
     */
    public <T> void setCacheObject(final String key, final T value, final Integer timeout, final TimeUnit timeUnit) {
        redisTemplate.opsForValue().set(key, value, timeout, timeUnit);
    }

    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout) {
        return expire(key, timeout, TimeUnit.SECONDS);
    }

    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @param unit 时间单位
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout, final TimeUnit unit) {
        return redisTemplate.expire(key, timeout, unit);
    }

    /**
     * 获得缓存的基本对象。
     *
     * @param key 缓存键值
     * @return 缓存键值对应的数据
     */
    public <T> T getCacheObject(final String key) {
        ValueOperations<String, T> operation = redisTemplate.opsForValue();
        return operation.get(key);
    }

    /**
     * 删除单个对象
     *
     * @param key
     */
    public boolean deleteObject(final String key) {
        return redisTemplate.delete(key);
    }

    /**
     * 删除集合对象
     *
     * @param collection 多个对象
     * @return
     */
    public long deleteObject(final Collection collection) {
        return redisTemplate.delete(collection);
    }

    /**
     * 缓存List数据
     *
     * @param key 缓存的键值
     * @param dataList 待缓存的List数据
     * @return 缓存的对象
     */
    public <T> long setCacheList(final String key, final List<T> dataList) {
        Long count = redisTemplate.opsForList().rightPushAll(key, dataList);
        return count == null ? 0 : count;
    }

    /**
     * 获得缓存的list对象
     *
     * @param key 缓存的键值
     * @return 缓存键值对应的数据
     */
    public <T> List<T> getCacheList(final String key) {
        return redisTemplate.opsForList().range(key, 0, -1);
    }

    /**
     * 缓存Set
     *
     * @param key 缓存键值
     * @param dataSet 缓存的数据
     * @return 缓存数据的对象
     */
    public <T> BoundSetOperations<String, T> setCacheSet(final String key, final Set<T> dataSet) {
        BoundSetOperations<String, T> setOperation = redisTemplate.boundSetOps(key);
        Iterator<T> it = dataSet.iterator();
        while (it.hasNext()) {
            setOperation.add(it.next());
        }
        return setOperation;
    }

    /**
     * 获得缓存的set
     *
     * @param key
     * @return
     */
    public <T> Set<T> getCacheSet(final String key) {
        return redisTemplate.opsForSet().members(key);
    }

    /**
     * 缓存Map
     *
     * @param key
     * @param dataMap
     */
    public <T> void setCacheMap(final String key, final Map<String, T> dataMap) {
        if (dataMap != null) {
            redisTemplate.opsForHash().putAll(key, dataMap);
        }
    }

    /**
     * 获得缓存的Map
     *
     * @param key
     * @return
     */
    public <T> Map<String, T> getCacheMap(final String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * 往Hash中存入数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @param value 值
     */
    public <T> void setCacheMapValue(final String key, final String hKey, final T value) {
        redisTemplate.opsForHash().put(key, hKey, value);
    }

    /**
     * 获取Hash中的数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @return Hash中的对象
     */
    public <T> T getCacheMapValue(final String key, final String hKey) {
        HashOperations<String, String, T> opsForHash = redisTemplate.opsForHash();
        return opsForHash.get(key, hKey);
    }

    /**
     * 删除Hash中的数据
     * 
     * @param key
     * @param hkey
     */
    public void delCacheMapValue(final String key, final String hkey) {
        HashOperations hashOperations = redisTemplate.opsForHash();
        hashOperations.delete(key, hkey);
    }

    /**
     * 获取多个Hash中的数据
     *
     * @param key Redis键
     * @param hKeys Hash键集合
     * @return Hash对象集合
     */
    public <T> List<T> getMultiCacheMapValue(final String key, final Collection<Object> hKeys) {
        return redisTemplate.opsForHash().multiGet(key, hKeys);
    }

    /**
     * 获得缓存的基本对象列表
     *
     * @param pattern 字符串前缀
     * @return 对象列表
     */
    public Collection<String> keys(final String pattern) {
        return redisTemplate.keys(pattern);
    }

    /**
     * 递增Hash中的数据
     *
     * @param key   Redis键
     * @param hKey  Hash键
     * @param value 递增的数值
     */
    public void incrementCacheMapValue(String key, String hKey, Long value) {
        redisTemplate.opsForHash().increment(key, hKey, value);
    }
}
~~~


## 配置日志记录
<font size=4>使用AOP实现日志记录，参考下面博客：</font>

- [<font size=4>SpringBoot使用AOP实现日志记录 + Slf4j 日志配置</font>](https://heyzqf.com/articles/SpringBoot%E4%BD%BF%E7%94%A8AOP%E5%AE%9E%E7%8E%B0%E6%97%A5%E5%BF%97%E8%AE%B0%E5%BD%95%20+%20Slf4j%20%E6%97%A5%E5%BF%97%E9%85%8D%E7%BD%AE/)


## 配置热部署与使用方法
<font size=4>参考以下转载博客：</font>

- [<font size=4>IDEA 2021.2版本 DevTools 热部署配置无效的解决方案</font>](https://blog.csdn.net/luokha/article/details/119746359#:~:text=%E5%A6%82%E6%9E%9C%E4%BD%BF%E7%94%A8%E7%9A%84%E6%98%AFIDEA,2021.2%E4%B9%8B%E5%89%8D%E7%89%88%E6%9C%AC%E7%9A%84%E8%AF%9D%E8%BF%98%E6%98%AF%E4%BD%BF%E7%94%A8%E5%BF%AB%E6%8D%B7%E9%94%AEshift%2BCtrl%2BAlt%2B%2F%EF%BC%8C%E9%80%89%E6%8B%A9Registry%E2%80%A6%EF%BC%8C%E5%B0%86compiler.automake.allow.when.app.running%E9%80%89%E9%A1%B9%E5%8B%BE%E4%B8%8A%E3%80%82)
- [<font size=4>Spring Boot 整合 DevTools（实现热部署）</font>](https://blog.csdn.net/cxy35/article/details/105363784)
- [<font size=4>使用Spring Boot Devtools实现热部署</font>](https://zhuanlan.zhihu.com/p/133233569)


## Swagger2配置

### 启用Swagger2
~~~java
@SpringBootApplication
@MapperScan("org.bailang.mapper")
@Slf4j
@EnableScheduling   // 开启定时任务
@EnableSwagger2     // 开启 Swagger2
public class BaiLangBlogApplication {
    public static void main(String[] args) {
        SpringApplication.run(BaiLangBlogApplication.class, args);
        System.out.println("\n" +
                "____    __    ____  __    __   __  .___________. ___________    __    ____  ___   ____    ____  _______     _______.\n" +
                "\\   \\  /  \\  /   / |  |  |  | |  | |           ||   ____\\   \\  /  \\  /   / /   \\  \\   \\  /   / |   ____|   /       |\n" +
                " \\   \\/    \\/   /  |  |__|  | |  | `---|  |----`|  |__   \\   \\/    \\/   / /  ^  \\  \\   \\/   /  |  |__     |   (----`\n" +
                "  \\            /   |   __   | |  |     |  |     |   __|   \\            / /  /_\\  \\  \\      /   |   __|     \\   \\    \n" +
                "   \\    /\\    /    |  |  |  | |  |     |  |     |  |____   \\    /\\    / /  _____  \\  \\    /    |  |____.----)   |   \n" +
                "    \\__/  \\__/     |__|  |__| |__|     |__|     |_______|   \\__/  \\__/ /__/     \\__\\  \\__/     |_______|_______/    \n" +
                "                                                                                                                    \n");
    }
}

// 测试查看文档：访问 http://localhost:7777/swagger-ui.html 注意其中localhost和7777要调整成实际项目的域名和端口号
~~~

### Controller配置、接口配置、接口参数配置 示例代码
~~~java
@RestController
@RequestMapping("/comment")
@Api(tags = "评论", description = "评论相关接口-CommentController")
public class CommentController {

    @Autowired
    private CommentService commentService;
    public static CommentService commentService_static;

    @PostConstruct
    public void init(){
        commentService_static = commentService;
    }


    @GetMapping("/commentList")
    @SystemLog(BusinessName = "查询文章评论列表")
    @ApiOperation(value = "文章评论列表", notes = "获取一页文章评论")
    @ApiImplicitParams(
            {
                    @ApiImplicitParam(name = "articleId", value = "文章ID", dataType = "long"),
                    @ApiImplicitParam(name = "pageNum", value = "页号", dataType = "int"),
                    @ApiImplicitParam(name = "pageSize", value = "每页大小", dataType = "int")
            }
    )
    public ResponseResult commentList(Long articleId, Integer pageNum, Integer pageSize){   // 查询文章评论列表

        ResponseResult result = commentService.commentList(SystemConstants.ARTICLE_COMMENT, articleId, pageNum, pageSize);

        return result;
    }

    @PostMapping
    @SystemLog(BusinessName = "发表评论")
    @ApiOperation(value = "发表评论", notes = "用户发表评论")
    @ApiImplicitParams(
            {
                    @ApiImplicitParam(name = "addCommentDto", value = "新增评论 Dto对象", dataType = "AddCommentDto")
            }
    )
    public ResponseResult addComment(@RequestBody AddCommentDto addCommentDto){ // 发表评论
        // 将 dto 转为 数据库映射实体类
        Comment comment = BeanCopyUtils.copyBean(addCommentDto, Comment.class);

        ResponseResult result = commentService.addComment(comment);

        return result;
    }

    @GetMapping("/linkCommentList")
    @SystemLog(BusinessName = "查询友联评论列表")
    @ApiOperation(value = "友联评论列表", notes = "获取一页友联评论")
    @ApiImplicitParams(
            {
                    @ApiImplicitParam(name = "pageNum", value = "页号", dataType = "int"),
                    @ApiImplicitParam(name = "pageSize", value = "每页大小", dataType = "int")
            }
    )
    public ResponseResult linkCommentList(Integer pageNum, Integer pageSize){   // 查询友联评论列表

        ResponseResult result = commentService.commentList(SystemConstants.LINK_COMMENT, null, pageNum, pageSize);

        return result;
    }
}
~~~

### 实体类配置、实体类属性配置 示例代码
~~~java
/**
 * 新增评论接口 数据传输对象
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
@ApiModel(description = "新增评论DTO")
public class AddCommentDto {
    // 标识主键ID
    @ApiModelProperty(notes = "标识主键ID")
    private Long id;
    //评论类型（0代表文章评论，1代表友链评论）
    @ApiModelProperty(notes = "评论类型（0代表文章评论，1代表友链评论）")
    private String type;
    //文章id
    @ApiModelProperty(notes = "文章id")
    private Long articleId;
    //根评论id
    @ApiModelProperty(notes = "根评论id")
    private Long rootId;
    //评论内容
    @ApiModelProperty(notes = "评论内容")
    private String content;
    //所回复的目标评论的userid
    @ApiModelProperty(notes = "所回复的目标评论的userid")
    private Long toCommentUserId;
    //回复目标评论id
    @ApiModelProperty(notes = "回复目标评论id")
    private Long toCommentId;

    private Long createBy;

    private Date createTime;

    private Long updateBy;

    private Date updateTime;
    //逻辑删除标志（0代表未删除，1代表已删除）
    @ApiModelProperty(notes = "逻辑删除标志（0代表未删除，1代表已删除）")
    private Integer delFlag;
}
~~~

### 文档信息配置
~~~java
/**
 * Swagger2文档信息配置
 */
@Configuration
public class SwaggerConfig {

    @Bean
    public Docket customDocket(){ // 自定义 Docket
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("org.bailang.controller"))
                .build();
    }

    private ApiInfo apiInfo(){  // 自定义 ApiInfo
        // Contact contact = new Contact("名字", "网站url地址", "邮箱");    // 联系方式
        Contact contact = new Contact("WhiteWaves", "https://heyzqf.com", "WhiteWaves@qq.com");
        return new ApiInfoBuilder()
                .title("测试 Swagger2 文档")        // 文档标题
                .description("这是一个测试Swagger2的文档")  // 文档描述
                .contact(contact)      // 联系方式
                .version("1.0.0")      // 版本
                .build();
    }
}
~~~

### swagger2 注解说明
<font size=4>参考以下转载博客：</font>

- [<font size=4>swagger2 注解说明</font>](https://blog.csdn.net/xiaojin21cen/article/details/78654652)


## Web工具类
~~~java
public class WebUtils {
    /**
     * 将字符串渲染到客户端
     *
     * @param response 渲染对象
     * @param string 待渲染的字符串
     * @return null
     */
    public static void renderString(HttpServletResponse response, String string) {
        try {
            response.setStatus(200); // 设置 HTTP 的状态码, 说明HTTP请求成功
            response.setContentType("application/json"); // 相应格式 json格式
            response.setCharacterEncoding("utf-8");
            response.getWriter().print(string);
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void setDownLoadHeader(String fileName, ServletContext context, HttpServletResponse response) throws UnsupportedEncodingException {

    }

    /**
     * 设置 下载Excel文件响应体信息
     *
     * @param fileName  文件名
     * @param response  响应体信息
     * @throws UnsupportedEncodingException
     */
    public static void setExcelDownLoadHeader(String fileName, HttpServletResponse response) throws UnsupportedEncodingException {
        // 设置ContentType响应头信息
        response.setContentType("application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
        // 设置编码格式
        response.setCharacterEncoding("utf-8");
        // 对文件名进行转码
        String fName = URLEncoder.encode(fileName, "UTF-8").replaceAll("\\+", "%20");
        // 设置Content-disposition响应头信息，filename下载文件名
        response.setHeader("Content-disposition","attachment; filename="+fName);
    }
}
~~~


## Jwt工具类
~~~java
/**
 * JWT工具类
 */
public class JwtUtil {

    //有效期为
    public static final Long JWT_TTL = 24*60 * 60 *1000L;// 60 * 60 *1000  一个小时
    //设置秘钥明文
    public static final String JWT_KEY = "bailang";

    public static String getUUID(){
        String token = UUID.randomUUID().toString().replaceAll("-", "");
        return token;
    }

    /**
     * 生成jwt
     * @param subject token中要存放的数据（json格式）
     * @return
     */
    public static String createJWT(String subject) {
        JwtBuilder builder = getJwtBuilder(subject, null, getUUID());// 设置过期时间
        return builder.compact();
    }

    /**
     * 生成jwt
     * @param subject token中要存放的数据（json格式）
     * @param ttlMillis token超时时间
     * @return
     */
    public static String createJWT(String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, getUUID());// 设置过期时间
        return builder.compact();
    }

    private static JwtBuilder getJwtBuilder(String subject, Long ttlMillis, String uuid) {
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
        SecretKey secretKey = generalKey();
        long nowMillis = System.currentTimeMillis();
        Date now = new Date(nowMillis);
        if(ttlMillis==null){
            ttlMillis=JwtUtil.JWT_TTL;
        }
        long expMillis = nowMillis + ttlMillis;
        Date expDate = new Date(expMillis);
        return Jwts.builder()
                .setId(uuid)              //唯一的ID
                .setSubject(subject)   // 主题  可以是JSON数据
                .setIssuer("bl")     // 签发者
                .setIssuedAt(now)      // 签发时间
                .signWith(signatureAlgorithm, secretKey) //使用HS256对称加密算法签名, 第二个参数为秘钥
                .setExpiration(expDate);
    }

    /**
     * 创建token
     * @param id
     * @param subject
     * @param ttlMillis
     * @return
     */
    public static String createJWT(String id, String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, id);// 设置过期时间
        return builder.compact();
    }

    /**
     * 生成加密后的秘钥 secretKey
     * @return
     */
    public static SecretKey generalKey() {
        byte[] encodedKey = Base64.getDecoder().decode(JwtUtil.JWT_KEY);
        SecretKey key = new SecretKeySpec(encodedKey, 0, encodedKey.length, "AES");
        return key;
    }

    /**
     * 解析
     *
     * @param jwt
     * @return
     * @throws Exception
     */
    public static Claims parseJWT(String jwt) throws Exception {
        SecretKey secretKey = generalKey();
        return Jwts.parser()
                .setSigningKey(secretKey)
                .parseClaimsJws(jwt)
                .getBody();
    }
}
~~~


## Bean拷贝工具类
~~~java
/**
 * Bean拷贝工具类
 */
public class BeanCopyUtils {

    private BeanCopyUtils() {
    }

    // 拷贝单个对象
    public static <V> V copyBean(Object source, Class<V> clazz){
        // 创建目标对象
        V result = null;
        try {
            result = clazz.newInstance();
            // 实现属性copy
            // org.springframework.beans.BeanUtils
            BeanUtils.copyProperties(source, result);
        } catch (Exception e) {
            e.printStackTrace();
        }
        // 返回结果
        return result;
    }

    // 拷贝对象列表
    public static <O, V> List<V> copyBeanList(List<O> source, Class<V> clazz){
        return source.stream()
                .map(t -> copyBean(t, clazz))
                .collect(Collectors.toList());

    }
}
~~~

## Security工具类
~~~java
public class SecurityUtils {

    /**
     * 判断是否管理员
     * Id为 1 的用户为管理员
     */
    public static Boolean isAdmin(){
        Long userId = getUserID();

        return userId!=null && userId==1L;
    }

    /**
     * 存入 用户信息
     */
    public static void setAuthentication(LoginUser loginUser){
        // 用户信息存入 SecurityContextHolder
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(loginUser, null, null);
        getContext().setAuthentication(authenticationToken);

        return ;
    }

    /**
     * 获取 用户ID
     * @return User ID
     */
    public static Long getUserID(){
        return getLoginUser().getUser().getId();
    }

    /**
     * 获取 用户信息
     * @return LoginUser
     */
    public static LoginUser getLoginUser(){
        return (LoginUser) getAuthentication().getPrincipal();
    }

    /**
     * 获取 Authentication
     *
     */
    public static Authentication getAuthentication(){
        return getContext().getAuthentication();
    }

    /**
     * 获取 SecurityContext
     *
     */
    public static SecurityContext getContext(){
        return SecurityContextHolder.getContext();
    }
}
~~~





参考链接

- [一个完整的springboot项目所需要导入的依赖合集（方便查找）](https://blog.csdn.net/qq_56728342/article/details/125417615)
- [SpringBoot的pom.xml之依赖版本管理](https://blog.csdn.net/weixin_53041251/article/details/122309150)
- [Spring Boot项目中pom.xml文件各标签详解](https://blog.csdn.net/m0_47744782/article/details/121780814)
- [spring boot 配置文件pom.xml详解](https://blog.csdn.net/qq_41931797/article/details/94560019)
- [超级详细的Maven教程（四）Maven核心配置文件：pom.xml详解](https://developer.aliyun.com/article/799936)
- [IDEA 2021.2版本 DevTools 热部署配置无效的解决方案](https://blog.csdn.net/luokha/article/details/119746359#:~:text=%E5%A6%82%E6%9E%9C%E4%BD%BF%E7%94%A8%E7%9A%84%E6%98%AFIDEA,2021.2%E4%B9%8B%E5%89%8D%E7%89%88%E6%9C%AC%E7%9A%84%E8%AF%9D%E8%BF%98%E6%98%AF%E4%BD%BF%E7%94%A8%E5%BF%AB%E6%8D%B7%E9%94%AEshift%2BCtrl%2BAlt%2B%2F%EF%BC%8C%E9%80%89%E6%8B%A9Registry%E2%80%A6%EF%BC%8C%E5%B0%86compiler.automake.allow.when.app.running%E9%80%89%E9%A1%B9%E5%8B%BE%E4%B8%8A%E3%80%82)
- [Spring Boot 整合 DevTools（实现热部署）](https://blog.csdn.net/cxy35/article/details/105363784)
- [使用Spring Boot Devtools实现热部署](https://zhuanlan.zhihu.com/p/133233569)


