---
category: API
redirect_from:
    - /docs/latest/api/basic_info/
title: "基本信息"
sort_title: "0"
flag: "1"
---



### 说明

- API Server默认在7777端口监听
- 若无特别说明，API均以HTTP协议给出，返回值为json格式，数据格式如下:

```javascript
//失败情况
{
    success: false,
    msg: "错误描述"
}

//成功情况
{
    success: true,
    msg: "描述信息",
    data: {} //相关数据
}
```

### 插件基本信息API

#### 请求方式

URI                 | Method
------------------- | ----
/plugins            | Get

#### 参数

无

#### 返回结果

```javascript
{
  "success": true, //API请求是否成功
  "data": {
    "plugins": {
      "monitor": {//插件名称
        "enable": true, //是否开启
        "active_selector_count": 1, //开启的选择器数目
        "inactive_selector_count": 0, //关闭的选择器数目
        "active_rule_count": 1, //开启的规则数目
        "inactive_rule_count": 0, //开启的规则数目
        "name": "monitor" // 插件名称
      },
      "rate_limiting": {
        "enable": true,
        "active_selector_count": 1,
        "inactive_selector_count": 0,
        "inactive_rule_count": 0,
        "name": "rate_limiting",
        "active_rule_count": 1
      },
      "key_auth": {
        "enable": false,
        "active_selector_count": 1,
        "inactive_selector_count": 0,
        "inactive_rule_count": 0,
        "name": "key_auth",
        "active_rule_count": 2
      },
      "basic_auth": {
        "enable": true,
        "active_selector_count": 1,
        "inactive_selector_count": 0,
        "inactive_rule_count": 0,
        "name": "basic_auth",
        "active_rule_count": 2
      },
      "kvstore": {
        "enable": true,
        "name": "kvstore"
      },
      "divide": {
        "enable": true,
        "active_selector_count": 1,
        "inactive_selector_count": 1,
        "inactive_rule_count": 0,
        "name": "divide",
        "active_rule_count": 0
      },
      "stat": {
        "active_selector_count": 0,
        "inactive_selector_count": 0,
        "inactive_rule_count": 0,
        "name": "stat",
        "active_rule_count": 0
      },
      "waf": {
        "enable": true,
        "active_selector_count": 0,
        "inactive_selector_count": 2,
        "inactive_rule_count": 0,
        "name": "waf",
        "active_rule_count": 0
      },
      "property_rate_limiting": {
        "enable": false,
        "active_selector_count": 1,
        "inactive_selector_count": 0,
        "inactive_rule_count": 0,
        "name": "property_rate_limiting",
        "active_rule_count": 0
      },
      "rewrite": {
        "enable": false,
        "active_selector_count": 1,
        "inactive_selector_count": 0,
        "inactive_rule_count": 0,
        "name": "rewrite",
        "active_rule_count": 2
      },
      "signature_auth": {
        "enable": false,
        "active_selector_count": 0,
        "inactive_selector_count": 0,
        "inactive_rule_count": 0,
        "name": "signature_auth",
        "active_rule_count": 0
      },
      "redirect": {
        "enable": true,
        "active_selector_count": 0,
        "inactive_selector_count": 1,
        "inactive_rule_count": 0,
        "name": "redirect",
        "active_rule_count": 0
      }
    }
  }
}
```

