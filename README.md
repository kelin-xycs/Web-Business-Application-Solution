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

下面我们说说 部署。我们希望做出一套 虚拟机 + 容器编排 + 负载均衡 + Redis 来构建 Web 集群 的 解决方案。

接下来是 运维。根据 DevOps 的 理论，我再发展一下，就是： 开发 = 运维 。大家都是 开发人员，只不过这段时间我做 开发，你做 运维；下个月 我做 运维，你做 开发。 对不对？ ^ ^  这样就好玩了。

这套解决方案的 关键词 是 架构 部署 运维。


参考资料 和 解决方案组件 如下：

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


解决方案组件：


WebApiClient ，一款基于HttpClient封装，只需要定义c#接口并修饰相关特性，即可异步调用远程http接口的客户端库
https://github.com/dotnetcore/WebApiClient

Cef ，CEF：给客户端内嵌一个Chrome吧  https://www.itsvse.com/thread-4102-1-1.html


之后会慢慢更新。 ^ ^

















