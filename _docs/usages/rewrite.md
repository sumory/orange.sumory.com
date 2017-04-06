---
category: Usages
redirect_from:
    - /docs/latest/usages/rewrite/
title: "URI重写插件使用示例"
sort_title: "203"
---


URI重写主要用于站内请求的重写，比如将`/user/create`重写为`/u/new`， 当时用浏览器访问被重写的URI时， 浏览器地址栏的URL并不会发生变化， 请特别注意它与[URL重定向插件](/docs/usages/redirect/)的区别。


#### 示例


比如我们要将访问`/rewrite/`的请求重写成访问`/to_rewrite/`的请求， 可如下配置

![](/images/usages/rewrite1.png)

之后访问`/rewrite?a=123`时， 真正访问的是`/to_rewrite/123`。

关于图中"规则"的配置请阅读[条件判断模块的使用](/docs/usages/judge/)， 关于"变量"、"处理"时参数值的使用请阅读[变量提取模块的使用](/docs/usages/extractor/)。


要**特别**说明的是：

![](/images/usages/rewrite2.png)

对于红线标出的值(即要rewrite到的地址)需要是一个<span style="color:red">URI</span>， 而**不能是**URL。 请特别注意它与"重定向"的区别。
