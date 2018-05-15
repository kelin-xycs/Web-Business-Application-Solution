# Web-Business-Application-Solution


大家好，这个项目的目的，是 为 企业应用 + 互联网商业应用 提供一个 开源 解决方案。


这个项目 设计 为 组件 方式。不搞大包大揽。按需引用 需要的模块 就行，就像组件一样。^ ^
这样避免了 侵入性，铁板一块，难理解，难维护。

企业应用 + 互联网商业应用，要 解决 的 第一个问题是 Web 大规模 使用。


我简单的画了个 架构图：

我本来画了一个架构图的，但是又不知道怎么把图片贴进来，所以就文字描述一下了。如下：

架构

    Web
  
    缓存
  
    数据库
  

这个架构里只有 水平伸缩。这样比较简单。当 访问量 增大时，水平扩展 Web层，缓存层 就行。
数据库的话，现在 分布式数据库 用的还不普遍。对于 单主机 的 数据库，扩展CPU 核心数，内存；使用SSD硬盘就行。
比如 SqlServer 2017，配置了 足够的 CPU 核心数，内存，SSD，在 大数据量 高并发 时 的 表现很好。


缓存层 的 使用方法，可谓 各家各派，各种方法方式。^ ^
我的想法比较简单，就是 避免频繁的对数据库的读写。
具体的说，就是，在进行 业务运算和处理 时，把数据从数据库读取到 缓存，然后 运算处理。运算处理完成后，再把结果批量更新回数据库。运算过程中，对于数据的读写都是在缓存进行。
这样可以解决数据库成为瓶颈的问题。

这个架构，我把它叫做“3.5层架构”。因为 缓存层 算 0.5层 嘛。 ^ ^

上面是 架构。

下面我们说说 部署。我们希望做出一套 虚拟机(云服务器) + 容器编排 + 负载均衡 + Redis 来构建 Web 集群 的 解决方案。

接下来是 运维。根据 DevOps 的 理论，我再发展一下，就是： 开发 = 运维 。大家都是 开发人员，只不过这段时间我做 开发，你做 运维；下个月 我做 运维，你做 开发。 对不对？ ^ ^  这样就好玩了。

这套解决方案的 关键词 是 架构 部署 运维。

其实我不太赞同 微服务。但 SOA 倒是可能成为 趋势。因为 平台 与 平台 之间的 交互 应该会是一个 趋势。
微服务 的 第一个 课题 就是 实现 数据一致性。SOA 也需要 解决 这个问题。
但实际上，数据不一致 并不可怕，关键在于 清晰的 记录Log，及 提示出来，
让 用户 和 各方 能够清楚的看到 这一笔交易 是 失败 的，问题出在了哪里，接下来要怎么处理。
我们为此提供了一个 组件 SOALog。 SOALog 的功能就是 记录 Log 到 数据库 里。 见  https://github.com/kelin-xycs/SOALog 。
所以，在 调用服务失败 的时候，可以用 SOALog 记录 Log 到 数据库 里。 
Log 记录到 数据库 的 优点 是 便于 查询 分析，还可以用 报表 呈现出来。
报表 也可以呈现给 用户和相关各方 看，作为问题处理追踪 的 一个 报表。
总的来说，这是一种 松耦合 乐观 的 数据一致性 解决方案。

在 服务 调用中，会 涉及  “熔断” “重试”  等概念。我写了一个 熔断 重试 的 例子，见  https://github.com/kelin-xycs/FusingAndReTrySample

这里想表达的一些理念是：

少搞一点 封装， 少搞一点 控制反转（Ioc）， 少搞一点 AOP， 少搞一些 “声明式”。

不要隐藏太多代码，让 代码 回归 代码。

是 找回 80 年代 写 Basic 的 那种感觉 的 时候了。 ^ ^

负载均衡 方面，公有云可以使用 云 提供的 负载均衡 服务，比如 阿里云 的 弹性计算 负载均衡。
企业内部私有云，可以使用 Nginx。Nginx 是一个 反向代理服务器。可以实现 基于 Http 转发 的 负载均衡。
是一个 使用广泛，高效，稳定 的 负载均衡 服务器。
据说 使用 Nginx 可以 支持 每分钟 1 万笔 的 交易量。当然，也可能更多。 ^ ^



解决方案组件 和 参考资料 如下：


解决方案组件：


WebApiClient ，一款基于HttpClient封装，只需要定义c#接口并修饰相关特性，即可异步调用远程http接口的客户端库
https://github.com/dotnetcore/WebApiClient

MultiThreadFileScanner，一个 多线程 检索 文件内容 的 小程序
https://github.com/kelin-xycs/MultiThreadFileScanner

SOALog，为 SOA 架构 提供一种 松耦合 乐观 的 数据一致性 解决方案，说白了这个组件的功能就是 记录 Log 到 数据库 里，详细介绍 见 上文。
https://github.com/kelin-xycs/SOALoghttps://github.com/kelin-xycs/SOALog

FusingAndReTrySample， 熔断 重试 的 例子。 
https://github.com/kelin-xycs/FusingAndReTrySample

阿里云弹性计算
https://www.aliyun.com/?utm_medium=text&utm_source=bdbrand&utm_campaign=bdbrand&utm_content=se_32492



参考资料：

《后端架构师技术图谱》
https://github.com/xingshaocheng/architect-awesome

日访问量百亿级的应用如何做缓存架构设计
https://mp.weixin.qq.com/s?__biz=MzI3MzEzMDI1OQ==&mid=2651820285&idx=1&sn=b5f9fcb6ec6838afd5428070a1d81707&chksm=f0dcc681c7ab4f9734dc2b6a015c2508983df34cc004cd7be5a8269284bed5a444e68cbef896&mpshare=1&scene=1&srcid=0507vgwEC6jt4bKXLkrtXM5G&pass_ticket=XkwMJABPdvsBn%2F6Dky%2BMLVyhCKt55wJ1twxtjGvQgYWJEmAjjrE%2BdtRHT5jkaxtW#rd

DevOps老司机如何兼顾运维与开发？(附脑图)
https://mp.weixin.qq.com/s?__biz=MzI3MzEzMDI1OQ==&mid=2651820291&idx=1&sn=a704126dfee9762d74e1c30e3464f439&chksm=f0dcc77fc7ab4e690fb8310ab8ee1eb6624686e62f70a61a7149d299d9a6f01822ca9017953e&mpshare=1&scene=1&srcid=0508rgQzU5eAd11ccS8zL0Af&pass_ticket=XkwMJABPdvsBn%2F6Dky%2BMLVyhCKt55wJ1twxtjGvQgYWJEmAjjrE%2BdtRHT5jkaxtW#rd

蚂蚁金服CTO程立：金融级分布式交易的技术路径
https://mp.weixin.qq.com/s?__biz=MzI3MzEzMDI1OQ==&mid=2651820298&idx=1&sn=e01179f16295ac1fddc3c33696845fe4&chksm=f0dcc776c7ab4e604e1d7d3171708a40331e1e0dce100579026df9b4365be1f7af0e37da5e0a&mpshare=1&scene=1&srcid=0509JF8HPfpgyAQL3orPFGXq&pass_ticket=XkwMJABPdvsBn%2F6Dky%2BMLVyhCKt55wJ1twxtjGvQgYWJEmAjjrE%2BdtRHT5jkaxtW#rd

Oracle, Red Hat等大厂近期密集发布，加强支持Kubernetes
https://mp.weixin.qq.com/s?__biz=MzIzNjUxMzk2NQ==&mid=2247489297&idx=1&sn=ddcd1d728a7aa130e9ae950a12a17d64&chksm=e8d7e8d3dfa061c5c84f144005ea54d6ce03a73a48cd180af35bf9294615f5e01187cf0e2fc1&mpshare=1&scene=1&srcid=0507k5qY4RlrzKFodWKULXVl&pass_ticket=XkwMJABPdvsBn%2F6Dky%2BMLVyhCKt55wJ1twxtjGvQgYWJEmAjjrE%2BdtRHT5jkaxtW#rd

Netty——基本使用介绍
https://blog.csdn.net/haoyuyang/article/details/53243785

小白科普：Netty有什么用？
https://blog.csdn.net/bjweimengshu/article/details/78786315

Cef ，CEF：给客户端内嵌一个Chrome吧  https://www.itsvse.com/thread-4102-1-1.html

Polly.net
待更新

log4net Nlog 
https://www.cnblogs.com/qinjin/p/5134982.html
https://blog.csdn.net/lx520aa/article/details/77950057
http://www.cnblogs.com/Terrylee/archive/2006/12/03/opensource_framework_and_resource_recommendation_Log.html





之后会慢慢更新。 ^ ^

















