---
title: MYSQL
layout: post
date:   2020-07-07 20:32:09 +0800
categories: 秋招
tags:
	- mysql
	- 数据库
---

# WINDOWS 安装

1. 从官网下载zip包，解压到磁盘的某一个位置，然后在其目录下新建my.ini文件内容如下：

   ```ini
   [client]
   # 设置mysql客户端默认字符集
   default-character-set=utf8
    
   [mysqld]
   # 设置3306端口
   port = 3306
   # 设置mysql的安装目录
   basedir=C:\\web\\mysql-8.0.11
   # 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
   # datadir=C:\\web\\sqldata
   # 允许最大连接数
   max_connections=20
   # 服务端使用的字符集默认为8比特编码的latin1字符集
   character-set-server=utf8
   # 创建新表时将使用的默认存储引擎
   default-storage-engine=INNODB
   ```

2. 以管理员身份运行cmd并进入mysql的bin目录下面

   - 初始化数据库

     ```powershell
     mysqld --initialize --console
     ```

     执行完，会输出root用户的初始默认密码

     ```powershell
     2018-04-20T02:35:05.464644Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost:APWCY5ws&hjQ
     ```

     `APWCY5ws&hjQ`就是默认密码，后续登录需要用到，也可以在登陆后修改密码。

   - 安装

     ```powershell
     mysqld install
     ```

   - 启动

     ```powershell
     net start mysql
     ```

3. 登录mysql

   ```powershell
   mysql -h host -u user -p
   ```

   `-h` : 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0.1)该参数可以省略;

   `-u` : 登录的用户名;

   `-p` : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。

   要登录本机的 MySQL 数据库，只需要输入以下命令即可：

   ```powershell
   mysql -u root -p
   ```

4. 修改密码

   进入mysql命令行输入如下命令

   ```mysql
   # mysql8.0 以后的版本
   alter user root@localhost identified by 'newpassword';
   flush privileges;
   
   # mysql8.0 之前的版本
   use mysql;
   update user set authentication_string='' where user='root';
   # 或者
   update mysql.user set password='newpassword' where user='root';
   # 或者
   update mysql.user set password=PASSWORD('newpassword') where User='root';
   ```

5. 启动mysql

   ```powershell
   mysqld --console
   ```

6. 关闭mysql

   ```powershell
   mysqladmin -uroot -p shutdown
   ```

# 管理MySQL的命令

1. USE 数据库名 :

   选择要操作的Mysql数据库，使用该命令后所有Mysql命令都只针对该数据库

   ```mysql
   use RUNOOB;
   ```

   ![image-20200707210155508](https://gitee.com/zyuegege/images/raw/master/imgs/image-20200707210155508.png)

2. SHOW DATABASES:

   列出 MySQL 数据库管理系统的数据库列表

   ```mysql
   SHOW DATABASES;
   ```

   ![image-20200707210243231](https://gitee.com/zyuegege/images/raw/master/imgs/image-20200707210243231.png)

3. SHOW TABLES:

   显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库

   ```mysql
   SHOW TABLES;
   ```

   ![image-20200707210449528](https://gitee.com/zyuegege/images/raw/master/imgs/image-20200707210449528.png)

4. SHOW COLUMNS FROM 数据表:

   显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息

   ```mysql
   SHOW COLUMNS FROM user;
   ```

   ![image-20200707210645697](https://gitee.com/zyuegege/images/raw/master/imgs/image-20200707210645697.png)

5. SHOW INDEX FROM 数据表:

   显示数据表的详细索引信息，包括PRIMARY KEY（主键）

   ```mysql
   SHOW INDEX FROM user;
   ```

   ![image-20200707210832569](https://gitee.com/zyuegege/images/raw/master/imgs/image-20200707210832569.png)

6. SHOW TABLE STATUS LIKE [FROM db_name] [LIKE 'pattern'] \G:

   该命令将输出Mysql数据库管理系统的性能及统计信息

   ```mysql
   SHOW TABLE STATUS  FROM RUNOOB;   # 显示数据库 RUNOOB 中所有表的信息
   
   SHOW TABLE STATUS from RUNOOB LIKE 'runoob%';     # 表名以runoob开头的表的信息
   SHOW TABLE STATUS from RUNOOB LIKE 'runoob%'\G;   # 加上 \G，查询结果按列打印
   ```

