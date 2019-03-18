---
title: Unity 点击UI与点击屏幕冲突
date: 2019-02-20 22:37:34
tags: CSDN迁移
---
  Unity 有点击屏幕进行移动操作，通过Input.GetMouseButtonDown(0)。如果点击到了一些UI上面会触发点击屏幕事件。

 引入UnityEngine.EventSystems，用函数判断一下即可

 ![](https://img-blog.csdnimg.cn/20190220222237381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 
```
 using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using DG.Tweening;
using UnityEngine.EventSystems;
public class PlayerController : MonoBehaviour
{
    private void Update()
    {
        if (EventSystem.current.IsPointerOverGameObject()) return;
        if (Input.GetMouseButtonDown(0))
        {
            Debug.Log("点击屏幕");
        }
    }
}
```
 这个方法会将点击Text的时候也会当作点击UI

 将raycast target 取消勾选可以避免。

 ![](https://img-blog.csdnimg.cn/20190220222821768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

 

   
 