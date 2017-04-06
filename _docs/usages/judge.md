---
category: Usages
redirect_from:
    - /docs/latest/usages/judge/
title: "条件判断模块的使用"
sort_title: "101"
---

[条件判断模块](/docs/concept/judge/)是多个插件中都使用到的一个模块， 它主要用来筛选出目标流量。

**对于PostPrams类型的条件**, 当前只支持`Content-Type`为`application/x-www-form-urlencoded`的POST请求。

条件判断模块有四种类型：

- 单一条件匹配： 顾名思义， 只支持一个条件
- and匹配： 对所有条件取与操作
- or匹配： 对所有条件取或操作
- 复杂匹配： 根据另行配置的表达式判断是否匹配

其中， 每个[条件](/docs/concept/condition/)都支持8种数据来源的8种操作符运算， 可自由搭配。


### 单一条件匹配

单一条件匹配非常简单， 比如我们想筛选出来自某个IP的流量， 只需:

![](/images/usages/judge_1.png)

比如想筛选出访问某个域名的流量， 只需:

![](/images/usages/judge_2.png)


### and匹配

支持多个条件作and运算，如果结果为真， 则表示该请求命中这个规则， 即筛选出了这部分流量。

比如我们想筛选出来自10.3.2.1访问my_domain.com的请求， 只需：

![](/images/usages/judge_3.png)


比如我们想筛选出**访问/user**这个URI并且**query中含有id这个参数**， 并且其**值大于1234**的请求， 只需：

![](/images/usages/judge_4.png)


### or匹配

or匹配与and匹配类似， 不过是对所有条件作**或**运算。


### 复杂匹配

**注意**： 复杂匹配可使用lua条件运算来筛选流量， 但其性能相较于上文几种方式会有下降， 应优先选择前三种流量筛选方式。

复杂匹配需要配置一个表达式，名为expression， 表达式中每个值的格式为v[index], 比如v[1]对应的就是第一个条件的值。


比如我们需要筛选出以下流量：

- 来源IP为10.3.2.1或10.4.5.6
- query中含有id， 并且id>1234

我们只需如下配置即可：

![](/images/usages/judge_5.png)


(v[1] or v[2]) and v[3]，即前两个条件至少一个为真并且第三个条件为真时，规则为真， 这时这个请求就被筛选出来了。



