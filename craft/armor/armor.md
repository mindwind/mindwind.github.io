---
layout    : craft-post
title     : Armor
date      : 2015-03-18
author    : mindwind
permalink : /craft/armor/
---


## Usage
Using maven import.

{% include_relative armor-code-maven.md %}

More details please see [user guide](/craft/armor/guide/).


## Capability
[Armor](#) is an in-process service container to arm your application and enhance it’s robustness.
It provides a series of armed attributes to enhance the application's robustness.
The attributes list as follows:

  - Service thread isolation  
    `Service has isolated thread to execute.`
  - Service timeout control  
    `Service has timeout configuration and recycle execution thread fast.`
  - Service degradation  
    `Service can be degraded to fail fast.`
  - Service interface switch  
    `Servie invocation can be switched to backup service provider.`
  - Service monitoring  
    `Service can be monitored to know the execution time elapse.`
  - Service asynchronous execution  
    `Service with synchronous invocation can be converted to asynchronous execution.`


## Contract
  - Only support service class which must implement an interface.  
    `Because armor using JDK dynamic proxy to implement.`
  - Service thread isolation works restricted by two conditions.  
    `1. Service method must be all armed not partial.`  
    `2. All service isolated thread pool size be summed up is less than total thread pool size.`
  - Service asynchronous execution only works for void return method.  
    `Pay attention to the side effect of asynchronous execution.`


## Version
  - 1.1.2  
    `Fix bug, in async execution mode, the ArmorListener error callback missing.`
  - 1.1.1  
    `Fix bug, in async execution mode, the ArmorListener callback order is wrong.`
  - 1.1.0  
    `Add feature, new async execution mode and @Arm annotation.`
  - 1.0.0  
    `First release version for basic features.`
