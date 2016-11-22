---
layout    : craft-post
title     : craft-atom-lock
date      : 2014-10-31
author    : mindwind
permalink : /craft/atom/lock/
---


## Introduction
A kind of distributed lock base on [redis](http://redis.io).


## Getting Started
A typical usage idiom for this component would be:
{% include_relative lock-code-1.md %}


How to get a `DLock` instance?
{% include_relative lock-code-2.md %}
