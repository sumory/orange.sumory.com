---
title: Orange 安装
permalink: /install/
layout: simple
---

### 安装依赖

- OpenResty: 版本应在1.9.7.3+
    - Orange的监控插件需要统计http的某些状态数据，所以需要编译OpenResty时添加`--with-http_stub_status_module`
    - 由于使用了*_block指令，所以OpenResty的版本最好在1.9.7.3以上.
    - 在OpenResty安装好后需要将`nginx`和`resty`命令配置到**环境变量**中， 使用`nginx -v`和`resty -v`能正常输出。
- [lor](https://github.com/sumory/lor)框架
    - 若使用的Orange版本低于v0.6.2则应安装lor v0.2.*版本
    - 若使用的Orange版本高于或等于v0.6.2则应安装lor v0.3.0+版本
- MySQL
    - 配置存储和集群扩展需要MySQL支持。

### 数据表导入MySQL

- 在MySQL中创建数据库，名为orange
- 将与当前代码版本配套的SQL脚本(如install/orange-v0.6.3.sql)导入到orange库中； 如果需要从低版本升级到最新版，请自行对比版本间SQL脚本差异

### 修改配置文件

Orange有**两个**配置文件，一个是`conf/orange.conf`，用于配置插件、存储方式和内部集成的默认Dashboard，另一个是`conf/nginx.conf`用于配置Nginx(OpenResty).

orange.conf的配置如下，请按需修改:

```javascript
{
    "plugins": [ //可用的插件列表，若不需要可从中删除，系统将自动加载这些插件的开放API并在7777端口暴露
        "stat",
        "monitor",
        "redirect",
        "rewrite",
        "rate_limiting",
        "property_rate_limiting",
        "basic_auth",
        "key_auth",
        "signature_auth",
        "waf",
        "divide",
        "kvstore"
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


### 安装

- 在根据上文配置各种环境后， Orange无需“安装”即可使用
    - 默认的， 源码里提供了`start.sh`用来启动Orange
    - 本质上只要启动Nginx/OpenResty并根据默认的配置文件来加载Orange配置即可启动Orange
    - 用户可自行参考`start.sh`编写符合自己需要的类似脚本
- 此外， 如果有需要将Orange放到其他路径， 可通过`make install`来**安装**。 执行此命令后， 以下两部分将被安装：

```bash
/usr/local/orange     #orange运行时需要的lua文件
/usr/local/bin/orange #orange命令行工具, 依赖`resty`命令
```

### 验证是否安装成功

如已使用Makefile安装Orange， 则执行`orange help`查看有哪些命令可以使用：

```bash
Usage: orange COMMAND [OPTIONS]

The commands are:

start   Start the Orange Gateway
stop    Stop current Orange
reload  Reload the config of Orange
restart Restart Orange
store   Init/Update/Backup Orange store
version Show the version of Orange
help    Show help tips
```

根据以上两种方式选择使用`sh start.sh`或`orange start`来启动Orange。 Orange启动成功后， Dashboard和API server也随之启动：

- 内置的Dashboard可通过`http://localhost:9999`访问
- API Server默认在`7777`端口监听，如不需要API Server可删除nginx.conf里对应的配置


