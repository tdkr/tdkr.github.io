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

* ILRuntime
* xLua
* HybridCLR