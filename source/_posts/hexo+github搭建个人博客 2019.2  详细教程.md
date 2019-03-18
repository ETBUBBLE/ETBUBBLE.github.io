---
title: hexo+github搭建个人博客 2019.2  详细教程
date: 2019-02-04 17:45:33
tags: CSDN迁移
---
  大过年的，就去试试搭一个自己的博客，看了别人的博客，照着搭了一个，中间因为各种bug，试了半天哎。不过也是个很好理解github,hexo的法子，毕竟不出错，怎么会理解。中间还有些东西还没搞明白，希望大佬前来解答一下。

 下面就开始搭建了。

 
# 准备工作

 node.js [https://nodejs.org/en/](https://nodejs.org/en/) 

 链接：[https://pan.baidu.com/s/1-I1ROA_2rq4MDG1BFe0BqA](https://pan.baidu.com/s/1-I1ROA_2rq4MDG1BFe0BqA) 提取码：cnsz 

 GIT [https://git-scm.com/downloads](https://git-scm.com/downloads) 

 百度云链接：[https://pan.baidu.com/s/1-I1ROA_2rq4MDG1BFe0BqA ](https://pan.baidu.com/s/1-I1ROA_2rq4MDG1BFe0BqA%C2%A0) 提取码：cnsz   
 (不要问为啥给你们放个百度云链接 ，我特么官网下不了，还找了半天)

 安装好后确认一下：

 ![](https://img-blog.csdnimg.cn/20190204163424732.png)

 装好后，在cmd 或者 power shell 里面打出这几个指令，就可以显示版本，也就是有这几个软件。

 
## first

 注册个github,这个小孩子玩意我就不和你们BB，自己去注册。

 ![](https://img-blog.csdnimg.cn/2019020416395090.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 注册好后应该是个这样的瞎J B样子，然后创建一个仓库

 ![](https://img-blog.csdnimg.cn/20190204164620559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 仓库名: _yourname**_._github.oi**_

 

 

 

 

 

 

 

 ![](https://img-blog.csdnimg.cn/20190204164932220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 进入setting

 ![](https://img-blog.csdnimg.cn/20190204165031831.png)

 这个是你的仓库名，然后向下滑动。

 ![](https://img-blog.csdnimg.cn/20190204165501861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 上面那个网址，就是你可以外网访问的，你的服务器就是这个了。我们现在就是要把博客放上去。

 在这个地方遇到了几个错误

 
```
 cd ~/.ssh
```
 这个指令进入ssh,如果显示没有这个文件夹，那就直接创建文件夹BLOG 后面就是用来放博客文件的

 ![](https://img-blog.csdnimg.cn/2019020417012734.png)

 进入文件夹，右击，git bash，(通过cmd,power shell 命令行进入也是一样的)

 ![](https://img-blog.csdnimg.cn/20190204170312757.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 在命令行中输入指令 hexo init 初始化。

 
```
 hexo init
```
 ![](https://img-blog.csdnimg.cn/20190204170558692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 输入npm install，安装所需要的组件

 
```
 npm install
```
 ![](https://img-blog.csdnimg.cn/20190204170905725.png)

 然后输入 hexo -g 静态部署

 
```
 hexo g
```
 ![](https://img-blog.csdnimg.cn/20190204170954724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 差不多就是这个样子，这个时候 博客已经可以访问了。

 要看一下的话输入 hexo s 服务器启动

 
```
 hexo s
```
 ![](https://img-blog.csdnimg.cn/20190204171101827.png)

 然后你输入一下 [http://localhost:4000/](http://localhost:4000/) 就可以看一下

 localhost 是本机的地址 ，端口是 4000 如果 4000 端口被用了的话可以换一个端口。

 hexo server -p 端口号

 指令是这个，发生什么我就不截图了。

 ![](https://img-blog.csdnimg.cn/20190204171257114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 应该是这个样子。

 看完之后ctrl +c 停止运行服务器。

 。

 下一步就是把这个网页传到github 上去。

 为了方便我们建一个 ssh

 git使用https协议，每次pull,push都要输入密码，使用git协议，使用ssh秘钥，可以省去每次输密码  
 一、本地生成密钥对；  
 二、设置github上的公钥；  
 

 cd ~/.ssh 

 ssh-keygen -t rsa -C “你创建那个github的邮箱” 

 
```
 cd ~/.ssh
ssh-keygen -t rsa -C "3035536707@qq.com"
```
 连续三个回车，生成密钥，最后得到了两个文件：id_rsa和id_rsa.pub（默认存储路径是：C:\Users\Administrator\.ssh ）找不到就用搜索。

 如果显示没有这个文件夹，就先创建，再进入，就是把两个指令换一下输入。

 ![](https://img-blog.csdnimg.cn/20190204171944688.png)

 用记事本打开这个

 ![](https://img-blog.csdnimg.cn/20190204172054327.png)

 然后把里面的东西复制一下，应该是一大堆看不懂的东西。

 然后就放到github上。

 ![](https://img-blog.csdnimg.cn/20190204172230822.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 

 ![](https://img-blog.csdnimg.cn/20190204172337770.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 

 

 

 

 ![](https://img-blog.csdnimg.cn/20190204172506322.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 测试：  
 在终端 ssh -T git@github.com

 ![](https://img-blog.csdnimg.cn/20190204172743225.png)

 添加好后

 重新回到你创建的文件夹。

 ![](https://img-blog.csdnimg.cn/20190204172636325.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 文本编辑一下这个文件

 
```
 deploy:
  type: git
  repository: git@github.com:ETBUBBLE/ETBUBBLE.github.io.git
  branch: master
```
 

 ![](https://img-blog.csdnimg.cn/20190204173200916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 你的仓库地址就是是这个

 ![](https://img-blog.csdnimg.cn/20190204172929891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 这个是你前面看过的那个玩意，如果没有设置会出啥问题自己去试试。

 ![](https://img-blog.csdnimg.cn/20190204173502880.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 然后重新回到命令行，进入 BLOG 文件夹

 在生成以及部署文章之前，需要安装一个扩展

 ![](https://img-blog.csdnimg.cn/20190204173703274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 然后输入 hexo clean

 hexo d -g

 
```
 hexo clean
hexo d -g
```
 传到仓库去就可以了。

 ![](https://img-blog.csdnimg.cn/20190204174036466.png)

 ![](https://img-blog.csdnimg.cn/20190204174228418.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 然后就再也没有然后了。

 ![](https://img-blog.csdnimg.cn/20190204174149915.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

 hexo new post "你的博客内容" 

 就可以发送出去了。  
 一个博客就搭好了，是不是特别简单。

 

 
## 

 
## 补充绑定域名

 去域名解析上面添加解析。

 ![](https://img-blog.csdnimg.cn/20190224142727377.png)

 然后再在自己的github仓库里面新建一个CNAME文件，没有后缀 里面写上你的域名。

 
```
 www.etbubble.xyz
```
 再去source创建一个CNAME文件和上面一样

 ![](https://img-blog.csdnimg.cn/20190224144132388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

   
 