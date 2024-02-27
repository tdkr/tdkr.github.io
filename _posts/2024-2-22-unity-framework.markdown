---
layout:  post
title:  "游戏开发框架"
date:  2024-02-21 12:00:00 +0800
category: Unity
tags:
  - Unity
---

* content
{:toc}

# 网络通信

## 通讯方式
* Http, 获取资源更新，登录服等
* TCP Socket
* 基于 TCP Socket 的 WebSocket
* UDP，方案包括 KCP

## 数据包格式
* http2(grpc)
* 自定义数据包: header(size, payload, checksum)
* WebSocket(DataFrame)

## 数据协议
* Protobuf
* Json

# 资源管理

## 方案

* Resources
* AssetBundle, 如 YooAsset
* Addressable

## 资源管理
* 同步/异步加载
* 依赖加载
* 资源卸载

# 代码热更新

## 方案

* ILRuntime, 内置.net解释器，两个虚拟机，跨域问题
* xLua
* HybridCLR, 拓展 il2cpp, 由纯 AOT runtime 变成 AOT + Interpreter 的混合runtime，支持动态加载assembly

# 渲染组件
* Mesh & Mesh Filter & Mesh Render

  Mesh是Unity提供网格数据类型，Mesh Renderer对Mesh数据在程序中进行渲染，而Mesh Filter起到桥梁的作用，将Mesh传递到Mesh Renderer。官方解释由于历史原因，Mesh Renderer无法像蒙皮网格一样直接使用Mesh，因此才有了Mesh Filter。

* UI.Graphic

  2019.3.11f版本中Graphic关键定义如下:

```C#
[DisallowMultipleComponent]
[RequireComponent(typeof(RectTransform))]
public abstract class Graphic : UIBehaviour, ICanvasElement
{
    [SerializeField] protected Material m_Material;// 材质
    [NonSerialized] private RectTransform m_RectTransform;// 绘制区域
    [NonSerialized] protected Mesh m_CachedMesh;// 顶点属性
    public virtual Texture mainTexture;// 主纹理

    public virtual void SetAllDirty()
    {
        …
        SetLayoutDirty();
        …
        SetMaterialDirty();
        …
        SetVerticesDirty();
    }

    protected override void OnEnable()
    {
        base.OnEnable();
        CacheCanvas();
        GraphicRegistry.RegisterGraphicForCanvas(canvas, this);

        if (s_WhiteTexture == null)
            s_WhiteTexture = Texture2D.whiteTexture;

        SetAllDirty();
    }


    protected virtual void OnPopulateMesh(Mesh m)
    {
        OnPopulateMesh(s_VertexHelper);
        s_VertexHelper.FillMesh(m);
    }

    protected virtual void OnPopulateMesh(VertexHelper vh)
    {
        var r = GetPixelAdjustedRect();
        var v = new Vector4(r.x, r.y, r.x + r.width, r.y + r.height);

        Color32 color32 = color;
        vh.Clear();
        vh.AddVert(new Vector3(v.x, v.y), color32, new Vector2(0f, 0f));
        vh.AddVert(new Vector3(v.x, v.w), color32, new Vector2(0f, 1f));
        vh.AddVert(new Vector3(v.z, v.w), color32, new Vector2(1f, 1f));
        vh.AddVert(new Vector3(v.z, v.y), color32, new Vector2(1f, 0f));

        vh.AddTriangle(0, 1, 2);
        vh.AddTriangle(2, 3, 0);
    }
}
```

* UI.Text

  UGUI处理文本显示使用Text组件，继承Graphic。对于字体图集的处理，通过key值记录文本在图集中uv坐标。在渲染文本时，只需要找到字体UV绘制到组件区域。

* SkinnedMeshRenderer

  蒙皮骨骼动画。动画关键帧存储骨骼数据，关键帧之间通过插值的方式计算骨骼位置；Mesh绑定到骨骼，根据骨骼位置动态调整顶点数据。（骨骼是一组具有父子关系的节点，可以理解为Unity的Transform）