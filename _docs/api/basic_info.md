---
category: API
redirect_from:
    - /docs/latest/api/basic_info/
title: "基本信息"
sort_title: "1"
flag: "1"
---




### 说明

- API Server默认在7777端口监听
- 若无特别说明，API均以HTTP协议给出，返回值为json格式，数据格式如下:

```
#失败情况
{
    success: false,
    msg: "错误描述"
}

#成功情况
{
    success: true,
    msg: "描述信息",
    data: {}//相关数据
}
```

### 插件基本信息API

URI                 | Method 
------------------- | ---- 
/plugins            | Get 

**参数** 
无 

**返回结果**  

```
{
    "success": true,
    "data": {
        "plugins": {//可用插件
            "monitor": {//key为插件名称
                "enable": true,//插件是否启用
                "inactive_rule_count": 0,//被关闭的规则数
                "name": "monitor",//插件名称
                "active_rule_count": 1//被开启的规则数
            },
            "stat": {
                "enable": true,//stat是全局插件，默认开启，不能关闭
                "inactive_rule_count": 0,
                "name": "stat",
                "active_rule_count": 0
            },
            "redirect": {//URL重定向插件
                "enable": true,
                "inactive_rule_count": 0,
                "name": "redirect",
                "active_rule_count": 2
            },
            "rewrite": {//URI重写插件
                "enable": false,
                "inactive_rule_count": 0,
                "name": "rewrite",
                "active_rule_count": 0
            },
            "rate_limiting": {//访问限速插件
                "enable": true,
                "inactive_rule_count": 0,
                "name": "rate_limiting",
                "active_rule_count": 0
            },
            "basic_auth": {//HTTP Basic Auth插件
                "enable": true,
                "inactive_rule_count": 0,
                "name": "basic_auth",
                "active_rule_count": 0
            },
            "key_auth": {//HTTP Key Auth插件
                "enable": true,
                "inactive_rule_count": 0,
                "name": "key_auth",
                "active_rule_count": 0
            },
            "waf": {//WAF插件
                "enable": true,
                "inactive_rule_count": 0,
                "name": "waf",
                "active_rule_count": 0
            },
            "divide": {//分流插件
                "enable": true,
                "inactive_rule_count": 0,
                "name": "divide",
                "active_rule_count": 1
            }
        }
    }
}
```

