---
category: Usages
redirect_from:
    - /docs/latest/usages/divide/
title: "divide分流插件使用示例"
sort_title: "204"
---

> 0.6.4及以前版本的divide插件支持静态分流，即需要先在nginx.conf里配置upstream，然后再在dashboard里使用。

### 使用场景

在Nginx里，对请求的变换有很多种方法，比如redirect一个URL到其他地址、rewrite一个URI到另一个内部location、通过upstream来代理其他server。

其中只有`通过upstream来代理`这种场景才适合使用Orange的divide分流插件，请不要混淆。

### 简单使用示例

1) 首先我们需要在nginx.conf里建了一个upstream，它的配置如下

```
#代理位于其他服务器的程序，这里以127.0.0.1:8001和127.0.0.1:8002为例
upstream my_upstream {
     server 127.0.0.1:8001;
     server 127.0.0.1:8002;
}
```

2) 确保`127.0.0.1:8001`和`127.0.0.1:8002`能提供`/abc`这个请求的处理，即访问`http://127.0.0.1:8001/abc` 时能正常提供服务

3) 在dashboard里`代理&分流插件`的配置如下：

配一个选择器和一条规则：

![image](https://user-images.githubusercontent.com/1326893/28348843-dfc78d04-6c71-11e7-9726-abb6b314f64b.png)

规则详情如下：

![image](https://user-images.githubusercontent.com/1326893/28348880-17f1a552-6c72-11e7-827e-9b5de864e54e.png)

- `upstream_host`指的是访问upstream时需要带过去的HTTP的header头Host，不填写的话默认为当前请求的Host头。
- `upstream_url`指的是`upstream`的配置，这个值最终会传给`proxy_pass`指令使用。

4) 访问`http://${your_host}:${your_port}/abc`, 就会发现能正常访问后端实际的API，即`http://127.0.0.1:8001/abc`或`http://127.0.0.1:8002/abc`


### 需要特别注意的问题

1) 如果nginx.conf里配置了一个upstream名为`abc_upstream`，使用它时填写`upstream_url`最好使用`http://abc_upstream`。除非非常清楚Nginx的proxy_pass指令用法，否则，不建议出现类似这样的写法: `http://abc_upstream/`、`http://abc_upstream/other_api`、`http://abc_upstream/api?name=123`。具体原因请参看issue: [分流插件: GET请求出现参数丢失问题](https://github.com/sumory/orange/issues/128)

2) 关于post请求，可参看issue: [使用分流插件后获取post请求参数问题](https://github.com/sumory/orange/issues/136)
