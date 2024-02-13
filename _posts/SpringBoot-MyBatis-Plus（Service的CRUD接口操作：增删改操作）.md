---
title: SpringBoot-MyBatis-Plus（Service的CRUD接口操作：增删改操作）
top_img: https://w.wallhaven.cc/full/z8/wallhaven-z8vjrw.jpg
cover: https://w.wallhaven.cc/full/6d/wallhaven-6dqk5l.jpg
abbrlink: SpringBoot-MyBatis-Plus（Service的CRUD接口操作：增删改操作）
tags:
    - JAVA
    - SpringBoot
    - MyBatis-Plus
categories:
    - 杂七杂八

date: 2022/11/13 23:09:21
---
<!-- <font size=4></font> -->

# 准备工作

## 数据表设计
~~~sql
/*
SQLyog Ultimate v10.00 Beta1
MySQL - 8.0.30 
*********************************************************************
*/
/*!40101 SET NAMES utf8 */;
create table `user` (
	`id` bigint ,
	`username` varchar ,
	`password` varchar ,
	`age` varchar ,
	`create_by` bigint ,
	`create_time` datetime ,
	`update_by` bigint ,
	`update_time` datetime ,
	`del_flag` int 
); 
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('18','username_1','password_1','1','3','2022-11-13 20:45:21','3','2022-11-13 20:45:21','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('19','username_2','test_password','123321','3','2022-11-13 20:45:21','3','2022-11-13 21:56:19','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('20','username_3','password_3','3','3','2022-11-13 20:45:21','3','2022-11-13 20:45:21','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('21','username_1','password_1','1','3','2022-11-13 20:45:29','3','2022-11-13 20:45:29','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('22','username_2','test_password','123321','3','2022-11-13 20:45:29','3','2022-11-13 21:56:19','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('23','77777','888888888','99999','3','2022-11-13 20:45:29','3','2022-11-13 20:50:51','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('24','username_1','password_1','1','3','2022-11-13 20:45:38','3','2022-11-13 20:45:38','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('25','77777','888888888','99999','3','2022-11-13 20:45:38','3','2022-11-13 22:10:37','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('26','username_3','password_3','3','3','2022-11-13 20:45:38','3','2022-11-13 20:45:38','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('27','aaaaaaaaaa','bbbbbbbbbbbbbbbb','1111111','3','2022-11-13 20:45:48','3','2022-11-13 21:01:00','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('28','wwwwwwwwwwwwww','ddddddddddddddddd','333333','3','2022-11-13 20:45:48','3','2022-11-13 21:01:00','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('29','username_3','password_3','3','3','2022-11-13 20:45:48','3','2022-11-13 20:45:48','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('30','username_1','password_1','1','3','2022-11-13 20:45:56','3','2022-11-13 20:45:56','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('31','username_2','test_password','123321','3','2022-11-13 20:45:56','3','2022-11-13 21:56:19','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('32','username_3','password_3','3','3','2022-11-13 20:45:56','3','2022-11-13 20:45:56','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('33','username_1','password_1','1','3','2022-11-13 20:46:05','3','2022-11-13 20:46:05','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('34','username_2','test_password','123321','3','2022-11-13 20:46:05','3','2022-11-13 21:56:19','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('35','username_3','password_3','3','3','2022-11-13 20:46:05','3','2022-11-13 20:46:05','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('36','username_1','password_1','1','3','2022-11-13 20:46:13','3','2022-11-13 20:46:13','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('37','username_2','test_password','123321','3','2022-11-13 20:46:13','3','2022-11-13 21:56:19','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('38','username_3','password_3','3','3','2022-11-13 20:46:13','3','2022-11-13 20:46:13','1');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('39','77777','888888888','99999','3','2022-11-13 22:07:20','3','2022-11-13 22:07:20','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('40','123321123321','888888888','99999','3','2022-11-13 22:09:41','3','2022-11-13 22:09:41','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('41','77777','888888888','99999','3','2022-11-13 22:10:01','3','2022-11-13 22:10:01','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('42','aaaaaaaaaa','bbbbbbbbbbbbbbbb','1111111','3','2022-11-13 22:10:37','3','2022-11-13 22:10:37','0');
insert into `user` (`id`, `username`, `password`, `age`, `create_by`, `create_time`, `update_by`, `update_time`, `del_flag`) values('43','wwwwwwwwwwwwww','ddddddddddddddddd','333333','3','2022-11-13 22:10:37','3','2022-11-13 22:10:37','0');
~~~
![](../../../img/posts_img/SpringBoot-MyBatis-Plus（Service的CRUD接口操作：增删改操作）/2022-11-14-20-52-52.png)

## 导入依赖
~~~xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.0</version>
</parent>

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
    <!--springboot Web开发依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--MySql数据库依赖-->
    <!--导入了mysql依赖后需要连接数据库
    在application.yaml配置文件中配置连入数据库 的参数，url：跟自己数据库的地址，数据库、username和password填上自己数据库的名字和密码即可连接-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <!--Mybatis-Plus依赖-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.4.3</version>
    </dependency>
    <!--Lombok依赖-->
    <!--导入lombok依赖后还需要进行一步操作，下载lombok插件，方法：点击File—>Setting—>Plugins
    然后再搜索框搜索Lombok，安装插件即可-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
~~~

## application.yml项目配置
~~~yml
spring:
  # 数据库配置
  datasource:
    url: jdbc:mysql://localhost:3306/mybatis-plus-crud-test?characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: 数据库用户名
    password: 数据库密码
    driver-class-name: com.mysql.cj.jdbc.Driver

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

## entity实体类
~~~java
@Data
@AllArgsConstructor
@NoArgsConstructor
@TableName("user")  // 标识表名
public class User  {
    @TableId // 标识主键ID
    private Long id;
    //用户名
    private String username;
    //密码
    private String password;
    //年龄
    private String age;

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

    //删除标志（0代表未删除，1代表已删除）
    @TableLogic // 逻辑删除 注释
    private Integer delFlag;
}
~~~

## mapper
~~~java
public interface UserMapper extends BaseMapper<User> {

}
~~~

## service
~~~java
public interface UserService extends IService<User> {

}
~~~

## serviceimpl
~~~java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {

}
~~~

## MP字段自动填充
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
        this.setFieldValByName("updateBy", (longzi)3, metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
~~~

## 文件目录结构
![](../../../img/posts_img/SpringBoot-MyBatis-Plus（Service的CRUD接口操作：增删改操作）/2022-11-14-20-58-09.png)


# Service的CRUD接口操作（代码实现）：增删改操作
<font size=4>
</font>

## save新增数据
- <font size=4>save方法 可以将一个实体对象插入到对应的数据表中：（插入成功后，当前插入对象在数据库中的 id 会写回到该实体中）</font>
~~~java
User user1 = new User();
user1.setUsername("username_1");
user1.setPassword("password_1");
user1.setAge("1");

userService.save(user1);              // 新增单个数据
~~~
- <font size=4>saveBatch 方法可以批量插入数据：</font>
~~~java
User user1 = new User();
user1.setUsername("username_1");
user1.setPassword("password_1");
user1.setAge("1");

User user2 = new User();
user2.setUsername("username_2");
user2.setPassword("password_2");
user2.setAge("2");

User user3 = new User();
user3.setUsername("username_3");
user3.setPassword("password_3");
user3.setAge("3");

List<User> userList = Arrays.asList(user1, user2, user3);

userService.saveBatch(userList);      // 批量新增数据
~~~
- <font size=4>saveBatch 方法还可以设置每个批次的插入数量：</font>
~~~java
User user1 = new User();
user1.setUsername("username_1");
user1.setPassword("password_1");
user1.setAge("1");

User user2 = new User();
user2.setUsername("username_2");
user2.setPassword("password_2");
user2.setAge("2");

User user3 = new User();
user3.setUsername("username_3");
user3.setPassword("password_3");
user3.setAge("3");

List<User> userList = Arrays.asList(user1, user2, user3);

userService.saveBatch(userList, 2);   // 批量新增数据（每批2条数据）
~~~

## update修改数据
- <font size=4>updateById 方法根据实体对象中的 ID 进行修改：（如果实体对象中某个属性为 null，不会更新该属性（即不会把对应的数据库字段值设置为null））</font>
~~~java
User user1 = new User();
user1.setUsername("77777");
user1.setPassword("888888888");
user1.setAge("99999");
user1.setId((long)25);

userService.updateById(user1);                // 根据ID修改单个数据
~~~
- <font size=4>updateBatchById 方法根据实体对象中的 ID 进行批量修改：</font>
~~~java
User user1 = new User();
user1.setUsername("77777");
user1.setPassword("888888888");
user1.setAge("99999");
user1.setId((long)25);

User user2 = new User();
user2.setUsername("aaaaaaaaaa");
user2.setPassword("bbbbbbbbbbbbbbbb");
user2.setAge("1111111");
user2.setId((long)27);

User user3 = new User();
user3.setUsername("wwwwwwwwwwwwww");
user3.setPassword("ddddddddddddddddd");
user3.setAge("333333");
user3.setId((long)28);

List<User> userList = Arrays.asList(user1, user2, user3);

userService.updateBatchById(userList);        // 根据ID批量修改数据
~~~
- <font size=4>updateBatchById 方法还可以设置每个批次的修改的数量：</font>
~~~java
User user1 = new User();
user1.setUsername("77777");
user1.setPassword("888888888");
user1.setAge("99999");
user1.setId((long)25);

User user2 = new User();
user2.setUsername("aaaaaaaaaa");
user2.setPassword("bbbbbbbbbbbbbbbb");
user2.setAge("1111111");
user2.setId((long)27);

User user3 = new User();
user3.setUsername("wwwwwwwwwwwwww");
user3.setPassword("ddddddddddddddddd");
user3.setAge("333333");
user3.setId((long)28);

List<User> userList = Arrays.asList(user1, user2, user3);

userService.updateBatchById(userList, 2);     // 根据ID批量更新数据（每批2条数据）
~~~
- <font size=4>update 方法可以使用实体对象封装操作类进行更新操作：</font>
~~~txt
数据更新相关的构造器（UpdateWrapper、LambdaUpdateWrapper、LambdaUpdateChainWrapper）使用方法类似于查询构造器（QueryWrapper、LambdaQueryWrapper、LambdaQueryChainWrapper），不同的是它增加了如下两个方法：
    set：设置数据库字段值
    setSql：设置 set 部分的 sql
~~~
  - <font size=4>我们也可以通过 updateWrapper 的 set 方法直接设置字段值</font>
  ~~~java
  // 查询条件：名字中包含 "2" 并且 Age小于123322
  // 将符合条件的查询结果的password设置为："test_password", Age设置为： "123321"
  LambdaUpdateWrapper<User> userLambdaQueryWrapper1 = new LambdaUpdateWrapper<>();
  userLambdaQueryWrapper1.like(User::getUsername, "2").lt(User::getAge, 123322)
          .set(User::getPassword,"test_password")
          .set(User::getAge, "123321");
  // 开始修改
  // boolean isUpdata = userService.update(userLambdaQueryWrapper);  
  // 注意坑：updata只传入Wrapper条件时，不触发MybatisPlus的自动填充字段方法。使用下面带有实体类的传参重载方法便可解决
  boolean isUpdata = userService.update(user3, userLambdaQueryWrapper);

  /*********** 二者可以结合使用的，下面效果等效于上面的 ****************/

  // 查询条件：名字中包含 "2" 并且 Age小于3
  // 将符合条件的查询结果的password设置为："test_password", Age设置为： "123321"
  LambdaUpdateWrapper<User> userLambdaQueryWrapper2 = new LambdaUpdateWrapper<>();
  userLambdaQueryWrapper2.like(User::getUsername, "2").lt(User::getAge, 123322)
          .set(User::getPassword,"test_password");
  User user4 = new User();
  user4.setAge("123321");
  // 开始修改
  boolean isUpdata = userService.update(user4, userLambdaQueryWrapper2);
  ~~~
  - <font size=4>也可以通过 updateWrapper 的 setSql 方法可以直接设置 set 部分的 sql，下面的效果同上面是一样的：</font>
  ~~~java
  // 查询条件：名字中包含 "2" 并且 Age小于123322
  // 将符合条件的查询结果的password设置为："test_password", Age设置为： "123321"
  LambdaUpdateWrapper<User> userLambdaQueryWrapper1 = new LambdaUpdateWrapper<>();
  userLambdaQueryWrapper1.like(User::getUsername, "2").lt(User::getAge, 123322)
          .setSql("password = test_password")
          .setSql("age = 123321");
  // 开始修改
  // boolean isUpdata = userService.update(userLambdaQueryWrapper);  
  // 注意坑：updata只传入Wrapper条件时，不触发MybatisPlus的自动填充字段方法。使用下面带有实体类的传参重载方法便可解决
  boolean isUpdata = userService.update(user3, userLambdaQueryWrapper);

  /*********** 二者可以结合使用的，下面效果等效于上面的 ****************/

  // 查询条件：名字中包含 "2" 并且 Age小于3
  // 将符合条件的查询结果的password设置为："test_password", Age设置为： "123321"
  LambdaUpdateWrapper<User> userLambdaQueryWrapper2 = new LambdaUpdateWrapper<>();
  userLambdaQueryWrapper2.like(User::getUsername, "2").lt(User::getAge, 123322)
          .setSql("password = test_password");
  User user4 = new User();
  user4.setAge("123321");
  // 开始修改
  boolean isUpdata = userService.update(user4, userLambdaQueryWrapper2);
  ~~~


## save or updata新增或修改数据
- <font size=4>saveOrUpdate 会先根据实体类中的ID判断数据库是否有该数据，如果有的话则执行更新操作，没有的话则执行新增操作：（实体类中没有ID便执行新增操作）</font>
~~~java
User user1 = new User();
user1.setUsername("77777");
user1.setPassword("888888888");
user1.setAge("99999");
user1.setId((long)25);

User user2 = new User();
user2.setUsername("aaaaaaaaaa");
user2.setPassword("bbbbbbbbbbbbbbbb");
user2.setAge("1111111");
user2.setId((long)27);

// 由于数据库中有 user1，则执行更新操作
userService.saveOrUpdate(user1);                // 根据ID修改或者新增单个数据
// 由于数据库中没有 user2，则执行新增操作
userService.saveOrUpdate(user2);                // 根据ID修改或者新增单个数据
~~~
- <font size=4>saveOrUpdateBatch 方法可以执行批量的新增或修改操作：</font>
~~~java
User user1 = new User();
user1.setUsername("77777");
user1.setPassword("888888888");
user1.setAge("99999");
user1.setId((long)25);

User user2 = new User();
user2.setUsername("aaaaaaaaaa");
user2.setPassword("bbbbbbbbbbbbbbbb");
user2.setAge("1111111");
user2.setId((long)27);

User user3 = new User();
user3.setUsername("wwwwwwwwwwwwww");
user3.setPassword("ddddddddddddddddd");
user3.setAge("333333");
user3.setId((long)28);

List<User> userList = Arrays.asList(user1, user2, user3);

userService.saveOrUpdateBatch(userList);        // 根据ID批量修改或者新增数据
~~~
- <font size=4>saveOrUpdateBatch 方法还可以设置每个批次的新增或修改的数量：</font>
~~~java
User user1 = new User();
user1.setUsername("77777");
user1.setPassword("888888888");
user1.setAge("99999");
user1.setId((long)25);

User user2 = new User();
user2.setUsername("aaaaaaaaaa");
user2.setPassword("bbbbbbbbbbbbbbbb");
user2.setAge("1111111");
user2.setId((long)27);

User user3 = new User();
user3.setUsername("wwwwwwwwwwwwww");
user3.setPassword("ddddddddddddddddd");
user3.setAge("333333");
user3.setId((long)28);

List<User> userList = Arrays.asList(user1, user2, user3);

userService.saveOrUpdateBatch(userList, 2);     // 根据ID批量修改或者新增数据（每批2条数据）
~~~


## remove删除数据
><font size=4>删除操作实际上是：将逻辑删除字段标识为 "1" （1为已删除字段）</font>

- <font size=4>removeById 方法可以根据 id 删除一条记录：</font>
~~~java
userService.removeById(18); // 根据ID删除单个数据
~~~
- <font size=4>removeByIds 方法根据 id 批量删除：</font>
~~~java
userService.removeByIds(Arrays.asList(20,21,22,23,25)); // 根据 id 批量删除数据
~~~
- <font size=4>removeByMap 方法通过 Map 封装的条件删除记录：</font>
~~~txt
注意：map 写的是数据表中的列名，而非实体类的属性名。比如属性名为 userName，数据表中字段为 user_name，这里应该写的是 user_name。
~~~
~~~java
// map 写的是数据表中的列名，而非实体类的属性名。比如属性名为 userName，数据表中字段为 user_name，这里应该写的是 user_name
Map<String, Object> columnMap = new HashMap<>();
columnMap.put("age", "3");
userService.removeByMap(columnMap);
~~~
- <font size=4>remove 方法使用查询构造器，删除记录：</font>
~~~java
// 查询条件：名字中包含 "1", 并且年龄小于2
LambdaQueryWrapper<User> userLambdaQueryWrapper = new LambdaQueryWrapper<>();
userLambdaQueryWrapper.like(User::getUsername, "1").lt(User::getAge, 2);
// 返回删除结果
boolean isRemove = userService.remove(userLambdaQueryWrapper);
~~~


参考链接

- [SpringBoot - MyBatis-Plus使用详解11（Service的CRUD接口3：增删改操作）](https://www.hangge.com/blog/cache/detail_2919.html)



