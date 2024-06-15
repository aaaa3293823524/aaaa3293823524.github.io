---
title: elasticsearch学习
tags:
  - 搭建博客
  - 前端
date: 2021-12-22 09:15:53
abbrlink: 824vf
---
倒排索引
![](https://gitee.com/mosheng123456789/pics/raw/master/img/360截图20211222092253165.jpg)
![](https://gitee.com/mosheng123456789/pics/raw/master/img/360截图20211222092544966.jpg)
搜索结果处理 排序 分页 高亮

存储,搜索,分析  很大存储搜索和分析也很快1秒 可扩展,分布式文档数据库  索引分片(单个lucene实例),副本高可用

分片水平分割数据,允许并行操作提高性能和吞吐量

主分片失败副分片可以成为主分片,主副不在一个节点上

如GitHub搜索

存储格式json

滚动升级,服务不中断

搜索路由多个值

索引路由一个值

_routing

分词器
分词过滤
字符过滤

索引模版


度量,分组,管道聚合


Elasticsearch的search_after功能允许你实现深度分页，这对于处理大量数据非常有用。传统的分页方法（如使用from和size参数）在数据量很大时可能会变得非常低效，因为Elasticsearch需要为每个请求重新计算整个结果集。而search_after允许你从上一次查询的最后一个文档开始继续搜索，从而避免了重新计算整个结果集



跨库分页


抢红包:
发  lpush list 

抢 lpop

记 hash

拆 二倍均值法
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-05-23_19-40-17.png)

下订单先放redis 然后放mq,然后持久化mysql

没有支付,24小时后此订单自动失效 后台冻结库存回退

推广短链接  分享字数限制:
短链接算法

映射匹配

短 跳转 长链接地址



set 抽奖  可能认识的人  共同好友  我关注的人也关注他

