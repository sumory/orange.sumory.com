---
category: Guides
redirect_from:
    - /docs/latest/guides/issues/
title: "常见问题"
sort_title: "3"
---



#### 提示`resty`或`nginx`命令找不到

Orange提供的默认脚本以及`orange` cli工具依赖`resty`和`nginx`命令， `resty`和`nginx`命令都是在安装OpenResty时安装的， 如果使用过程中提示找不到， 应该将这两个命令加入到环境变量中。

#### 默认的Dashboard登录账户是什么？

在开启权限验证的情况下， Dashboard需要登录才可使用， 用户名：admin 密码：orange_admin， 登录后再修改或添加其他普通账户即可。

#### Orange依赖的OpenResty版本是多少？

Orange的监控插件需要统计http的某些状态数据，所以需要编译OpenResty时添加`--with-http_stub_status_module`。由于使用了*_block指令，所以OpenResty的版本最好在1.9.7.3以上.

#### 全局统计插件不可用？

安装OpenResty时需要开启`--with-http_stub_status_module`.

#### Orange是否支持集群？

- Orange支持集群部署， 但并没有耦合诸如服务自动注册、发现的功能， 如有需求可使用Consul、 ETCD、 zookeeper等来实现。
- Orange的配置均存储于MySQL中, Orange节点内部状态（如连接信息、吞吐量、url访问状况、插件配置缓存等）存储在shared dict中。
- 在通过Dashboard或者API更新了一个Orange节点的配置后， 可通过以下三种方式来将配置同步到其他Orange节点
    - 重启其他Orange节点， 重启时会从MySQL拉取一遍全新的配置。
    - 通过在其他Orange节点的Dashboard上点击“同步配置”按钮来从MySQL中更新对应插件的最新配置。
    - 通过调用其他节点的`同步API`来更新最新配置。


#### start.sh脚本无法启动

- 检查OpenResty是否安装成功
    - 命令行要能直接执行`Nginx -v`和`lord -v`
    - OpenResty版本应在1.9.7.3++

- 检查[lor](https://github.com/sumory/lor)框架
    - 若使用的Orange版本低于v0.6.2则应安装lor v0.2.*版本
    - 若使用的Orange版本高于或等于v0.6.2则应安装lor v0.3.0+版本


#### 找不到libuuid.so

Orange现在依赖libuuid来生成规则id，centos用户可尝试使用`yum install  libuuid-devel`安装，其他用户请自行google对应平台的安装方式。

**注意**： Orange v0.6.0+ 已移除对libuuid的依赖。





