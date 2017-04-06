---
category: API
redirect_from:
    - /docs/latest/api/stat/
title: "全局统计"
sort_title: "101"
flag: "2"
---


全局统计插件API： 用于获取当前Orange节点的运行状态， 如工作路径、程序版本、启动时间、总请求数、连接数等等。

当前默认为开启状态，暂不提供关闭功能。


#### 请求方式

URI                 | Method
------------------- | ----
/stat/status        | Get


#### 参数

无

#### 返回结果

```javascript
{
  "data": {
    "total_count": 16194, //总请求数
    "worker_count": 4,
    "con_reading": "0",
    "request_4xx": 16194,
    "ngx_lua_version": "0.10.6",
    "request_3xx": 0,
    "nginx_version": "1.11.2",
    "total_success_count": 0,
    "load_timestamp": 1491468071,
    "con_writing": "1",
    "request_2xx": 0,
    "ngx_prefix": "/usr/local/orange/", //工作路径
    "con_rw": 1,
    "con_active": "2",
    "address": "127.0.0.1",
    "traffic_write": 4611705,
    "traffic_read": 615746,
    "start_time": 1491468071,
    "timestamp": 1491469927,
    "con_idle": "1",
    "request_5xx": 0,
    "total_request_time": 94 //总请求耗时，秒
  },
  "success": true
}
```

