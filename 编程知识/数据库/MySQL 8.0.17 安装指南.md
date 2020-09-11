# MySQL 8.0.17 安装指南

环境 Centos 8

下载安装

```shell
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum update
yum install mysql-server
```

权限设置：

```
chown mysql:mysql -R /var/lib/mysql
chmod -R 777 /var/lib/mysql
```

初始化 MySQL：

```
mysqld --initialize
```

查看默认密码

```
sudo grep 'temporary password' /var/log/mysql/mysqld.log 
```

启动 MySQL：

```
systemctl start mysqld
```

登录MySQL:

```
mysql -u root -p
输入默认密码
```

修改密码

```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '*{your-password}*';
```

退出MySQL并用新密码重新登录，开搞！

