---
title: 阿里云轻量应用服务器CentOS8.2配置记录
top_img: >-
  https://get.wallhere.com/photo/1920x1200-px-Agil-anime-anime-girls-Asada-Shino-Ayano-Keiko-Kirigaya-Kazuto-Kirigaya-Suguha-manga-Mills-Andrew-Gilbert-Shinozaki-Rika-Sword-Art-Online-Tsuboi-Ryotaro-Yuuki-Asuna-1107926.jpg
cover: >-
  https://get.wallhere.com/photo/1500x1060-px-animal-boat-building-cat-clouds-dog-Hana-male-Nama-original-Penguin-scenic-sky-water-1904667.jpg
top: false
tags:
  - Linux
  - CentOS8
categories:
  - Linux
date: '2024/1/11 17:45:26'
copyright_author: 白浪
copyright_info: 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
abbrlink: d565577d
---

# 重置密码
![](http://qiniu.heyzqf.com/aliyunCentos8/bce10f0c1d1a7b23fdd98e9f4f9ae06.png)

![](http://qiniu.heyzqf.com/aliyunCentos8/5440ccd461aab47d328f57dd535bcc6.png)


# 重新启用密码登录方式
因为轻量应用服务器创建密钥且重启服务器使密钥生效后，服务器会自动禁止使用 root用户 及 密码登录。如果您需要重新启用密码登录方式，需要修改服务器内的配置文件。具体操作如下所示：

1.通过管理控制台远程连接服务器

2.运行以下命令，打开/etc/ssh/sshd_config文件
```shell
sudo vim /etc/ssh/sshd_config
```

3.按 i 进入编辑模式，然后在文件末尾，将PasswordAuthentication no修改为PasswordAuthentication yes。

修改完成后，如下图所示

![](http://qiniu.heyzqf.com/aliyunCentos8/dbf1fd3c2cb5120b762a84c5ee14ee8.png)

4.按 Esc 退出编辑，然后输入 :wq 后按 Enter 键，保存并退出文件

5.运行以下命令，重启SSH服务
```shell
systemctl restart sshd.service
```

![](http://qiniu.heyzqf.com/aliyunCentos8/16f272bb4643a19282a078a3df49685.png)


# 在Windows环境中远程连接Linux服务器

通过系统用户及密码远程连接Linux实例

![](http://qiniu.heyzqf.com/aliyunCentos8/b1c1d90fcaa67b97591daae3291660a.png)
![](http://qiniu.heyzqf.com/aliyunCentos8/7c3d4dca4bcfc931f5e8aee84f1a77c.png)


# 关闭防火墙和SELinux

1、关闭防火墙
- 运行以下命令，查看当前防火墙的状态
  ```
  systemctl status firewalld
  ```
- 如果防火墙的状态参数是inactive，则防火墙为关闭状态

- 如果防火墙的状态参数是active，则防火墙为开启状态

- 临时关闭防火墙：
  ```
  sudo systemctl stop firewalld
  ```

- 永久关闭防火墙：
  - 关闭防火墙
  ```
  sudo systemctl stop firewalld
  ```
  - 实例开机时，禁止启动防火墙服务
  ```
  sudo systemctl disable firewalld
  ```

2、关闭SELinux

- 运行以下命令，查看SELinux的当前状态：
  ```
  getenforce  
  ```
  - 如果SELinux状态参数是Disabled，则SELinux为关闭状态
  - 如果SELinux状态参数是Enforcing，则SELinux为开启状态


# 切换CentOS 8源地址

CentOS 8操作系统版本结束了生命周期（EOL），Linux社区已不再维护该操作系统版本。建议您切换到Anolis或Alinux

```
本文主要说明ECS实例中的相关操作与配置，如果您的服务器不是ECS实例，请将源地址http://mirrors.cloud.aliyuncs.com替换为http://mirrors.aliyun.com。例如，yum源替换为http://mirrors.aliyun.com/centos-vault/8.5.2111/，epel源替换为http://mirrors.aliyun.com/epel-archive/8/。
```

- 运行以下命令备份之前的repo文件
  ```
  rename '.repo' '.repo.bak' /etc/yum.repos.d/*.repo
  ```

- 运行以下命令下载最新的repo文件
  ```
  wget http://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo -O /etc/yum.repos.d/Centos-vault-8.5.2111.repo
  
  wget http://mirrors.aliyun.com/repo/epel-archive-8.repo -O /etc/yum.repos.d/epel-archive-8.repo
  ```

- 运行以下命令替换repo文件中的链接
  ```shell
  sed -i 's/http:\/\/mirrors.cloud.aliyuncs.com/url_tmp/g'  /etc/yum.repos.d/Centos-vault-8.5.2111.repo &&  sed -i 's/http:\/\/mirrors.aliyun.com/http:\/\/mirrors.cloud.aliyuncs.com/g' /etc/yum.repos.d/Centos-vault-8.5.2111.repo && sed -i 's/url_tmp/http:\/\/mirrors.aliyun.com/g' /etc/yum.repos.d/Centos-vault-8.5.2111.repo

  sed -i 's/http:\/\/mirrors.aliyun.com/http:\/\/mirrors.cloud.aliyuncs.com/g' /etc/yum.repos.d/epel-archive-8.repo
  ```

- 运行以下命令重新创建缓存
  ```shell
  yum clean all && yum makecache
  ```

# 安装Nginx

- 安装Nginx 1.16.1
  
  ```shell
  sudo dnf -y install http://nginx.org/packages/centos/8/x86_64/RPMS/nginx-1.16.1-1.el8.ngx.x86_64.rpm
  ```

- 查看Nginx版本
  ```shell
  nginx -v
  ```

# 配置Nginx

- 查看Nginx配置文件默认路径
  ```shell
  cat /etc/nginx/nginx.conf
  ```

  在http大括号内，查看include配置项。即配置文件的默认路径

  ![](http://qiniu.heyzqf.com/aliyunCentos8/2d3cd1131a6a22263ea4065fde258b3.png)

- 在配置文件的默认路径下，备份默认配置文件
  ```shell
  cd /etc/nginx/conf.d

  sudo cp default.conf default.conf.bak
  ```

- 修改默认配置文件
  ```shell
  sudo vim default.conf


  在 location 大括号内，修改以下内容：

  location / {
	    #将该路径替换为网站根目录
        root   /usr/share/nginx/html;
	    #添加默认首页信息index.php、index.html等信息
        index  index.html index.htm;
    }
  ```

  -  **Nginx** 与 **PHP-FPM** 进程间通信方式有两种：
     - TCP Socket：该方式能够通过网络，可用于跨服务器通信的场景。
     - UNIX Domain Socket：该方式不能通过网络，只能用于同一服务器中通信的场景。

- 启动Nginx服务
  ```shell
  sudo systemctl start nginx
  ```

- 设置Nginx服务开机自启动
  ```shell
  sudo systemctl enable nginx
  ```

# 安装MySQL
  
- 安装MySQL
  ```shell
  sudo dnf -y install @mysql
  ```

- 查看MySQL版本
  ```shell
  mysql -V
  ```

# 配置MySQL

- 启动MySQL，并设置为开机自启动
  ```shell
  sudo systemctl enable --now mysqld
  ```

- 查看MySQL是否已启动
  ```shell
  sudo systemctl status mysqld
  ```

- 使用默认密码初次登录后, 必须要重置密码
  ```shell
  查看默认密码
  grep 'temporary password' /var/log/mysqld.log
  ```

  ```
  注意：如果你找不到文件，根本原因是mysql没有设置临时密码
  直接mysql -uroot -p进去，输入密码的时候回车
  ```

- 修改mysql登录密码（注意要切换到mysql数据库，使用use mysql）
  ```sql
  ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

  flush privileges;
  ```

- （如果你的mysql没有初始密码策略，想设置的可以设置一下）退出mysql，运行以下命令，执行MySQL安全性初始化操作并设置密码
  ```
  sudo mysql_secure_installation
  
  输入Y并回车开始相关配置

  选择密码验证策略强度，输入2并回车

  (策略0表示低，1表示中，2表示高。建议您选择高强度的密码验证策略)

  设置MySQL的新密码并确认(如果已经有了密码，不想更改的可以不改，输入No即可)

  设置密码123456

  输入Y并回车继续使用提供的密码

  输入Y并回车移除匿名用户

  设置是否允许远程连接MySQL

  不需要远程连接时，输入Y并回车

  需要远程连接时，输入N或其他任意非Y的按键，并回车

  输入Y并回车删除test库以及对test库的访问权限

  输入Y并回车重新加载授权表
  ```

- 查看mysql密码策略
  ```sql
  SHOW VARIABLES LIKE 'validate_password%'; 
  ```

- 修改mysql密码策略
  ```sql
  -- 密码验证策略低要求(0或LOW代表低级)
  set global validate_password.policy=0;

  -- 密码至少要包含的小写字母个数和大写字母个数
  set global validate_password.mixed_case_count=0;

  -- 密码至少要包含的数字个数。
  set global validate_password.number_count=0; 

  -- 密码至少要包含的特殊字符数
  set global validate_password.special_char_count=0; 

  -- 密码长度
  set global validate_password.length=6; 

  -- 刷新一下
  flush privileges;
  ```

- 查看服务端口
  ```sql
  show global variables like 'port';
  ```

- 服务器开启防火墙端口（如果不开启端口，则会报错：Can't connect to MysQL serer on 'xxxxxx'）
  ![](http://qiniu.heyzqf.com/aliyunCentos8/d7c873476352426ad5e4e04143627dc.png)

- 查看mysql连接的授权信息
  ```sql
  select host,user,authentication_string from mysql.user;
  ```

- 创建远程用户和授权
  
  在 **mysql8.0** 创建用户和授权和之前不太一样了，其实严格上来讲，也不能说是不一样, 只能说是更严格。
  
  **mysql8.0** 需要先**创建用户**（创建用户时要带@并指定地址, 则grant授权时的地址就是这个@后面指定的!, 否则grant授权就会报错!）和**设置密码**,然后才能授权。

  ```sql
  -- 创建远程root用户并设置密码
  create user 'root'@'%' identified by '123456';

  -- 授权用户
  grant all privileges on *.* to 'root'@'%' with grant option;

  flush privileges;
  ```

- SQLyog远程连接错误
  
  1、使用SQLyog远程连接时会出现**2058**的异常，此时我们需要修改mysql，命令行登录mysql（与修改密码中登录相同，使用修改后的密码），然后执行下面的命令：
  ```sql
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
  ```
  其中password为自己修改的密码。然后SQLyog中重新连接，则可连接成功，OK。

  2、如果报错：**ERROR 1396 (HY000): Operation ALTER USER failed for 'root'@'localhost'**，则使用下面命令：
  ```sql
  ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
  ```

- 查看MySQL编码方式
  ```sql
  SHOW VARIABLES WHERE Variable_name LIKE 'character_set_%' OR Variable_name LIKE 'collation%';

  character_set_client     (客户端来源数据使用的字符集)
  character_set_connection (连接层字符集)
  character_set_database   (当前选中数据库的默认字符集)
  character_set_results    (查询结果字符集)
  character_set_server     (默认的内部操作字符集)

  数据库连接参数中:
  characterEncoding=utf8 会被自动识别为utf8mb4，也可以不加这个参数，会自动检测
  autoReconnect=true 是必须加上的
  ```

- 部分参数配置查询命令
  ```sql
  -- 查询mysql最大连接数设置
  show global variables like 'max_conn%';
  
  -- 查看最大链接数
  show global status like 'max_used_connections';
  
  -- 查看慢查询日志是否开启以及日志位置
  show variables like 'slow_query%';
  
  -- 查看慢查询日志超时记录时间
  show variables like 'long_query_time';
  
  -- 查看链接创建以及现在正在链接数
  show status like 'threads%';
  
  -- 查看数据库当前链接
  show processlist;

  -- 查看数据库配置
  show variables like '%quer%'; 

  -- 根据库名查询所有表名
  show tables from 表名;

  -- 查询information_schema库
  select table_name from information_schema.tables where table_schema='表名';
  ```

- SQLyog远程连接数据库，创建数据库失败:
  >Access denied for user 'root'@'%' to database 'bailang'
  
  原因分析：root权限不够，登录mysql后通过 **SELECT * FROM mysql.user;** 命令查询权限信息，可以看到root对应的很多权限都是no，如下图所示：

  ![](http://qiniu.heyzqf.com/aliyunCentos8/848221d82b573cb2c1540876c957699.png)


  解决方案：将root权限全部修改为yes，执行如下代码（记得退出mysql，重启服务）：

  ```sql
  use mysql; 
  update user set Update_priv ='Y' where user = 'root';
  update user set Select_priv ='Y' where user = 'root';
  update user set Insert_priv ='Y' where user = 'root';
  update user set Update_priv ='Y' where user = 'root';
  update user set Delete_priv ='Y' where user = 'root';
  update user set Create_priv ='Y' where user = 'root';
  update user set Drop_priv ='Y' where user = 'root';
  update user set Reload_priv ='Y' where user = 'root';
  update user set Shutdown_priv ='Y' where user = 'root';
  update user set Process_priv ='Y' where user = 'root';
  update user set File_priv ='Y' where user = 'root';
  update user set Grant_priv ='Y' where user = 'root';
  update user set References_priv ='Y' where user = 'root';
  update user set Index_priv ='Y' where user = 'root';
  update user set Alter_priv ='Y' where user = 'root';
  update user set Show_db_priv ='Y' where user = 'root';
  update user set Super_priv ='Y' where user = 'root';
  update user set Create_tmp_table_priv ='Y' where user = 'root';
  update user set Lock_tables_priv ='Y' where user = 'root';
  update user set Execute_priv ='Y' where user = 'root';
  update user set Repl_slave_priv ='Y' where user = 'root';
  update user set Repl_client_priv ='Y' where user = 'root';
  update user set Create_view_priv ='Y' where user = 'root';
  update user set Show_view_priv ='Y' where user = 'root';
  update user set Create_routine_priv ='Y' where user = 'root';
  update user set Alter_routine_priv ='Y' where user = 'root';
  update user set Create_user_priv ='Y' where user = 'root';
  update user set Event_priv ='Y' where user = 'root';
  update user set Trigger_priv ='Y' where user = 'root';

  -- 刷新一下
  flush privileges;
  -- 退出mysql
  exit
  -- 重启mysql服务
  service mysqld restart
  ```

  当再去查询权限的时候已经全部修改为yes，问题就已经解决了

# 搭建基础 V2Ray（无伪装，容易被墙）

- 一键脚本安装V2Ray
  
  往期版本的脚本需要自己手动输入配置参数，现在新版本的脚本默认自动配置好所有参数，无需手动，运行命令即可。

  ```shell
  bash <(curl -s -L https://git.io/v2ray.sh)
  ```

- 检查 BBR 是否启动成功
  
  ```shell
  lsmod | grep bbr

  返回如下说明 开启成功
  tcp_bbr                20480  2
  ```

- 配置 V2Ray 客户端
  
  ```shell
  获取url
  v2ray url
  ```

  - 导入 vmess
  
    打开 v2rayN 软件界面，点击左上角的服务器，点击 从剪切板导入批量url

    ![](https://i.loli.net/2020/02/10/kgRQqhwpts97PLV.png)

  - 测试服务器
  
    右键点击刚刚导入的服务器，点击 测试服务器延迟 ，测试结果显示 ** ms 的话就说明服务器连接成功了。

  - 开启代理模式
    - PAC模式：只有在访问被墙了网站才会启用代理
    - 全局模式：所有连接都走代理

# 搭建 VLESS+Reality+uTLS+Vision 防止VPS端口封禁魔法

- 下载脚本
  
  ```shell
  wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
  ```

- 安装
  >有两种安装方式，第一种需要自己购买域名，第二种无需购买域名。此处展示第一种安装方式（有域名）

  选择: 2任意组合安装 -> 1Xray-core -> 7 ，这样的安装会同时安装 Vision_TLS、Reality+TCP+uTLS+Vision、Reality+gRPC+uTLS

  路径：2->1->7

  ![](http://qiniu.heyzqf.com/aliyunCentos8/f4fa86de36584d171dc7a9e41b15f13.png)

- 输入域名

  ```
  注意！这一步在初始化Nginx的时候很容易报错，注意查看报错信息和配置文件，大多数都是ssl文件丢失或者端口占用的错误。

  若配置了443端口，记得将default.conf文件关于443端口的配置注释掉。

  必要时修改vasma脚本代码，脚本位置：/etc/v2ray-agent/install.sh
  ```

  ![](http://qiniu.heyzqf.com/aliyunCentos8/658c9c3498e5c269a8f97e84cf5215b.png)

- 申请TLS证书(如果没有，填N即可)

  ![](http://qiniu.heyzqf.com/aliyunCentos8/dd5b302f8fbcebe8e2d41d8440df1ca.png)
  ![](http://qiniu.heyzqf.com/aliyunCentos8/098fa98078526aa0f88bdf2d6087043.png)

- 初始化Xray配置
  
  ![](http://qiniu.heyzqf.com/aliyunCentos8/369057f14391dd7aebfb2940d34c67d.png)

- 查看账号（订阅信息）
  
  脚本安装成功后，命令行输入vasma即可调用脚本
  ![](http://qiniu.heyzqf.com/aliyunCentos8/b02f96e96e0f3c432540518d338ddb4.png)

- 配置 V2Ray 客户端
  
  获取订阅信息后，直接 **从剪切板导入批量url** 即可。

  本文所用配置为：**通用格式(VLESS+reality+uTLS+Vision)**

  注意更新V2Ray客户端与Xray Core，否则可能会出现报错！

- 查看服务占用端口命令
  
  ```shell
  ss -ntlp | grep xxx(此处填写服务名字)
  ```

- 使用systemctl命令查看所有已启动的服务
  
  ```shell
  sudo systemctl list-units --type=service --state=running
  ```

# 卸载阿里云盾（2024年1月25日）

- 卸载aegis保护
  
  ```shell
  # 停止服务
  systemctl stop aegis 
  systemctl disable aegis
  rm -rf /etc/systemd/system/aegis.service

  # 删除启动项
  # 以下路径不一定都存在
  rm -f /etc/rc2.d/S80aegis
  rm -f /etc/rc3.d/S80aegis
  rm -f /etc/rc4.d/S80aegis
  rm -f /etc/rc5.d/S80aegis
  rm -f /etc/rc.d/rc2.d/S80aegis
  rm -f /etc/rc.d/rc3.d/S80aegis
  rm -f /etc/rc.d/rc4.d/S80aegis
  rm -f /etc/rc.d/rc5.d/S80aegis
  rm -f /etc/init.d/aegis

  # 删除目录
  rm -rf /usr/local/aegis
  ```

- 卸载云盾（尝试了但没成功）

  ```shell
  # 卸载云盾
  wget http://update.aegis.aliyun.com/download/uninstall.sh
  chmod +x uninstall.sh
  ./uninstall.sh
  wget http://update.aegis.aliyun.com/download/quartz_uninstall.sh
  chmod +x quartz_uninstall.sh
  ./quartz_uninstall.sh

  # 删除阿里云盾文件残留
  pkill aliyun-service
  rm -fr /etc/init.d/agentwatch /usr/sbin/aliyun-service
  rm -rf /usr/local/aegis*

  # 屏蔽阿里云盾IP
  iptables -I INPUT -s 140.205.201.0/28 -j DROP
  iptables -I INPUT -s 140.205.201.16/29 -j DROP
  iptables -I INPUT -s 140.205.201.32/28 -j DROP
  iptables -I INPUT -s 140.205.225.192/29 -j DROP
  iptables -I INPUT -s 140.205.225.200/30 -j DROP
  iptables -I INPUT -s 140.205.225.184/29 -j DROP
  iptables -I INPUT -s 140.205.225.183/32 -j DROP
  iptables -I INPUT -s 140.205.225.206/32 -j DROP
  iptables -I INPUT -s 140.205.225.205/32 -j DROP
  iptables -I INPUT -s 140.205.225.195/32 -j DROP
  iptables -I INPUT -s 140.205.225.204/32 -j DROP

  # FAQ
  # 删除文件提示Operation not permitted的处理办法
  # 普通用户有权限，可能进程服务占用了文件或目录，可以执行下面命令
  lsof +D [文件路径]
  kill -9 [pid]

  # 如果是root用户，依然报上面的错的话，则该档案很可能被锁定
  lsattr [文件]
  chattr -i [文件]

  # 注意：i属性chattr命令并不适合所有的目录
  ```

























