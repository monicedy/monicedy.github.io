---
layout: post 
date: 2020-12-05
title: " Ubuntu双系统及几种服务的搭建"
---

一、Ubuntu双系统安装

 1.  Ubuntu下载 
  > http://cn.ubuntu.com/download/
 
 2. 压缩一个分区给Linux备用

 
 1.  软碟通烧录镜像进U盘
 			
> 注意：UEFI支持一定要打开，否则会导致Linux分区时找不到efi项
 4. BIOS将U盘设置为第一启动项
 5.  安装类型选择其他

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201201220543548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTMzNDY3,size_16,color_FFFFFF,t_70)

 6. 在刚刚压缩出来的空间中建立4个分区，分别为

> 200M		-	efi			启动分区
> 8000M		-	swap		交换分区
> 30000M	-	/				根目录
20000M		-	/home		用户目录
 
 

 7. **将系统装入 efi分区** 
 8. 设置用户名密码
 9. 重启，Ubuntu自动建立了双系统引导                    
 
 

> 参考自[https://blog.csdn.net/flyyufenfei/article/details/79187656](https://blog.csdn.net/flyyufenfei/article/details/79187656)

 


二、Linux基本命令


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201201221600226.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTMzNDY3,size_16,color_FFFFFF,t_70#pic_center)


三、服务搭建

1、FTP

 1. 1.创建一个用户

```
adduser <用户名>
passwd <用户名> <密码>
```

 1. 2.下载vsftpd

```
apt install vsftpd
```

 1. 3.提前释放防火墙需要用到的端口

```
sudo ufw allow from any to any port 20,21,10000:10100 proto tcp
```

 1. 4.开启服务

```
service vsftpd start
```

 1. 5.修改配置文件

```
sudo vi /etc/vsftpd.conf

#配置文件内容
listen=NO
listen_ipv6=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
pasv_enable=Yes
pasv_min_port=10000
pasv_max_port=10100
allow_writeable_chroot=YES
```

 1. 6.重启服务

```
service vsftpd restart
```

 1. 7.访问

```
//命令终端访问
ftp <IP地址> 

//浏览器访问
ftp://<IP地址>

```

2、数据库服务

 2. 1.安装Mysql服务端和客户端

```
apt install mysql-server 
apt install mysql-client
```

 2. 2.启动数据库服务

```
service mysql start
```

 2. 3.进入数据库

```
sudo mysql -uroot
```

2. 4.创建一个可以远程访问数据库的用户并设置密码

```
create user <usrname> identified by '123456';
```

 2. 5.给刚创建的用户授予访问数据库的权限

```
grant all privileges on *.* to 'usrname'@'%';
```

 2. 6.刷新权限

```
flush privileges;
```

 2. 7.修改Mysql配置，使外部能访问FTP

```
vi /etc/mysql/mysql.conf.d/mysqld.cnf 

//注释掉下行
#bind-address=127.0.0.1`
```

 2. 8.重启Mysql服务

```
service mysql restart
```

 2. 9.远程访问

```
mysql -h<IP> -u<用户> -p3306 -p<密码>
```

 2. 10.显示数据库内容以检验是否连接成功

```
show databases;
```


3、http服务搭建

 3. 1.下载nginx

```
apt install nginx
```

 3. 2.开启nginx服务

```
service nginx start
```

 3. 3.在浏览器中测试是否成功

```
http://<IP地址>
```




