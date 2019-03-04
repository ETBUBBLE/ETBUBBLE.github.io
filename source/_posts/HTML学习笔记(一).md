---
title: HTML前端学习笔记(一)
date: 2019-03-04 16:51:20
tags:
    - note
    - html
    - 前端
categories:
    - note
    - html
    - 前端
---
## 组成
 ```
 <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
	<title>Document</title>
</head>
<body>
	
</body>
</html>
 ```
 如上 由DOCTYPE 文档说明，上述是xhtml文档声明，html标签内部为页面开始 ，分为head头部，和body两部分，head部分为传给浏览器的数据，body为页面显示部分，主体部分。
 
 ## HEAD元素
### title
title标签内浏览器标题显示标签。
![](https://i.loli.net/2019/03/04/5c7ceb81be4ea.png)
如上图
### mate
这个标签内容较多，之说主要的几个
<table border="1"><tr><th>属性名</th><th>属性值</th><th>描述</th></tr><tr><th rowspan="3" align="center" valign="middle">name</th><th>keywords</th><th>提供网页关键字,关键字之间要用英文,隔开</th></tr><tr><th>description</th><th>提供页面描述</th></tr><tr><th>author</th><th>标注作者</th></tr><tr><th align="center" valign="middle">http-equiv</th><th>Content-Type</th><th>设置网页字符集，例如 &lt;meta http-equiv="Content-Type" content="text/html;charset=UTF-8"> </th></tr></table>
## 文本元素
### 标题
```
	<h1></h1>
	<h2></h2>
	<h3></h3>
```
### 文本修饰标签
基本上用不上，主要CSS解决。
### 段落标签
``` 
<p></p> 
```
### 列表
#### 有序列表
```
<ol>
		<li></li>
		<li></li>
		<li></li>
	</ol>
```
### 无序列表
```
    <ul>
		<li>首页</li>
		<li>比赛</li>
		<li>排行</li>
		<li>题目</li>
	</ul>
```
### 自定义列表
```
	<dl>
		<dt>标题</dt> 
		<!-- 标题 -->
		<dd>描述</dd>
		<dt></dt>
		<dt></dt>
	</dl>
```
### div与span
用于分块

### 图片
```
<img src="图片地址" alt="代替文字" />
```
### 超文本
```
    <a href="连接">文字</a>
    
    <a href="#anchor">连接锚点</a>
	<a id="anchor">我的锚点</a>
    
    
    <a href="#">空连接回到最上面</a>
```


