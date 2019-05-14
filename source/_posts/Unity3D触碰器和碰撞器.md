---
title: Unity3D触发器和碰撞器
date: 2019-03-26 14:25:35
tags:
    - unity
categories:
    - 游戏
    - U3D
---
## Unity3d碰撞检测中碰撞器与触发器的区别
要产生碰撞必须为游戏对象添加刚体（Rigidbody）和碰撞器，刚体可以让物体在物理影响下运动。碰撞体是物理组件的一类，它要与刚体一起添加到游戏对象上才能触发碰撞。如果两个刚体相互撞在一起，除非两个对象有碰撞体时物理引擎才会计算碰撞，在物理模拟中，没有碰撞体的刚体会彼此相互穿过。

物体发生碰撞的必要条件

两个物体都必须带有碰撞器(Collider)，其中一个物体还必须带有Rigidbody刚体。

在unity3d中，能检测碰撞发生的方式有两种，一种是利用碰撞器，另一种则是利用触发器。

**碰撞器：**一群组件，它包含了很多种类，比如：Box Collider（盒碰撞体），Mesh Collider（网格碰撞体）等，这些碰撞器应用的场合不同，但都必须加到GameObjecet身上。

**触发器**，只需要在检视面板中的碰撞器组件中勾选IsTrigger属性选择框。勾选Is Trigger 后不会有碰撞物理现象
触发信息检测：

1.MonoBehaviour.OnTriggerEnter(Collider collider)当进入触发器

2.MonoBehaviour.OnTriggerExit(Collider collider)当退出触发器

3.MonoBehaviour.OnTriggerStay(Collider collider)当逗留触发器

碰撞信息检测：

1.MonoBehaviour.OnCollisionEnter(Collision collision) 当进入碰撞器

2.MonoBehaviour.OnCollisionExit(Collision collision) 当退出碰撞器

3.MonoBehaviour.OnCollisionStay(Collision collision)  当逗留碰撞器

```
void OnTriggerEnter(Collider collider)
{
     //进入触发器执行的代码
}
void OnCollisionEnter(Collision collision) 
{
     //进入碰撞器执行的代码
}
```
