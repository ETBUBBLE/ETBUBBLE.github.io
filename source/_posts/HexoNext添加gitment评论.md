---
title: HexoNext添加gitment评论
date: 2019-07-17 09:11:16
tags:
    - hexo
categories:
    - 配置
---

## 添加gitment评论区
### 安装gitment
```
npm install gitment --save  #安装gitment
```
### 创建应用
再创建一个 [OAuth application](https://github.com/settings/applications/new)
```
Application name:随便写
Homepage URL:这个也可以随意写,就写你的博客地址就行
Application description:描述,也可以随意写
Authorization callback URL:这个必须写你的博客地址
```
申请好之后点注册,然后就可以看到两个东西`ClientID`和`Client Secret`,后面会用到.

![](https://i.loli.net/2019/07/15/5d2c277d2346d92308.png)
创建完后这个等会要用上
### 配置
```
# Gitment
# Introduction: https://imsun.net/posts/gitment-introduction/
gitment:
 enable: true
 mint: false # 如果要修改gitment.swig地址就改成false    RECOMMEND, A mint on Gitment, to support count, language and proxy_gateway
 count: true # Show comments count in post meta area
 lazy: false # Comments lazy loading with a button
 cleanly: false # Hide 'Powered by ...' on footer, and more
 language: # Force language, or auto switch by theme
 github_user: {you github user id}
 github_repo: 随便写一个你的公开的git仓库就行,到时候评论会作为那个项目的issue
 client_id: {刚才申请的ClientID}
 client_secret: {刚才申请的Client Secret}
 proxy_gateway: # Address of api proxy, See: https://github.com/aimingoo/intersect
 redirect_protocol: # Protocol of redirect_uri with force_redirect_protocol when mint enabled
```
![](https://i.loli.net/2019/07/17/5d2e76ba6056846846.png)
然后在 主题配置文件next config.yml中开启gitment 
![](https://i.loli.net/2019/07/17/5d2e76df0333f93602.png)
因为某个服务器挂了，所以mint要为false才会执行你所修改的。
>**错误：** ***object ProgressEventc*** 错误就是上面这个原因，把mint改成法false即可。

找到`themes/next/layout/_third-party/comments/gitment.swig`文件
修改其中的css 和js ,注释掉的是一开始默认的，现在改成下方没有注释的。 ~~**(后面这个是汉化的)**~~
```html
<!-- LOCAL: You can save these files to your site and update links -->
{% if theme.gitment.mint %}
  {% set CommentsClass = 'Gitmint' %}
  <script src="https://cdn.jsdelivr.net/gh/theme-next/theme-next-gitment@1/gitmint.browser.js"></script>
{% else %}
  {% set CommentsClass = 'Gitment' %}
     <!-- <script src="https://cdn.jsdelivr.net/gh/theme-next/theme-next-gitment@1/gitment.browser.js"></script> -->
<script src="https://billts.site/js/gitment.js"></script>
{% endif %}
<!-- <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/theme-next/theme-next-gitment@1/default.css"/> -->
<link rel="stylesheet" href="https://billts.site/extra_css/gitment.css">
<!-- END LOCAL -->
```
需要注意的是确保`themes/next/_config.yml`中`theme.gitment.mint`设置为false,才会走到我们改动的分支.

还有修改一下`gitment`模板，只需要修改id
还是在上面那个文件, ~~**（我也不知道为啥要改，大佬门说要改，实际上我没改也没出现啥错误**~~ 
```
var gitment = new {{CommentsClass}}({
 id: '{{ page.date }}',
 owner: '{{ theme.gitment.github_user }}',
 repo: '{{ theme.gitment.github_repo }}',
```

