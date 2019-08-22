---
title: hexoNext主题插入数学公式
date: 2019-07-17 09:34:26
tags:
    - hexo
categories:
    - 配置

mathjax: true
---

## 开启mathjax
![](https://i.loli.net/2019/07/17/5d2e7b83ebc8c86302.png)
先把这个打开，然后看到mathjax上面这一行了没有，要用`hexo-rendering-pandoc` 或者`hexo-renderer-kramed`这个渲染，第一个我试的时候发现和`hexo-renderer-marked`这个语法有点出入 *(hexo默认使用 `hexo-renderer-marked` 渲染)* ，如果换了前面写所有md文件的全都要改语法，所以我就用了第二个,先把`hexo-renderer-marked`的卸了，再装`hexo-renderer-kramed`。
```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```
用数学公式的时候记得打开
![](https://i.loli.net/2019/07/17/5d2e7d2c533cd98537.png)

测试代码如下
```
$$
P = \frac
{\sum_{i=1}^n (x_i- x)(y_i- y)}
{\displaystyle \left[
\sum_{i=1}^n (x_i-x)^2
\sum_{i=1}^n (y_i-y)^2
\right]^{1/2} }
$$
```
效果如下:

$$
P = \frac
{\sum_{i=1}^n (x_i- x)(y_i- y)}
{\displaystyle \left[
\sum_{i=1}^n (x_i-x)^2
\sum_{i=1}^n (y_i-y)^2
\right]^{1/2} }
$$

---
## HexoEditor 展示数学公式
如果你用这个编辑器，想实时展示这个效果就打开`TeX数学表达式`。
![](https://i.loli.net/2019/07/17/5d2e7dd14830476215.png)
效果如下：
![](https://i.loli.net/2019/07/17/5d2e7e3d639fd34691.png)
