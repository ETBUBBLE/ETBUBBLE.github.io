---
title: JAVA环境配置
date: 2019-02-27 19:50:03
tags:
    - JAVA
    - 配置
categories:
    - JAVA
    - 配置
---
本篇文章用于记录JAVA配置

JAVA安装，百度搜索JAVA官网，下载对应系统版本，下载安装好后应该有 ***JDK*** 和***JDR*** 两个文件夹

配置系统环境
**(1)新建->变量名"JAVA_HOME"，变量值** `C:\Java\jdk1.8.0_05`(你的JDK路径)
**(2)新建->变量名“CLASSPATH”,变量值**  `.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`
第三个变量，如果已经有了Path变量应该直接编辑，然后再在背后添加变量
**(3)编辑->变量名"Path"，在原变量值的最后面加上** `;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin`

变量值是以 `；` 区分几个变量值。在win10情况下，点击编辑Path路径能够看见一个图表 里面存有各个变量值，点击编辑文本可以看见变量值文本形式，两者对照看应该就能明白怎么给Path添加一个新的变量。

