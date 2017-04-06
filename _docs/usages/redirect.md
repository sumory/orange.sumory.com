---
category: Usages
redirect_from:
    - /docs/latest/usages/redirect/
title: "URL重定向插件使用示例"
sort_title: "202"
---


URL重定向主要用于将访问站A的请求redirect到站B， 比较典型的应用是代理某些API。



#### 示例


比如我们要将访问`/to_baidu`的请求代理到baidu.com， 可如下配置

![](/images/usages/redirect_1.png)

之后访问`/to_baidu?search=abc`就会被重定向到`https://www.baidu.com/s?wd=abcd`。

关于图中"规则"的配置请阅读[条件判断模块的使用](/docs/usages/judge/)， 关于"处理"时参数值的使用请阅读[变量提取模块的使用](/docs/usages/extractor/)。


要**特别**说明的是：

![](/images/usages/redirect_2.png)

对于红线标出的值(即要redirect到的地址)需要是一个<span style="color:red">完整的URL</span>（带域名）， 而**不能是**URI。 若对此概念有混淆感，请首先搞清楚`redirect`和`rewrite`的区别。
