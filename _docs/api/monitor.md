---
category: API
redirect_from:
    - /docs/latest/api/monitor/
title: "自定义监控"
sort_title: "102"
flag: "2"
---

自定义监控： 用于对“筛选”出来的流量进行统计、监控。




### 1) 开启或关闭此插件

#### 请求方式

URI                 | Method
------------------- | ----
/monitor/enable     | Post

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
/monitor/config    | Get


#### 参数
无

#### 返回结果

```json
{
  "msg": "succeed to get configuration in this node",
  "data": {
    "enable": true,//插件是否启用
    "selectors": [//选择器列表
      {
        "enable": true,//选择器是否启用
        "rules": [//该选择器下的"规则"列表
          {
            "enable": true,//本条规则是否启用
            "id": "aa502670-e428-462e-a4ac-b41c74b88a14",
            "judge": {// "条件判断模块"配置
              "type": 0,// 见下文描述
              "conditions": [//条件列表
                {
                  "type": "URI",
                  "operator": "match",
                  "value": "^/monitor/"
                }
              ]
            },
            "time": "2017-02-18 13:15:20",//规则新建或更改时间
            "name": "monitor",// 规则名称
            "handle": {// "处理模块"配置
              "log": false,// 是否记录日志
              "continue": true// 匹配完该条规则后是否继续后续匹配
            }
          },
          {
            "enable": true, //本条规则是否启用
            "id": "D26E0C12-C687-4004-82C7-9AF258FE6470", //规则id
            "judge": { // "条件判断模块"配置
                "type": 3, // 见下文描述
                "expression": "(v[1] or v[2]) and v[3]", // 见下文描述
                "conditions": [// 见下文描述
                    {
                        "type": "URI",
                        "operator": "match",
                        "value": "/abc"
                    },{
                        "type": "Header",
                        "operator": "=",
                        "name": "uid",
                        "value": "123"
                    },{
                        "type": "Host",
                        "operator": "=",
                        "value": "127.0.0.1"
                    }
                ]
            },
            "time": "2016-05-04 18:57:23",//规则新建或更改时间
            "name": "/abc",// 规则名称
            "handle": { // "处理模块"配置
                "log": false, // 是否记录日志
                "continue": true // 匹配完该条规则后是否继续后续匹配
            }
           }
        ],
        "time": "2017-02-14 22:22:50",
        "handle": {
          "log": true,
          "continue": true
        },
        "type": 0,//0指该选择器是“全流量”选择器， 1指该选择器有一个“条件判断”模块，筛选出的流量才能进入此选择器
        "judge": [],//条件判断模块的配置
        "name": "全流量选择器",
        "id": "173250E2-9EA0-4AF9-BD2E-D400BF2C03A3"
      }
    ]
  },
  "success": true
}
```

- type: 0/1/2/3，0表示只有一个匹配条件，1表示对所有条件与操作，2表示对所有条件或操作，3表示按照另一个字段expression对所有条件求值
- expression: 当type为3时，存在此字段且不为空，它的格式是一个lua的逻辑判断表达式。表达式中每个值的格式为v[index], 比如v[1]对应的就是第一个条件的值。示例：(v[1] or v[2]) and v[3]，即前两个条件至少一个为真并且第三个条件为真时，规则为真。
conditions: 匹配条件集合
- conditions: [匹配条件](/docs/concept/condition)集合


### 3） 获取选择器信息


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/monitor/selectors    | Get    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

无

#### 返回结果

```json
{
  "data": {
    "enable": false, //该插件是否启用
    "meta": {// 该插件的一些基础配置
      "selectors": [ //该插件的所有”选择器“列表
        "173250E2-9EA0-4AF9-BD2E-D400BF2C03A3" //选择器id
      ]
    },
    "selectors": {//选择器详情
      "173250E2-9EA0-4AF9-BD2E-D400BF2C03A3": {
        "enable": true, //该选择器是否启用
        "rules": [ //该选择器下的所有规则id列表
          "aa502670-e428-462e-a4ac-b41c74b88a14"
        ],
        "time": "2017-02-14 22:22:50",
        "handle": { //该选择器的”处理模块“配置
          "log": true, //是否记录日志
          "continue": true //当该选择器退出时，是否继续执行后续选择器
        },
        "type": 0, //0指选择器是“全流量”选择器， 1指该选择器有一个“条件判断”模块,筛选出的流量才能进入此选择器
        "judge": [], //条件判断模块的配置， 这里因为是“全流量”选择器，故为空
        "name": "全流量选择器",
        "id": "173250E2-9EA0-4AF9-BD2E-D400BF2C03A3"//选择器id
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
/monitor/selectors    | Post    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector | string | 要创建的选择器的详细信息

示例（创建一个筛选出所有访问abc.com的流量， 则参数selector的值应为如下字符串）：

```json
{
    "name": "过滤abc.com流量",
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
/monitor/selectors    | Put    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


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
/monitor/selectors    | Delete    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


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
/monitor/selectors/:id/rules    | Post    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector_id | string | 此参数从URI里获取，即：id属性， 表示在该选择器下添加规则
rule | string | 该参数从form data表单里获取， 表示要添加的rule的字符串


示例（创建一个监控以user开头且含有某个header标识的流量）：

```json
{
    "name": "监控以/user开头且header里flag为1的流量",
    "judge": {
        "type": 1,
        "conditions": [
            {
                "type": "URI",
                "operator": "match",
                "value": "^/user"
            },
            {
                "type": "Header",
                "name": "flag",
                "operator": "=",
                "value": "1"
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
{"msg":"succeed to create rule","success":true}
```


### 8) 修改规则

#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/monitor/selectors/:id/rules  | Put    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector_id | string | 此参数从URI里获取，即：id属性， 表示要修改的规则位于该选择器下
rule | string | 该参数从form data表单里获取，指修改后的"规则"


修改上文创建的规则：

```json
{
    "name": "监控以/user/abc开头且header里flag为1的流量",
    "judge": {
        "type": 1,
        "conditions": [
            {
                "type": "URI",
                "operator": "match",
                "value": "^/user/abc"
            },
            {
                "type": "Header",
                "name": "flag",
                "operator": "=",
                "value": "1"
            }
        ]
    },
    "handle": {
        "continue": true,
        "log": false
    },
    "enable": true,
    "id": "db7b0cb0-5274-46f7-bd93-9f91a44b8f0b"
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
/monitor/selectors/:id/rules    | Delete    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8


#### 参数

名称 | 类型 | 说明
---- | ---- | -------
selector_id | string | 此参数从URI里获取，即：id属性， 表示要删除的规则位于该选择器下
rule_id | string | 指要删除的"规则"的id

#### 返回结果

```json
{"msg":"succeed to delete rule","success":true}
```


### 10) 获取满足某条规则的请求的统计信息

#### 请求方式

URI                 | Method
------------------- | ------
/monitor/stat       | Get


#### 参数

名称     | 类型   | 说明
----    | ----   | -------
rule_id | string | 指要获取统计数据的"规则"的id

#### 返回结果

```json
{
    "success": true,
    "data": {
        "average_traffic_read": 0, //请求平均读流量，bytes
        "request_2xx": 0,
        "average_traffix_write": 0, //请求平均写流量，bytes
        "request_4xx": 0,
        "request_5xx": 0,
        "traffic_read": 0,//读总流量，kb
        "request_3xx": 0,
        "traffic_write": 0,//写总流量，kb
        "total_request_time": 0, //总请求时间，s
        "average_request_time": 0, //平均响应时间，ms
        "total_count": 0 //总请求数
    }
}
```


### 11) 获取数据库中此插件的最新配置

#### 请求方式

URI                 | Method
------------------- | ------
/monitor/fetch_config | Get


#### 参数

无

#### 返回结果

```json
{
  "msg": "succeed to fetch config from store",
  "data": {
    "monitor.selector.173250E2-9EA0-4AF9-BD2E-D400BF2C03A3.rules": [
      {
        "enable": true,
        "handle": {
          "log": false,
          "continue": true
        },
        "id": "aa502670-e428-462e-a4ac-b41c74b88a14",
        "time": "2017-02-18 13:15:20",
        "name": "monitor",
        "judge": {
          "type": 0,
          "conditions": [
            {
              "type": "URI",
              "operator": "match",
              "value": "^/monitor/"
            }
          ]
        }
      }
    ],
    "monitor.selectors": {
      "173250E2-9EA0-4AF9-BD2E-D400BF2C03A3": {
        "enable": true,
        "id": "173250E2-9EA0-4AF9-BD2E-D400BF2C03A3",
        "judge": [],
        "handle": {
          "log": true,
          "continue": true
        },
        "type": 0,
        "time": "2017-02-14 22:22:50",
        "name": "全流量选择器",
        "rules": [
          "aa502670-e428-462e-a4ac-b41c74b88a14"
        ]
      }
    },
    "monitor.enable": false,
    "monitor.meta": {
      "selectors": [
        "173250E2-9EA0-4AF9-BD2E-D400BF2C03A3"
      ]
    }
  },
  "success": true
}
```



### 12) 将数据库中最新配置更新到此Orange节点


#### 请求方式

URI                 | Method | 说明
------------------- | ------ | -----
/monitor/sync       | Post    | Content-Type:application/x-www-form-urlencoded; charset=UTF-8

#### 参数

无

#### 返回结果

```json
{"msg":"succeed to load config from store","success":true}
```

