---
title: 安装JAVA环境 java能用javac不能用
date: 2019-02-22 19:52:40
tags: CSDN迁移
---
  安装java环境就不说了，百度一下一大片。重点是安装那个安装好后javac不能用。

 这个大部分都是因为没有主义一下环境设置细节。

 我把这几个打成代码免得不知道引号和空格是不是要输入的。。。

 新建环境变量

 变量名 

 
```
 CLASSPATH
```
 变量值 

 
```
 .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
```
 新建环境变量

 变量名

 
```
 JAVA_HOME
```
 变量值 //是你安装JDK的路径别直接复制粘贴

 
```
 H:\Program Files\Java\jdk
```
 前面两个是新建路径，一般不会有啥问题。后面这个一般是编辑，因为系统自带一部分路径。编辑就比较容易出错。

 编辑环境变量 

 变量名

 
```
 Path
```
 变量值

 
```
 C:\ProgramData\Oracle\Java\javapath;%java_home%\bin;%java_home%\jre\bin;
```
 编辑之后应该是这个样子，一开始有一部分。

 ![](https://img-blog.csdnimg.cn/20190222193951679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 一般就是出错在这，大部分人看不懂那个路径就直接复制粘贴上去。就会出现这种情况

 ![](https://img-blog.csdnimg.cn/20190222194351176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 解决办法，点击右下角的编辑文本,把下面两个引号删除就可以了

 ![](https://img-blog.csdnimg.cn/20190222194549733.png)

 然后你重新点击编辑变量值，应该是下面这个样子。

 ![](https://img-blog.csdnimg.cn/20190222194656931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 出现这种情况是因，你直接复制粘贴的这一段里面有分号，分号是环境变量的分割符，你输入的文本里面有分号，结果系统自动给你加了引号。

 把分号去掉一段一段的输入也是可以的。

 

   
 