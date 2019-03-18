---
title: 使用VScode 调试的时候提示Unable to start debugging
date: 2019-03-02 18:20:34
tags: CSDN迁移
---
  ## 使用VScode 调试的时候提示Unable to start debugging. Launch options string provided by the project system is invalid. Unable to determine path to debugger. Please specify the "MIDebuggerPath" option.

 ![提示错误](https://img-blog.csdnimg.cn/20190302181608228.png)

 提示这个错误。

 翻译过来就是 miDebuggerPath他出错了。

 如果你是在linux 下按照官网的陪应该就是

 ![](https://img-blog.csdnimg.cn/20190302181856139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 这个地方陪错了，要先下载gdb 

 在终端输入 apt-get install gdb

 然后把gdb的路径丢进去就行了。

 

   
 