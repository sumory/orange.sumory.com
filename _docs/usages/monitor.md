---
category: Usages
redirect_from:
    - /docs/latest/usages/monitor/
title: "自定义监控插件使用示例"
sort_title: "201"
---

自定义监控： 用于对“筛选”出来的流量进行统计、监控。

当前的监控指标有：

- 请求总次数： 分别统计200/300/400/500区间的请求数
- QPS
- 请求总耗时/平均请求耗时
- 总流量/平均请求流量


#### 示例

监控`/user`开头的请求流量， 则配置规则如下：

![](/images/usages/monitor_rule.png)


查看监控：

![](/images/usages/monitor_stat.png)
