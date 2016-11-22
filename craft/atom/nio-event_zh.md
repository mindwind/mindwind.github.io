---
layout    : craft-post-zh
title     : atom-nio 事件模型说明
date      : 2014-10-28
author    : mindwind
permalink : /craft/atom/nio/event/zh/
---


[atom-nio](/craft/atom/nio/zh/)
是基于事件驱动的并且每个事件关联到一个 Channel， 那么我们有多少种类型的事件呢？
下表种列出了所有的事件：

1. *CHANNEL_OPENED*  
   `当 channel 被打开后，触发该事件。`  
   ![CHANNEL_OPENED](/images/doc-craft-atom-nio-event-model-1.png)

2. *CHANNEL_CLOSED*  
   `当 channel 被关闭后，触发该事件。`  
   ![CHANNEL_CLOSED](/images/doc-craft-atom-nio-event-model-2.png)

3. *CHANNEL_READ*  
   `当从 channel 读取到一些数据时，触发该事件。`  
   ![CHANNEL_READ](/images/doc-craft-atom-nio-event-model-3.png)

4. *CHANNEL_FLUSH*  
   `当将从 channel flush 出数据（数据被写入网络缓冲区之前），触发该事件。`  
   ![CHANNEL_FLUSH](/images/doc-craft-atom-nio-event-model-4.png)

5. *CHANNEL_WRITTEN*  
   `当数据已被写入网络缓冲区后，触发该事件。`  
   ![CHANNEL_WRITTEN](/images/doc-craft-atom-nio-event-model-5.png)

6. *CHANNEL_IDLE*  
   `当一段时间内 channel 没有数据传输时，触发该事件。`  
   ![CHANNEL_IDLE](/images/doc-craft-atom-nio-event-model-6.png)  

7. *CHANNEL_THROWN*  
   `当读写 channel 发生异常时，触发该事件。`  
   ![CHANNEL_THROWN](/images/doc-craft-atom-nio-event-model-7.png)


## 事件分发
如上所述，当某些事情发生时就会触发一个事件。 这些事件必须被 `IoHandler` 来处理，
`IoHandler` 作为一个服务提供者（SPI）接口需要由组件使用者来实现。
事件分发行为就是说将事件传递给 `IoHandler`。

如何分发呢？ SPI 接口 `NioChannelEventDispatcher` 提供了该功能，默认我们提供了 2 种事件分发器的实现。

1. `NioOrderedDirectChannelEventDispatcher` 直接在当前 I/O 线程分发事件。
2. `NioOrderedThreadPoolChannelEventDispatcher` 使用一个新的线程池来分发事件。
