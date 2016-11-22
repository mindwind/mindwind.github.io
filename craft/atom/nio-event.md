---
layout    : craft-post
title     : Event Model Illustration of atom-nio
date      : 2014-10-28
author    : mindwind
permalink : /craft/atom/nio/event/
---


[atom-nio](/craft/atom/nio/) is base on event-driven
and each event is related to a `Channel`, so how many type of event we have?
All the events are illustrated in the table below:

1. *CHANNEL_OPENED*
   `When channel has been opened, fire this event.`  
   ![CHANNEL_OPENED](/images/doc-craft-atom-nio-event-model-1.png)

2. *CHANNEL_CLOSED*  
   `When channel has been closed, fire this event.`  
   ![CHANNEL_CLOSED](/images/doc-craft-atom-nio-event-model-2.png)

3. *CHANNEL_READ*  
   `When channel has read some data, fire this event.`  
   ![CHANNEL_READ](/images/doc-craft-atom-nio-event-model-3.png)

4. *CHANNEL_FLUSH*  
   `When channel would flush out data in it, fire this event.`  
   ![CHANNEL_FLUSH](/images/doc-craft-atom-nio-event-model-4.png)

5. *CHANNEL_WRITTEN*  
   `When channel has written some data, fire this event.`  
   ![CHANNEL_WRITTEN](/images/doc-craft-atom-nio-event-model-5.png)

6. *CHANNEL_IDLE*  
   `When channel has no data transmit for a while, fire this event.`  
   ![CHANNEL_IDLE](/images/doc-craft-atom-nio-event-model-6.png)  

7. *CHANNEL_THROWN*  
   `When channel operation throw exception, fire this event.`  
   ![CHANNEL_THROWN](/images/doc-craft-atom-nio-event-model-7.png)


## Dispatch Event
As mentioned above, an event would be fired when something occurs.
It must be handle by the `IoHandler`, the `IoHandler` is a SPI interface which
should be implemented by the component user.
The action event dispatch means hand over the event to `IoHandler`.

How to dispatch? A SPI interface `NioChannelEventDispatcher` provide this function,
and by default we have two kinds of event dispatcher implementor.

1. `NioOrderedDirectChannelEventDispatcher` Dispatch event on current I/O thread.
2. `NioOrderedThreadPoolChannelEventDispatcher` Dispatch event on a new thread pool.
