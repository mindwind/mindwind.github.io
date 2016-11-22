---
layout    : craft-post
title     : atom-io
date      : 2014-10-24
author    : mindwind
permalink : /craft/atom/io/
---


## Introduction
A reactor I/O API model definition, other component,
like [atom-nio](/craft/atom/nio/) implements it.

## Reactor I/O Model
Reactor io model architect as shown in the figure 1.  
```Channel``` is a nexus for io operations.  
```IoHandler``` handles I/O events.  
```IoAcceptor``` accepts I/O incoming request.  
```IoProcessor``` process I/O read and write.  
```IoConnector``` connects to server.  
```ChannelEvent``` is an I/O event associated with a ```Channel```.  

![I/O model architect](/images/doc-craft-atom-io-reactor-model.png)

## Implementor
Now we have one implementor as follows:  

  * [atom-nio](/craft/atom/nio/)
