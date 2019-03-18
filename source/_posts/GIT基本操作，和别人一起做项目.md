---
title: GIT基本操作，和别人一起做项目
date: 2018-04-16 13:26:13
tags: CSDN迁移
---
  Git基本操作



GIT教程：[https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

GIT下载：

在自己的电脑上，安装Git  
下载页面：[https://git-scm.com/downloads](https://git-scm.com/downloads)  
下载安装，一路下一步即可。

GitHub Desktop下载地址：[https://desktop.github.com](https://desktop.github.com/)



GIT 简单的使用：

1、 注册码云：

![](https://img-blog.csdn.net/20180416131940970?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  


码云注册页面：[https://gitee.com/signup](https://gitee.com/signup)  
  
应该不需要我多说

2、你需要访问仓库（是个网址）：

如果你需要创建自己有一个专门的分支还需要fork；

![](https://img-blog.csdn.net/20180416231538900?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  




点击fork 后会让你选择自己，然后 点击确定 ，左上角变成你自己的名字就对了。  


左上角变成你自己的名字就对了  


左上角变成你自己的名字就对了  


左上角变成你自己的名字就对了  


一定要记住 ，那个红圈里面是自己的码云昵称。不然你就等着一直认证失败吧。

![](https://img-blog.csdn.net/20180417202350906?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  






![](https://img-blog.csdn.net/20180416132027824?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  
  


点击的【克隆/下载】，点击【复制】按钮复制下面的连接，我们把这个连接称为【连接①】



3、开始clone ：

第一种：

![](https://img-blog.csdn.net/20180416132130265?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

要打开的是Git Bash Here 不是 Git GUI here 上图指错了

然后开始克隆就行了，输入 命令 git clone 

![](https://img-blog.csdn.net/20180416132146962?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



做完这些你就会发现你当前文件夹下面多了你clone出来的文件夹，也就是你的本地仓了。



第二种：

打开cmd窗口  


按Win键+R，输入cmd，按【确定】

![](https://img-blog.csdn.net/20180416132319126?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  



```
在cmd窗口中切换文件夹 随便找一个文件夹 比如d盘中123文件夹，那么输入D: 敲回车，cd 123 敲回车即可切换 
```

```
这个是转换到你要转换到你要clone的目录下 然后输入
```

```

```

```
git config --global user.name "注册码云时用的昵称"
```

```

```

```
git config --global user.email "注册码云时用的邮箱"
```
这个是登录

![](https://img-blog.csdn.net/20180416132348756?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



特别提醒：转到的目录是仓库下，不是你原本clone的目录下，是到clone出来的那个文件夹目录下。别和前面clone的目录一样。

特别提醒：转到的目录是仓库下，不是你原本clone的目录下，是到clone出来的那个文件夹目录下。别和前面clone的目录一样。  


特别提醒：转到的目录是仓库下，不是你原本clone的目录下，是到clone出来的那个文件夹目录下。别和前面clone的目录一样。  




和第一种的区别是用的是自己电脑带的命令行。不是git给的。



4.上传 上去



一样的有两种操作我就之说一种，另一种自己照着上面两种进行操作。

先进行修改

![](https://img-blog.csdn.net/20180417125134707?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  


修改后 进入命令行 操作

转到仓库位置也就是你clone出来的文件夹。这个是clone出来的文件夹位置（本地仓的位置），不是clone到的位置。  
git add -A .  
git commit -m “姓名”  
git push   
  


![](https://img-blog.csdn.net/20180416232107283?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

第一次push 需要登陆 ，输入密码的时候 输入什么都看不见不用管，也有可能弹出一个窗口让你登陆。 



告诉大家一种用GitHub Desktop 这个东西进行简单操作；

下载地址[https://desktop.github.com](https://desktop.github.com/)

然后安装好就行。



安装好后 直接登陆，添加仓库

![](https://img-blog.csdn.net/20180416132434341?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





然后选中

![](https://img-blog.csdn.net/20180416132456543?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



你原本clone 的文件夹就可以了，

![](https://img-blog.csdn.net/20180416131837622?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如果 你提交的是自己的，请注意 那个分支要提交对了

// 注意第一次提交 要登陆 ，登陆的用户名是那个个人主页 名称下面的那几个英文。

6. 提交Pull Request

到仓库

![](https://img-blog.csdn.net/20180417093955978?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  


![](https://img-blog.csdn.net/20180416232404888?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  




然后就OK了，这样就可以愉快的和别人一起做项目了







   
 