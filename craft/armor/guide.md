---
layout    : craft-post
title     : User guide of Armor
date      : 2014-11-24
author    : mindwind
permalink : /craft/armor/guide/
---


## What is Armor?
[Armor](/craft/armor/) is an in-process service container for developers to build reliable and robust
java application.
[Armor](/craft/armor/) enables exported service of apps to be executed in isolated thread context.
As a result, different service execution flow does not interfere with each other.


## Background
When we develop applications usually focus on the functional requirements,
for some non-functional requirements like reliability, robustness, and isolation we
put less attention.These non-functional requirements make inexperienced developers
feel thorny. [Armor](/craft/armor/) makes it easy to develop high reliability and robust
applications.


## Architecture
How does an [Armor](/craft/armor/) based application look like?  
A Bird's Eye View as follows:

![](/images/armor-architecture.png)

For java application, any exported service API can be armed to provide more reliable
and robust servie. When a service consumer invoke the service API, [Armor](/craft/armor/) play
a role to create an `ArmorContext` for this invocation and execute it in the context.


### Armor Context
Execute in the `ArmorContext` means an isolated thread pool is employed to execute the invocation. We employ a isolated thread pool to control the execution process, like interrupt execution while timeout or fail fast under certain situations.

`ArmorContext` like a transaction context we are already familiar with, it is bound to
each armed service invocation. In the execution of invocation a new `ArmorContext` can
be created as a sub-context if it is needed. It has propagation property like transaction.


### Armor Proxy
[Armor](/craft/armor/) create an `ArmorProxy` for armed service. `ArmorProxy` put the invocation through an `ArmorFilter` chain, of course if no filter is configured, the proxy actually just transfer the invocation to raw service implementor.


### Armor Filter
[Armor](/craft/armor/) provides a series of filters to support different armed attributes.
You can choose filter carefully to get different armed effect.
The default filter list as follows:

  - [DegradeArmorFilter](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/DegradeArmorFilter.java)
  - [TransferArmorFilter](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/TransferArmorFilter.java)

Also you can customize your own filter chain to get most appropriate enhancement.


### Armor Listener
When armed service API be invoked, the whole life cycle of invocation is notified to `ArmorListener`.
It is a SPI interface to be implemented by application. For details please see
[java doc](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/spi/ArmorListener.java).


## Getting Started
By default [Armor](/craft/armor/) provide [spring](http://spring.io) pointcut to arm your application.
So if you application is not based on [spring](http://spring.io), you should implement your own
application container pointcut.


### Armor with spring pointcut
We implement a custom `BeanPostProcessor` named [ArmorBeanPostProcessor](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/ArmorBeanPostProcessor.java) to cut in the spring application initialization process.
You just only include [Armor](/craft/armor/) dependency and the spring configuration xml files as follows:

{% include_relative guide-code-spring.md %}

Then we need put an @Armor annotation on the service class.
The service class must implement at least one interface directly, and all the methods in its interface would be armed, like the bottom demo code:

{% include_relative guide-code-armor.md %}


### Using @Arm annotation for static configuration
@Arm is a method annotation for service implementor, it provides some partial armed
attributes which are appropriate for static configuration, more details please see [java doc](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/Arm.java).

Its common usage pattern as follows:
{% include_relative guide-code-arm.md %}

_NOTE_:

  - the `async` attribute relates only to `void` return type method, if the method has a return type don't set `async` be `true`.
  - In an armed method execution chain, the `async` attribute works only on the head of chain. This means a chain like `sync` + `async` = `sync` and `async` + `sync` = `async`, the first method of chain is the key point.



### Using ArmorService for dynamic configuration
[ArmorService](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/ArmorService.java) provides some APIs to modify and view the [Armor](#) running attributes.
We can get an [ArmorService](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/ArmorService.java) instance as follows:
{% include_relative guide-code-armorservice.md %}


## Performance
About the performance analysis of [Armor](/craft/armor/), please see [performance doc](/craft/armor/performance/).
