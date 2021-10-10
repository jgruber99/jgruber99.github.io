# 在CentOS6服务器上安装ownCloud9


## 介绍
> ownCloud是一种私人文件系统，在公司内部搭建方便上传、下载以及分享文件。
> 
> 产品可以上传需求文件，测试可以上传测试用例。
> 
> 参考地址：http://tecadmin.net/install-owncloud-on-centos/

<!--more-->

## 安装

### 第一步 添加仓库的yum源

```shell
rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
```

### 第二步 安装LAMP环境

#### 安装Apache，有的是系统默认安装

```shell
yum —enablerepo=remi,epel install httpd
```

#### 安装MySQL
 
```shell
yum —enablerepo=remi,epel install mysql-server
service mysqld start
mysql_secure_installation
```

#### 安装PHP环境

```shell
yum --enablerepo=remi,epel install php php-mysql php-mcrypt php-curl php-gd php-xml php-mbstring
```

### 第三步 下载owncloud源码

```shell
wget https://download.owncloud.org/community/owncloud-9.1.1.tar.bz2
tar -xjf owncloud-9.1.1.tar.bz2 -C /var/www/html/
chown -R apache.apache /var/www/html/owncloud
chmod -R 755 owncloud
```

#### 添加链接

> 我在按照原文安装后，浏览器访问404，查看`httpd`的错误日志才发现，提示`/var/wwwnone/html/owncloud`不存在，所以需要在这个位置添加一个链接。

```shell
ln -s /var/www/html/owncloud/ /var/wwwnone/html/owncloud
```

> 重启`httpd`服务

```shell
service httpd restart
```

### 第四步 创建数据库和用户

```shell
mysql -uroot -p
```
```mysql
mysql> CREATE DATABASE owncloud;
mysql> GRANT ALL ON owncloud.* TO 'owncloud'@'localhost' IDENTIFIED BY '_password_';
mysql> FLUSH PRIVILEGES;
mysql> quit
```

### 第五步 开启浏览器访问安装

> 在浏览器中输入`http://ip/owncloud`，创建admin用户，填写数据库连接地址，点击完成安装即可。

