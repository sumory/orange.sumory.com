---
title: Orange Installation
permalink: /install/
layout: simple
---


### 安装依赖

- OpenResty: 版本应在1.9.7.3+
- [lor](https://github.com/sumory/lor)框架: 版本在v0.1.0+
    - git clone https://github.com/sumory/lor
    - cd lor & sh install.sh
- libuuid.so
    - Orange依赖libuuid生成uuid
    - centos用户可通过命令`yum install libuuid-devel`安装，其它情况请自行google
- MySQL
    - 从0.2.0版本开始，Orange不再支持本地文件存储，目前仅提供MySQL存储支持.

### 数据表导入MySQL

- 在MySQL中创建数据库，名为orange
- 将与当前代码配套的SQL脚本（如install/orange-v0.4.0.sql）导入到orange库中

### 修改配置文件

Orange有两个配置文件，一个是`orange.conf`，用于配置插件、存储方式和内部集成的默认Dashboard，另一个是`conf/nginx.conf`用于配置Nginx(OpenResty).

orange.conf的配置如下，请按需修改:

```javascript
{
    "plugins": [ //可用的插件列表，若不需要可从中删除，系统将自动加载这些插件的开放API并在7777端口暴露
        "stat", 
        "monitor", 
        "redirect", 
        "rewrite",
        "rate_limiting",
        "basic_auth",
        "key_auth",
        "waf", 
        "divide"
    ],

    "store": "mysql",//目前仅支持mysql存储
    "store_mysql": { //MySQL配置
        "timeout": 5000,
        "connect_config": {//连接信息，请修改为需要的配置
            "host": "127.0.0.1",
            "port": 3306,
            "database": "orange",
            "user": "root",
            "password": "",
            "max_packet_size": 1048576
        },
        "pool_config": {
            "max_idle_timeout": 10000,
            "pool_size": 3
        },
        "desc": "mysql configuration"
    },

    "dashboard": {//默认的Dashboard配置.
        "auth": false, //设为true，则需用户名、密码才能登录Dashboard,默认的用户名和密码为admin/orange_admin
        "session_secret": "y0ji4pdj61aaf3f11c2e65cd2263d3e7e5", //加密cookie用的盐，自行修改即可
        "whitelist": [//不需要鉴权的uri，如登录页面，无需修改此值
            "^/auth/login$",
            "^/error/$"
        ]
    },

    "api": {//API server配置
        "auth_enable": true,//访问API时是否需要授权
        "credentials": [//HTTP Basic Auth配置，仅在开启auth_enable时有效，自行添加或修改即可
            {
                "username":"api_username",
                "password":"api_password"
            }
        ]
    }
}
```

conf/nginx.conf里是一些nginx相关配置，请自行检查并按照实际需要更改或添加配置，特别注意以下几个配置：

- lua_package_path：需要根据本地环境配置适当修改，如[lor](https://github.com/sumory/lor)框架的安装路径
- resolver：DNS解析
- 各个server或是location的权限，如是否需要通过`allow/deny`指定配置黑白名单ip


#### 启动

执行`sh start.sh`即可启动orange.

- 内置的Dashboard可通过`http://localhost:9999`访问
- API Server默认在`7777`端口监听，如不需要API Server可删除nginx.conf里对应的配置

#### 安装Orange到系统中

从v0.5.0开始，Orange提供`安装到系统`功能，步骤如下：

```
git clone https://github.com/sumory/orange
cd orange 
make install
```

执行成功后， 有两部分文件被安装：

- 在`/usr/local/orange`下安装了Orange运行时所需的lua文件
- 安装了一个命令`/usr/local/bin/orange`, 它的内容如下：

```lua
#!/usr/bin/env resty
package.path="/usr/local/orange/?.lua;;" .. package.path
require("bin.main")(arg)
```


之后通过`orange help`命令查看可以使用那些命令进行操作：


```shell
[17:53]root:✗✗✗ # orange help
Orange v0.5.0, OpenResty/Nginx API Gateway.

Usage: orange COMMAND [OPTIONS]

The commands are:

stop    Stop current Orange
reload  Reload the config of Orange
version Show the version of Orange
help    Show help tips
restart Restart Orange
start   Start the Orange Gateway
store   Init/Update/Backup Orange store
```

如果想查看某个具体的子命令(如orange start)如何使用， 执行`orange start -h`即可。
