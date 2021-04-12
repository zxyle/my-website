---
title: CentOS 安装MySQL
---



## 介绍
使用oracle提供的mysql bundle安装包进行安装.



## 准备
- CentOS 7系统
- [清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-8.0/)
- [阿里云镜像站](https://mirrors.aliyun.com/mysql/MySQL-8.0/)
- 关键字查找 mysql-8.0.23-1.el7.x86_64.rpm-bundle.tar

## 下载
```shell
wget https://mirrors.aliyun.com/mysql/MySQL-8.0/mysql-8.0.23-1.el7.x86_64.rpm-bundle.tar
```

## 安装
```shell
# 依次安装下面5个rpm包 common --> libs --> libs-compat --> client --> server
mysql-community-common
mysql-community-libs
mysql-community-libs-compat
mysql-community-client
mysql-community-server

# 强制安装
rpm -ivh mysql-community-common-8.0.23-1.el7.x86_64.rpm --force --nodeps
rpm -ivh mysql-community-libs-8.0.23-1.el7.x86_64.rpm --force --nodeps
rpm -ivh mysql-community-libs-compat-8.0.23-1.el7.x86_64.rpm --force --nodeps
rpm -ivh mysql-community-client-8.0.23-1.el7.x86_64.rpm --force --nodeps
rpm -ivh mysql-community-server-8.0.23-1.el7.x86_64.rpm --force --nodeps
```

## 服务启停
```shell
# 启动服务，关闭服务就是service mysqld stop
service mysqld start
```

## 账号修改
```shell
# 查看临时密码
[root@localhost log]# cat /var/log/mysqld.log 

# 登录临时root用户，使用上面临时密码
[root@localhost log]# mysql -u root -p

#修改root账号密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'QWqw12!@';

# 切换数据库
use mysql;

# 修改root账号方便其他Host访问
UPDATE mysql.user SET host = '%' WHERE user = 'root';

# 赋予权限
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';

# 刷新权限
FLUSH PRIVILEGES;
```

> 远程访问需要系统开放3306端口

## binlog开启
注意一点是MySQL8.x默认开启binlog

```sql
SHOW VARIABLES LIKE '%bin%';
```
![1412331-20200313004407681-279772349.png](https://note.youdao.com/src/WEBRESOURCE46076a70ff52804e65e27a9f3be7e17e)

## 配置文件地址
- /etc/my.cnf
- datadir=/var/lib/mysql
- log-error=/var/log/mysqld.log
- pid-file=/var/run/mysqld/mysqld.pid