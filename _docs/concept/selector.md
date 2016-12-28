---
category: Concept
redirect_from:
    - /docs/latest/concept/selector/
title: "选择器"
sort_title: "1"
---


**选择器selector** ，它可能包括多个[rule](/docs/concept/rule/)，即是一个`rule group`, 选择器内置了一个条件筛选模块（这个模块的概念和使用方法等同于rule里的[judge](/docs/concept/judge/)模块），可以将流量进行初步划分。

在多个插件里（比如URL重定向插件、WAF插件、自定义监控插件）都使用了选择器来将流量进行第一步划分， 在流量被一个选择器命中后才会进入它内部的规则过滤。

使用选择器的目的是减少不必要的规则判断，进一步提高性能。


