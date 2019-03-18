---
title: 用虚拟机CentOS7 做服务器 搭建个人博客 详细教程 2019.02
date: 2019-02-11 15:23:37
tags: CSDN迁移
---
  服务器用的是CentOS7 , 我建在虚拟机上，像腾讯云什么的其实也是一样的。

 虚拟机的安装就不说了，不懂的自己去百度下，一百度一大堆。

 
# 准备工作

 没有给出下载连接，都是些常见的东西，如果实在找不到或者有疑问留下评论。

 安装 有CentOS 虚拟机的 VMware (安装CentOS7 的时候记得打开网卡，不然后面要用命令行打开挺麻烦的，这个自己去百度怎么打开。)   
 Xshell 6 同类型的连接服务器软件也可以，腾讯云或自己的连接也行。

 
# start

 （先登录。。。。）

 查看自己虚拟机 IP 指令：ip add

 
```
 ip add
```
 ![](https://img-blog.csdnimg.cn/20190211132800300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 如上图 就是IP4 的地址，然后通过Xshell6 连接 (为了模拟服务器连接，所以虚拟机直接后台运行，实际上直接在虚拟机处理也是一样，这里说一下连接服务器的方法)

 ![](https://img-blog.csdnimg.cn/20190211133125161.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 新建一个连接

 ![](https://img-blog.csdnimg.cn/20190211133339822.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 ![](https://img-blog.csdnimg.cn/20190211133532819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 然后连接就可以了

 ![](https://img-blog.csdnimg.cn/20190211133452428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 关闭防火墙，也可以开一个端口。

 为了方便我直接关闭防火墙。

 
```
 systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动
```
 ![](https://img-blog.csdnimg.cn/20190211135247802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 然后直接安装，httpd即可也就是Apache

 指令：yum install httpd

 
```
 yum install httpd
```
 然后启动服务

 指令:systemctl start httpd.service

 
```
 systemctl start httpd.service
```
 ![](https://img-blog.csdnimg.cn/20190211135646309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 输入IP应该就能看见这个玩意了。

 说明已经安装好了

 然后就是安装mysql

 因为没有mysql 源 所以先装一个。

 指令：sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm

 yum install mysql mysql-server mysql-libs mysql-server  
 

 
```
 sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
yum install mysql mysql-server mysql-libs mysql-server
```
 然后启动一下这个mysql服务

 systemctl restart mysql.service

 
```
 systemctl restart  mysql.service   #重启mysql服务
systemctl start  mysql.service     #启动mysql服务
systemctl stop  mysql.service      #停止mysql服务
```
 为mysql设置登陆密码，然后登陆，在创建一个wordpress 的数据库。

 
```
 /usr/bin/mysqladmin -u root password '123456'   #后面这两个引号里面的是密码
mysql -uroot --password='123456'                #输入登陆密码
CREATE DATABASE wordpress;                      #创建wordpress数据库
exit                                            #退出mysql
```
 ![](https://img-blog.csdnimg.cn/20190211140718169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 然后再安装PHP

 指令：yum install php-fpm php-mysql -y

 
```
 yum install php-fpm php-mysql -y
yum install php
yum install php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc
```
 启动PHP服务

 再把它设置为开机启动。

 
```
 service php-fpm start
chkconfig php-fpm on
```
 然后再把，Apache 和 mysql 设置成开机自动启动再重启一下服务

 
```
 systemctl enable httpd.service
systemctl enable mysqld.service
systemctl restart httpd.service
systemctl restart mysqld.service
```
 ![](https://img-blog.csdnimg.cn/20190211141240546.png)

 然后创建一个 php文件试试是不是成功装好了PHP

 指令：vi /var/www/html/info.php (如果直接修改不了就进入这个文件夹里面先创建再修改)

 
```
 vi /var/www/html/info.php
```
 然后按 i 进入输入模式，再输入

 
```
 <?php
phpinfo();
?>
```
 然后按ESC 按 : 输入 wq 确定 ，保存退出。

 然后再去浏览器输入 网址 你原本的ip/info.php

 ![](https://img-blog.csdnimg.cn/20190211143345717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 应该就是这个样子，然后，就去自己电脑上找wordpress 的文件

 ![](https://img-blog.csdnimg.cn/20190211143452830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 然后获得这个安装包

 wget 刚才那个连接

 如果显示 没有wget 先 输入 yum install wegt 先安装

 
```
 wget https://wordpress.org/latest.zip
```
 然后在 解压 unzip latest.zip 

 如果显示没有unzip 一样的先输入 yum insatll unzip

 
```
 unzip latest.zip      #是什么文件名就是解压什么文件
```
 然后再复制到html 文件里面去

 
```
 cp -rf wordpress/* /var/www/html/
```
 再修改一下文件权限。

 
```
 chmod -R 777 html/

```
 ![](https://img-blog.csdnimg.cn/20190211151611670.png)

 然后输入IP应该就可以进入安装界面了。

 ![](https://img-blog.csdnimg.cn/20190211151527643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 然后一路点下去就行了，输入数据库名，还有数据库登陆账户和密码，就是登陆mysql的。用户名一般就是root。

 ![](https://img-blog.csdnimg.cn/20190211151652550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 ![](https://img-blog.csdnimg.cn/20190211151832632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 如果出现这个，把里面内容复制一遍，创建一个wp-config.php文件然后复制粘贴进去就行了。

 然后按提示一路下去就行了，如果后面装插件出了点问题，要FTP协议的话就去装一下

 yum install vsftpd

 useradd admin

 passwd 123456systemctl enable vsftpd.service

 systemctl restart vsftpd.service

 
```
 yum install vsftpd

useradd admin

passwd 123456

systemctl enable vsftpd.service 

systemctl restart vsftpd.service


```
 

   
 