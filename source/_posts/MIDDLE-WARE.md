---
title: MIDDLE_WARE
date: 2020-07-15 00:42:15
tags:
---

- MQ压测

如何压测：应该在TPS和机器的cpu负载、内存使用率、jvm gc频率、磁盘io负载、网络流量负载 之间取得一个平衡，尽量让 TPS尽可能的提高，同时让机器的各项资源负载不要太高。 

实际压测过程：采用几台机器开启大量线程并发读写消息，然后观察TPS、cpu load（使用top命令）、内存使用率（使用free命 令）、jvm gc频率（使用jstat命令）、磁盘io负载（使用top命令）、网卡流量负载（使用sar命令 sar –n DEV 1 4），不断增加机器和线程，让TPS不 断提升上去，同时观察各项资源负载是否过高。 

生产集群规划：根据公司的后台整体QPS来定，稍微多冗余部署一些机器即可，实际部署生产环境的集群时，使用高配置物理机，同时 合理调整os内核参数、jvm参数、中间件核心参数，如此即可



- JVM配置

如果我们准备上线一个新的系统，如何根据这个系统未来预估的业务量，访问量，去 推算这个系统每秒种的并发量，然后推算每秒钟的请求对内存空间的占用，进而推算出整个系统运行期间的JVM内存运转模 型。

然后基于这个推算出来的JVM内存运转模型，再接着去在系统上线前就选择一个合理的机器配置，要多大内存的机器，另外给 JVM堆内存空间一个合理的大小。

