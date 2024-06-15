---
title: elasticSearch学习
tags:
  - elasticsearch
date: 2020-11-30 22:50:38
abbrlink: 7q7i2
---
>基于lucene

创建索引 搜索
倒排索引
文本分析 分析器由分词器，过滤器，字符映射器组成

**Lucene查询语言**

前缀查询(prefix query)不会被分析
匹配查询(map query)会被分析

查询类型
## 模糊查询  ~

## 范围查询
[]带边界  {}不带边界

索引分片,复制机制及其配置

**ElasticSeaarch基本概念**

索引，文档，映射，类型，节点，集群，分片，副本，网关

默认评分公式
二次评分
排序
查询重写



redis  双缓存

set session transaction isolation level read committed;

set session transaction isolation level repeatable read;

show status like '%innodb_row_lock%';

select * from information_schema.innodb_trx;


mysqldumpslow