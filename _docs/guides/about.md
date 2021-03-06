---
category: Guides
redirect_from:
    - /docs/latest/guides/about/
title: "关于Orange"
sort_title: "1"
---

欢迎使用Orange，使用过程中如碰到问题，请到[Github](https://github.com/sumory/orange/issues)进行提问。

关于Orange的公开分享请参看[OpenResty Con 2016](http://con.openresty.org/cn/2016/)。


### 关于

Orange是一个基于OpenResty的API Gateway，提供API及自定义[规则](/docs/rule.html)的监控和管理，如访问统计、流量切分、API重定向、API鉴权、WEB防火墙等功能。Orange可用来替代前置机中广泛使用的Nginx/OpenResty， 在应用服务上无痛前置一个功能丰富的网关系统。它有以下特性：

- 动态更新Nginx/OpenResty配置而无需重启或reload
- 通过MySQL存储来简单支持集群部署
- 支持多种[条件匹配和变量提取](/docs/expression.html)
- 支持通过自定义插件方式扩展功能
- 内置多个通用插件
    - 全局状态统计
    - 自定义监控
    - URL重写
    - URI重定向
    - 访问限速
    - Key based rate limiting
    - HTTP Basic Auth
    - HTTP Key Auth
    - Signature Auth
    - 简单防火墙WAF
    - 代理、ABTesting、分流
    - Shared Dict存取接口
- 提供Dashboard用于管理内置插件
- 开放API: 所有插件均开放API供第三方使用， 通过这些API可简单灵活得配置插件、查看运行状态、统计数据等。
- 架构简单，依赖少，许可协议宽松，适合直接二次开发或改造。


### API

内置插件以HTTP Restful形式开放全部API，详细可查看[API文档](/docs/api/)


### 注意

- 现实中由于用户的业务系统多种多样，对于复杂应用，Orange并不是一个开箱即用的组件，需要调整一些配置才能集成到现有系统中。
- Orange提供的的配置文件和示例都是最简配置，用户使用时请根据具体项目或业务需要自行调整，这些调整可能包括但不限于:

    - 使用的各个shared dict的大小， 如ngx.shared.status
    - nginx.conf配置文件中各个server、location的配置及其权限控制，比如orange dashboard/API的server应该只对内部有权限的机器开放访问
    - 根据不同业务而设置的不同nginx配置，如timeout、keepalive、gzip、log、connections等等

### Licence

Orange采用MIT协议开源

### 其它

- Orange的插件模式参考了[Kong](https://getkong.org)，Kong是一个功能比较全面的API Gateway实现，推荐关注。
- Orange与Kong的不同(刨除基础设计，如存储、API)主要体现在针对"插件"和"API"的组织方式， Orange在流量筛选和变量提取方面相对来说更灵活一些。
