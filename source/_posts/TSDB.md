---
title: TSDB
date: 2020-09-17 11:22:17
tags:
---

###  TSDB设计：

### V1:

- 写：在磁盘上分散地写入单个数据点会相当地缓慢。因此，我们需要按顺序写入更大的数据块。

顺序写：随机写性能不高是因为ssd的GC问题

批量写：时序数据点一般比较小，会有写放大的问题（writing a 16 byte sample is equivalent to writing a full 4KiB page. This behavior is part of what is known as [write amplification](https://en.wikipedia.org/wiki/Write_amplification), which as a bonus causes your SSD to wear out）

- 读：查询范围不是完全水平或垂直的，从1000个序列中查一个数据点，也可能从一个序列查询1000个数据点。

每次读取hdd会读一个扇区，而ssd会读4k的page，如果你需要的只是时间窗口单独的几个datapoint，会很浪费磁盘的吞吐量。



### V2:

Batch up 1KiB **chunks** of samples for a series in memory and append those chunks to the individual files, once they are full.

缺点： one file per time series 



### Current:



nightinggale的存储：使用ringbuffer结构的 in-memory tsdb

- 将填满的chunk指针放到chunkslot中  已经finished的chunkslot定时刷盘
- 超过ringbuffer长度则currentPosition重置



### Ref:

**LSM 树**

https://en.wikipedia.org/wiki/Log-structured_merge-tree

LevelDB：

https://catkang.github.io/2017/01/07/leveldb-summary.html

[https://www.codedump.info/post/20190215-leveldb/#%E5%90%88%E5%B9%B6%E6%9D%A1%E4%BB%B6%E5%8F%8A%E5%8E%9F%E7%90%86](https://www.codedump.info/post/20190215-leveldb/#合并条件及原理)

   

Prometheus tsdb设计：

https://fabxc.org/tsdb/

https://developer.aliyun.com/article/354894?spm=a2c6h.13262185.0.0.5223700eOkHEUB

https://developer.aliyun.com/article/692437

https://n4mine.github.io/post/cacheserver-in-memory-tsdb-design/

http://www.voycn.com/article/nixuyaojingtongyizhongjiankong-shijianxulieshujuku

