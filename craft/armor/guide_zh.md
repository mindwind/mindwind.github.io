---
layout    : craft-post-zh
title     : Armor 用户指南
date      : 2014-11-24
author    : mindwind
permalink : /craft/armor/guide/zh/
---


## Armor 是什么？
[Armor](/craft/armor/zh/) 是一个进程内的服务容器为开发者构建可靠和健壮的 java 应用提供帮助。
[Armor](/craft/armor/zh/) 使应用暴露的服务执行在相互隔离的线程上线文中，因此不同的服务执行流程并不会造成相互干扰。


## 背景
当我们开发应用时通常关注功能需求，而对于非功能需求像可靠性、健壮性和隔离性我们甚少关注。
这些非功能需求让缺乏经验的开发者感到恐惧。[Armor](/craft/armor/zh/) 则让开发高可靠和健壮的应用变得简单。


## 架构
基于 [Armor](/craft/armor/zh/) 增强的应用看起来是怎样的呢？
一个鸟瞰的全景视图如下：

![](/images/armor-architecture.png)

对于 java 应用，任何暴露的服务 API 都能被增强以提供更可靠和健壮的服务。
当一个服务消费者调用服务时，[Armor](/craft/armor/zh/) 发挥作用了，它为本次调用创建了一个 `ArmorContext`（Armor 上下文）
并且在上线文环境中执行调用。


### Armor 上下文
在 `ArmorContext`（Armor 上下文）中执行意味着使用了一个隔离的线程池来执行本次调用。
我们使用隔离的线程池来控制调用执行过程，例如像超时时中断执行或者在一些特定情况下地快速失败。

Armor 上下文就像我们熟悉的事务上下文，它与每一个增强的服务调用绑定在一起。
在调用执行过程中假如需要的话一个新的 Armor 上下文可以被创建作为子上线文存在。
它拥有和事务类似的传播属性。


### Armor 代理
对于增加的服务 [Armor](/craft/armor/zh/) 会为其创建代理（`ArmorProxy`）。
代理将调用放在一个过滤器链上来执行，当然如果没有过滤器被配置，代理实际上仅仅是将调用转交给原始的服务实现者。


### Armor 过滤器
[Armor](/craft/armor/zh/) 提供了一系列过滤器来支持不同的属性增强。
你能通过仔细挑选过滤器来得到不同的增强效果。  
默认的过滤器列表如下：

- [DegradeArmorFilter](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/DegradeArmorFilter.java)
- [TransferArmorFilter](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/TransferArmorFilter.java)

当然你也可以定制自己的过滤器链以获得最适当的提升。


### Armor 监听器
当 Armor 增强的服务 API 被调用时，整个调用的生命周期会通知到 `ArmorListener`。
这是一个由应用实现的 SPI 接口。详细情况请参见 [java doc](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/spi/ArmorListener.java)。


## 入门
默认 [Armor](/craft/armor/zh/) 提供基于 [spring](http://spring.io) 的切入点来增强应用。
所以如果你的应用不是基于 [spring](http://spring.io) 的那么你需要实现你的应用特定的容器切入点。


### Armor 通过 spring 切入
我们实现了一个 `BeanPostProcessor` 命名为 [ArmorBeanPostProcessor](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/ArmorBeanPostProcessor.java) 来切入到 spring 应用的初始化过程中。
你只需要包含 [Armor](/craft/armor/zh/) 的库依赖并配置一个如下所示的 spring 配置文件：

{% include_relative guide-code-spring.md %}

然后我们需要在服务类上放一个 @Armor 注解。
服务类必须直接实现至少一个接口，所有接口中的方法将被 Armor 增强，如下示例代码：

{% include_relative guide-code-armor.md %}


### 使用 @Arm 注解进行静态配置
@Arm 是一个用于服务实现类的方法级注解，它提供了部分适合于静态配置的增强属性，更多细节请阅读 [java doc](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/Arm.java).

通用用法如下所示：
{% include_relative guide-code-arm.md %}

注意：
  - `async` 属性仅适用于 `void` 返回类型的方法，假如方法声明了返回值千万别设置 `async` 属性为 `true`
  - 在一个 Armor 增强的方法执行链中，`async` 属性仅在执行链的首位发挥作用。这意味着一个像这样的执行链
    `sync` + `async` = `sync` 和 `async` + `sync` = `async` 首位方法是关键点。


### 使用 ArmorService 进行动态配置
[ArmorService](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/ArmorService.java) 提供了一些 API 来修改和观察 [Armor](/armor/zh/) 的运行时属性。

我们可以通过如下方法获取一个 [ArmorService](https://github.com/mindwind/craft-armor/blob/master/src/main/java/io/craft/armor/api/ArmorService.java) 的实例：

{% include_relative guide-code-armorservice.md %}


## 性能
关于 [Armor](/craft/armor/zh/) 的性能分析，请参见 [性能文档](/craft/armor/performance/zh/)
