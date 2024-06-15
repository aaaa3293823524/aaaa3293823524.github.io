---
title: mysql集群搭建
tags:
  - 搭建博客
  - 前端
date: 2023-11-09 21:24:53
abbrlink: 8ncro
---

rpm -qa | grep mysql 

rpm -qa | grep mari
rpm -e --nodeps


wget -c https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm

rpm -ivh mysql80-community-release-el7-7.noarch.rpm


yum -y install mysql-server

#查看mysql运行状态
systemctl status mysqld
#启动mysql
systemctl start mysqld
#停止mysql 
systemctl stop mysqld
#重启mysql 
systemctl restart mysqld  


#开启mysql开机自启动
systemctl enable mysqld
#关闭mysql开机自启动
systemctl disable mysqld


//获取MySQL临时密码
grep 'temporary password' /var/log/mysqld.log


//根据下图中的密码策略设置mysql数据库密码(你不设置密码就无法进行其它操作)                      
alter user 'root'@'localhost' identified by 'password@0';

set global validate_password.policy=LOW;

alter user 'root'@'localhost' identified by 'aa283617';


允许外部访问该MySQL数据库

//创建用户
       (远程连接用的账号)           (远程连接用的密码)
create user 'root'@'%' identified by 'mypassword';

create user 'root'@'%' identified by 'aa283617';
 
//分配权限，运行远程连接
                         (允许root账号远程连接)
grant all privileges on *.* to 'root'@'%' with grant option;
 
//刷新权限
flush privileges;



#关闭防火墙
systemctl stop firewalld.service


#永久允许该端口被外部访问(3306是MySQL默认端口号)
firewall-cmd --permanent --add-port=3306/tcp
#重启防火墙
firewall-cmd --reload

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'aa283617';
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'aa283617';
FLUSH PRIVILEGES;

# 主从复制
配置文件
/etc/my.cnf
/etc/mysql/mysql.conf.d/mysqld.cnf
存数据
/var/lib/mysql

shutdown -h now

cat /var/log/mysqld.log | grep ERROR


show variables like 'log_bin';

mysql -uroot -paa283617 -hnode1

[mysqld]
log-bin=mysql-bin
binlog-format=row
server-id=1
binlog-do-db=user_db
binlog-ignore-db=mysql
binlog-ignore-db=information_schema
binlog-ignore-db=performance_schema
binlog-ignore-db=sys



[mysqld]
log-bin=mysql-bin
binlog-format=row
server-id=2
replicate_wild_do_table=user_db.%
replicate_wild_ignore_table=mysql.%
replicate_wild_ignore_table=information_schema.%
replicate_wild_ignore_table=performance_schema.%
replicate_wild_ignore_table=sys.%




CHANGE MASTER TO
master_host = 'node4',
master_user = 'root',
master_password = 'aa283617',
master_log_file = 'mysql-bin.000001',
master_log_pos = 157;


show slave status\G;


DROP TABLE IF EXISTS `t_user`;
CREATE TABLE `t_user` (
`user_id` bigint(20) NOT NULL COMMENT '用户id',
`fullname` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '用户姓名',
`user_type` char(1) DEFAULT NULL COMMENT '用户类型',
PRIMARY KEY (`user_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;


show plugins;

# gtid复制
change master to 
master_host='master',
master_user='root',
master_password='aa283617' ,
MASTER_AUTO_POSITION=1;      #自动判断复制起点


percona-toolkit
库级别
基于组提交的并行复制

https://blog.csdn.net/xxxzzzqqq_/article/details/130340907
并行复制writeset
https://blog.csdn.net/liuyunshengsir/article/details/129416612



<property name="driver" value="com.mysql.cj.jdbc.Driver"/>
<property name="url" value="jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC"/>

![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-02-11_01-23-50.png)
