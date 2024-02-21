---
layout:  post
title:  "UGUI 渲染顺序"
date:  2024-02-19 12:00:00 +0800
category: Unity
tags:
  - Unity
  - UGUI
---

* content
{:toc}

## UGUI 渲染顺序

1. Camera

    顺序由 Camera 的 Depth 值决定，值越小越先渲染。 Camera 的 Culling Mask 决定渲染和射线检测的 layer

2. Canvas  

    Canvas默认的Render Mode是Screen Space-Overlay模式，此时仅具有sort order属性，当把它设置为Screen Space-Camera或者World Space时，sort order属性消失了，变成了另外两个属性：sorting layer，order in layer。

    当把画布设置为Screen Space-Camera或者World Space后，画布及画布上的UI对象在世界空间默认也是靠Z值来决定渲染顺序的，离相机远的先渲染。当设置了sorting layer后，渲染的次序就由sorting layer来确定了。sorting layer是自定义的标识符，哪个sortinglayer在前（在layer & tags中设置的先后顺序）哪个先渲染。

    order in layer是个数值，是在同一个sorting layer内的细分，sorting layer相同时order in layer的数值越小越先渲染。

    屏幕空间的东西都处于UI层不需要sorting layer，只提供sort order(其实也就是order in layer)。 默认情况下sort oder都是0，此时UI物件按照在hierachy中出现的顺序决定渲染顺序。如果sort order不同时，值越小越先渲染。

## 特殊情况

1. Sprite 

    特别注意sprite默认是个3D空间的物体，就算把它放到一个屏幕空间的canvas上，它仍然是3D空间的物体，只遵循3D空间的规则：总是会被UI挡住。

2. 特效
    
    可参考方案: SpriteRender, UIParticleSystem, 挂不同 Canvas