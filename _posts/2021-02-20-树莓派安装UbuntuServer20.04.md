---
layout: post
date: 2021-02-18
title: "树莓派安装UbuntuServer20.04"
tags: "blog"
categories: jekyll update
---

## 1.树莓派4b刷入ubuntu server 20.04
#### 1.1 下载[树莓派官网](https://www.raspberrypi.org/software/operating-systems/)中的ubuntu server
- 因为主要是用于vscode的服务器，所以这里不装图形界面。如果有需要，可以自行选择链接中的`Ubuntu Desktop`

#### 1.2 插入内存卡

- 这里记住内存卡盘符

#### 1.3 使用[win32diskimager](https://win32diskimager.download/)将下载好的镜像刷入**上述**内存卡

- 注意别选错了，尤其是电脑插了u盘或移动硬盘的情况下

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210204170239234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTMzNDY3,size_16,color_FFFFFF,t_70)

- 至此，镜像已经刷入内存卡中

#### ~~1.4 开启ssh~~ 

- 为了能够在*没插显示器*的情况下，能够进入ssh远程配置

- 在boot分区中，新建空白文件`ssh`，注意没有任何后缀
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210204170631581.png)

#### 1.5 连接网络

- 因为*没插显示器*，所以需要开机前配置好网络
- 此处提供两种方法

###### 1.5.1 在boot分区中，找到`network-config`文件，在里面更改成你的wifi信息

```bash
wifis:
	wlan0:
		dhcp4: true
		optional: true
		access-points:
			wifi名称:
				password: “密码”

```


###### 1.5.2 或者直接插网线
- 插路由器LAN口
#### 1.6 插电开机
###### 1.6.1 找到设备的`ip`
- 这里不作过多赘述，一般在路由器的管理界面可以找到，设备名是ubuntu

###### 1.6.2 用ssh登陆设备，（用户名和密码都是*ubuntu*）
```bash
ssh ubuntu@[你的ip]
```
###### 1.6.3 登陆成功后会要求你重新设置密码
- 至此ubuntu server已经安装完毕

# ~~【部署 `code-server`】~~ 


