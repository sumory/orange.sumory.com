---
category: API
redirect_from:
    - /docs/latest/api/waf/
title: "WAF防火墙"
sort_title: "110"
flag: "2"
---

WAF防火墙: 对筛选出的流量做访问控制， 如禁止某种类型的访问。

选择器和规则相关的API使用方法同[URL重定向API](/docs/api/redirect/)， 只是**规则**的字段有所差异。


### 获取防火墙统计信息

#### 请求方式

URI                 | Method
------------------- | ------
/waf/stat           | Get


#### 参数

无

#### 返回结果

```json
{
    "data": {
        "statistics": [{
            "count": 7, //命中规则的请求个数
            "rule_id": "C4DDC90B-F05C-4F69-94E1-6FCBC4F88392",// 规则id
        }]
    },
    "success": true
}
```
