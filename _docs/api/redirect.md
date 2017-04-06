---
category: API
redirect_from:
    - /docs/latest/api/redirect/
title: "URL重定向"
sort_title: "103"
flag: "2"
---


URL重定向： 将请求redirect到其它网址


### 1) 开启或关闭此插件

#### 请求方式

URI                 | Method
------------------- | ----
/redirect/enable     | Post

#### 参数

名称 | 类型 | 说明
---- | ---- | -------
enable | int | 0关闭1开启


#### 返回结果

```json
{
    "msg":"succeed to disable plugin",
    "success":true
}
```

### 2) 获取所有配置信息

#### 请求方式

URI                 | Method
------------------- | ----
/redirect/config    | Get


#### 参数
无

#### 返回结果

```json
{
  "msg": "succeed to get configuration in this node",
  "data": {
    "enable": true,
    "selectors": [
      {
        "enable": false,
        "id": "1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD",
        "time": "2016-11-13 17:14:32",
        "handle": {
          "log": false,
          "continue": true
        },
        "type": 0,
        "judge": [],
        "name": "全流量选择器",
        "rules": [
          {
            "enable": true,
            "handle": {
              "trim_qs": true,
              "redirect_status": "301",
              "url_tmpl": "https://www.baidu.com/s?wd={{query.search}}",
              "log": true
            },
            "id": "a6451360-efab-41e1-968b-dc95fb479c39",
            "judge": {
              "type": 2,
              "conditions": [
                {
                  "type": "URI",
                  "operator": "match",
                  "value": "/to_baidu"
                },
                {
                  "type": "Query",
                  "operator": "=",
                  "name": "to_baidu",
                  "value": "1"
                }
              ]
            },
            "time": "2017-04-06 19:28:43",
            "name": "将/to_baidu重定向到baidu.com",
            "extractor": {
              "type": 2,
              "extractions": [
                {
                  "type": "Query",
                  "name": "search"
                }
              ]
            }
          }
        ]
      }
    ]
  },
  "success": true
}
```



### 3） 获取选择器信息


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/selectors    | Get    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

无

#### 返回结果

```json
{
  "data": {
    "enable": true,
    "meta": {
      "selectors": [
        "1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD"
      ]
    },
    "selectors": {
      "1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD": {
        "enable": false,
        "id": "1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD",
        "time": "2016-11-13 17:14:32",
        "handle": {
          "log": false,
          "continue": true
        },
        "type": 0,
        "judge": [],
        "name": "全流量选择器",
        "rules": [
          "a6451360-efab-41e1-968b-dc95fb479c39"
        ]
      }
    }
  },
  "success": true
}
```

### 4） 创建选择器


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/selectors    | Post    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector | string | 要创建的选择器的详细信息

示例（创建一个筛选出所有访问abc.com的流量， 则参数selector的值应为如下字符串）：

```json
{
    "name": "筛选abc.com的流量",
    "type": 1,
    "judge": {
        "type": 0,
        "conditions": [
            {
                "type": "Host",
                "operator": "=",
                "value": "abc.com"
            }
        ]
    },
    "handle": {
        "continue": true,
        "log": false
    },
    "enable": true
}
```

#### 返回结果

```json
{"msg":"succeed to create selector","success":true} //失败时success为false
```


### 5） 修改选择器信息


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/selectors    | Put    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector | string | 要修改后的选择器的详细信息

示例（我们把上文中创建的选择器做下修改，添加一个过滤条件， 参数selector的值应为如下字符串）：

```json
{
    "name": "过滤abc.com流量下URI以/user开始的流量",
    "type": 1,
    "judge": {
        "type": 1,
        "conditions": [
            {
                "type": "Host",
                "operator": "=",
                "value": "abc.com"
            },
            {
                "type": "URI",
                "operator": "match",
                "value": "^/user"
            }
        ]
    },
    "handle": {
        "continue": true,
        "log": false
    },
    "enable": true,
    "id": "b7af8982-11a4-44f9-8777-8a35ef56fa75"
}
```

#### 返回结果

```json
{"msg":"succeed to update selector","success":true} //失败时success为false
```


### 6) 删除选择器


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/selectors    | Delete    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector_id | string | 要删除的选择器id


#### 返回结果

```json
{
    "success": true,
    "msg": "succeed to delete selector"
}
```

### 7) 添加规则


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/selectors/:id/rules    | Post    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector_id | string | 此参数从URI里获取，即：id属性， 表示在该选择器下添加规则
rule | string | 该参数从form data表单里获取， 表示要添加的rule的字符串


示例（将/to_baidu重定向到baidu.com）：

```json
{
    "name": "将/to_baidu重定向到baidu.com",
    "judge": {
        "type": 0,
        "conditions": [
            {
                "type": "URI",
                "operator": "match",
                "value": "/to_baidu"
            }
        ]
    },
    "extractor": {
        "type": 2,
        "extractions": [
            {
                "type": "Query",
                "name": "search"
            }
        ]
    },
    "handle": {
        "url_tmpl": "https://www.baidu.com/s?wd={{query.search}}",
        "trim_qs": true,
        "redirect_status": "301",
        "log": true
    },
    "enable": true
}
```

#### 返回结果

```json
{"msg":"succeed to create rule","success":true}
```


### 8) 修改规则

#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/selectors/:id/rules  | Put    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector_id | string | 此参数从URI里获取，即：id属性， 表示要修改的规则位于该选择器下
rule | string | 该参数从form data表单里获取，指修改后的"规则"


修改上文创建的规则, 增加一个判断条件，当这两个条件任一个为真是则跳转到baidu.com：

```json
{
    "name": "将/to_baidu重定向到baidu.com",
    "judge": {
        "type": 2,
        "conditions": [
            {
                "type": "URI",
                "operator": "match",
                "value": "/to_baidu"
            },
            {
                "type": "Query",
                "name": "to_baidu",
                "operator": "=",
                "value": "1"
            }
        ]
    },
    "extractor": {
        "type": 2,
        "extractions": [
            {
                "type": "Query",
                "name": "search"
            }
        ]
    },
    "handle": {
        "url_tmpl": "https://www.baidu.com/s?wd={{query.search}}",
        "trim_qs": true,
        "redirect_status": "301",
        "log": true
    },
    "enable": true,
    "id": "a6451360-efab-41e1-968b-dc95fb479c39"
}
```

#### 返回结果

```json
{"msg":"ok","success":true}
```

### 9) 删除规则

#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/selectors/:id/rules    | Delete    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector_id | string | 此参数从URI里获取，即：id属性， 表示要删除的规则位于该选择器下
rule_id | string | 指要删除的"规则"的id

#### 返回结果

```json
{"msg":"succeed to delete rule","success":true}
```



### 10) 获取数据库中此插件的最新配置

#### 请求方式

URI                 | Method
------------------- | ------
/redirect/fetch_config | Get


#### 参数

无

#### 返回结果

```json
{
    "msg": "succeed to fetch config from store",
    "data": {
        "redirect.meta": {
            "selectors": [
                "1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD"
            ]
        },
        "redirect.enable": true,
        "redirect.selectors": {
            "1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD": {
                "enable": false,
                "judge": [],
                "rules": [
                    "a6451360-efab-41e1-968b-dc95fb479c39"
                ],
                "handle": {
                    "log": false,
                    "continue": true
                },
                "type": 0,
                "time": "2016-11-13 17:14:32",
                "name": "全流量选择器",
                "id": "1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD"
            }
        },
        "redirect.selector.1C4A9916-B3BD-4E95-8108-3FBCFF08BEBD.rules": [
            {
                "enable": true,
                "handle": {
                    "trim_qs": true,
                    "redirect_status": "301",
                    "url_tmpl": "https://www.baidu.com/s?wd={{query.search}}",
                    "log": true
                },
                "id": "a6451360-efab-41e1-968b-dc95fb479c39",
                "judge": {
                    "type": 2,
                    "conditions": [
                        {
                            "type": "URI",
                            "operator": "match",
                            "value": "/to_baidu"
                        },
                        {
                            "type": "Query",
                            "operator": "=",
                            "name": "to_baidu",
                            "value": "1"
                        }
                    ]
                },
                "time": "2017-04-06 19:28:43",
                "name": "将/to_baidu重定向到baidu.com",
                "extractor": {
                    "type": 2,
                    "extractions": [
                        {
                            "type": "Query",
                            "name": "search"
                        }
                    ]
                }
            }
        ]
    },
    "success": true
}
```



### 11) 将数据库中最新配置更新到此Orange节点


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/redirect/sync       | Post    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8

#### 参数

无

#### 返回结果

```json
{"msg":"succeed to load config from store","success":true}
```

