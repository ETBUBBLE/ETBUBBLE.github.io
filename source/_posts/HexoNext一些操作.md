---
title: HexoNext一些操作
date: 2019-03-16 23:28:09
tags:
    - hexo
    - 配置
categories:
    - 配置
---
## 更改头像
1.在主题配置文件中搜索 avatar
2.![](https://i.loli.net/2019/03/16/5c8d168f25b28.png)
3.找到这个路径修改就行了。
4.![](https://i.loli.net/2019/03/16/5c8d17d61e219.png)

## 添加个人css
`themes\next\source\css\_custom`修改这个文件

```css
// Custom styles.
@media screen and (min-width:1200px) {

    body {
   		background:url(https://i.loli.net/2019/03/01/5c7942720fc39.png);
    	background-repeat: no-repeat;
    	background-attachment:fixed;
    	background-position:50% 50%;
    	background-size: cover;
	}

    #footer a {
        color:#eee;
    }
    .main-inner { 
    	margin-top: 10px;    	
    	opacity: 0.8;
	}
	.header-inner {
		margin-top: 10px;
		position: absolute;
		top: auto;
		overflow: hidden;
		padding: 0;
		width: 240px;
		background: #fff;
		box-shadow: initial;
		border-radius: initial;
		opacity: 1;
	}
	.post {
 	margin-bottom: 30px; 
   	padding: 25px;
   	-webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   	-moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
  	}
}
```
