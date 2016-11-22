---
layout    : craft-post-zh
title     : atom-io
date      : 2014-10-24
author    : mindwind
permalink : /craft/atom/io/zh/
---


## 简介
一种 reactor I/O 模型 API 定义，其他组件例如 [atom-nio](/craft/atom/nio/) 实现该 API。

## Reactor I/O 模型
Reactor I/O 模型架构如图1所示。  
```Channel``` 是 I/O 操作的枢纽。  
```IoHandler``` 负责处理 I/O 事件。  
```IoAcceptor``` 负责接入到达的 I/O 请求。  
```IoProcessor``` 负责处理 I/O 读写。  
```IoConnector``` 负责连接服务端。  
```ChannelEvent``` 是与某个 ```Channel``` 关联的事件。  

![I/O model architect](/images/doc-craft-atom-io-reactor-model.png)

## 实现者
目前只有一个实现者如下：

  * [atom-nio](/craft/atom/nio/zh/)
