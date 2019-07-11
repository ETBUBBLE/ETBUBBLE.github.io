---
title: 如何将本地项目传入github
date: 2019-06-24 19:47:30
tags:
    - githunb
    - 项目
categories:
    - github
---
## 初始化文件夹
在本地执行`git init`命令
```
git init
```
![](https://i.loli.net/2019/06/24/5d10b9bce74cd92458.png)
## 添加需要上传到github的代码到本地仓库
`git status`
查看哪些文件是没有加入到本地仓库的，红色的没有，绿色是已经添加了的。

![](https://i.loli.net/2019/06/24/5d10b93ac049626865.png)

```
git add 
```
可以把需要的文件加入本地仓库

## 将add的文件commit到仓库
```
git commit -m "第一次提交"
```
![](https://i.loli.net/2019/06/24/5d10bae89913965935.png)

## 去github上创建自己的Repository
这个不用教了吧
## 将本地的仓库关联到github上
```
git remote add origin +链接
```
![](https://i.loli.net/2019/06/24/5d10bb532334543812.png)
![](https://i.loli.net/2019/06/24/5d10bb94e505d92771.png)
## 上传代码到github远程仓库
```
git push #传上去
git pull #拉回来
```

```
git pull --rebase origin master

git push -u origin master

```
