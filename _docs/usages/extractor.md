---
category: Usages
redirect_from:
    - /docs/latest/usages/judge/
title: "变量提取模块的使用"
sort_title: "102"
---

[变量提取模块](/docs/concept/extractor/)是多个插件中都使用到的一个模块， 它主要用来从请求中提取出各种信息， 比如query string, form表单里的字段， 某个header头等等。

它支持两种提取方式：

- 索引式提取
- 模板式提取

关于变量提取方式的设计可参看[issues#15](https://github.com/sumory/orange/issues/15)。


### 索引式提取

顾名思义， 索引式提取的含义是将提取出的所有变量按照顺序保存在一个数组中， 后续以下标的方式来使用。


比如我们在"变量提取模块"提取了三个值：

![](/images/usages/extractor_1.png)

那么之后就可以通过${1}、${2}、${3}来使用， 其中

- ${1}指的是header头里的app_version字段， 如果header没有此字段， 则赋一个默认值`v0.1`
- ${2}指的是query中的uid字段
- ${3}指的是query中age字段， 若无则给予默认值18

### 模板式提取

模板时提取主要为了解决索引式提取必须要按序使用的问题， 并且当需要从uri中提取多个值时索引式提取方式并不友好。


如以下示例， 我们提取了四个值：

![](/images/usages/extractor_2.png)

则之后我们可以通过以下方式来使用：

- \{\{query.uid\}\}指的是从query中提取出的uid字段
- \{\{query.age\}\}指的是从query中提取出的age字段, 若无则给予默认值18
- \{\{uri.v1\}\}指的是从格式为`^/start/(.*)/(.*)/end`的URI中提取出的第1个分段值
    - 比如， 如果URI为/start/abc/123/end, 则此时值为abc
    - 如果URI为/start/momo/sharp/end, 则此时值为momo
- \{\{uri.v2\}\}指的是从格式为`^/start/(.*)/(.*)/end`的URI中提取出的第2个分段值
    - 比如， 若URI为/start/abc/123/end, 则此时值为123
    - 如果URI为/start/momo/sharp/end, 则此时值为sharp

注意， 若从URI中提取， 仍然要根据顺序来使用， 如\{\{uri.v1\}\}、\{\{uri.v2\}\}、\{\{uri.v3\}\}.
