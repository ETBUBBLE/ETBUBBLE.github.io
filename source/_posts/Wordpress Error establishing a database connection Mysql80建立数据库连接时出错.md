---
title: Wordpress Error establishing a database connection Mysql80建立数据库连接时出错
date: 2019-02-07 21:34:39
tags: CSDN迁移
---
  # 建立数据库连接时出错

 这或者意味着文件中的用户名和密码信息 `wp-config.php` 不正确，或者我们无法联系数据库服务器 `localhost` 。这可能意味着主机的数据库服务器已关闭。

 
  * 您确定拥有正确的用户名和密码吗？ 
  * 您确定已键入正确的主机名吗？ 
  * 您确定数据库服务器正在运行吗？ 如果您不确定这些术语的含义，您应该联系您的主人。如果您仍需要帮助，可以随时访问[WordPress支持论坛](https://wordpress.org/support/)。

 

 ![](https://img-blog.csdnimg.cn/20190207211754451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 出现这个问题有几种可能，最常见几种就是用户名密码或者数据库名打错了。

 还有一种可能就是你的mysql加密方式不一样。

 先输入 use mysql ;

 再输入 select host,user,plugin from user; 

 
```
 use mysql;
select host,user,plugin from user;  
```
 查看加密方式，就是 plugin 这个下面显示。

 caching_sha2_password 是一种新的加密方式，wp和discuz 有些版本 是不支持的。

 所以这个时候要修改一下

 输入指令 update user set plugin='mysql_native_password' where user='root';

 
```
 update user set plugin='mysql_native_password' where user='root';
```
 ![](https://img-blog.csdnimg.cn/20190207212102454.png)

 输入指令之后就是下面这个样子。输入这个指令之后 你原本的密码加密方式变了，所以你的密码也变了，如果你技术好，可以研究一下你的密码变成了多少，如果算不出来就老老实实改密码。注意：改密码必须指明加密方式，不然又会变回去。

 ![](https://img-blog.csdnimg.cn/2019020721274534.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 输入指令：ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';

 
```
 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```
 

 然后在输入前面那个指令看一下 select host,user,plugin from user; 

 最后输入最后一个更新权限指令： flush privileges;

 
```
 flush privileges;
```
 然后退出重启就行了。

 ![](https://img-blog.csdnimg.cn/20190207213137334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

   
 