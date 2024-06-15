---
title: rocketmq学习
tags:
  - 搭建博客
  - 前端
date: 2023-10-28 22:34:32
abbrlink: 8mzhm
---

9876 nameserver

发送同步消息  异步

负载均衡

顺序
延时

消息批量发送
消息过滤  tag  sql语法
事务消息

brokerName brokerId

https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.13.0-linux-aarch64.tar.gz


<mirror>
      <id>nexus-aliyun</id>
      <mirrorOf>central</mirrorOf>
      <name>Nexus aliyun</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>

mvn clean install -DskipTests
mvn compile -Dcheckstyle.skip=true

<dependency>
    <groupId>io.netty</groupId>
    <artifactId>netty-all</artifactId>
    <version>4.1.39.Final</version>
</dependency>


rocketmq-console

/Library/Preferences/VMware\ Fusion/vmnet8
172.16.117.100
172.16.117.2  网关
255.255.255.0 子网掩码

DNS服务器 223.5.5.5
ssh root@172.16.117.100

/usr/local/vmware

TimeUnit 睡眠


https://www.bilibili.com/video/BV1qc411d7nJ/?spm_id_from=333.337.search-card.all.click


论坛  Echo 飞天小牛肉 牛客论坛

rpc  模拟grpc http协议  protobuf序列化 hessian 多种序列化 泛化调用 服务发现 负载均衡
https://blog.csdn.net/BASK2311/article/details/131418774

秒杀

普通的RPC调用，我要拿到服务端提供的class或者jar包。但这样实在过重，更很差维护。不过，成熟的RPC框架都支持泛化调用，咱们的网关就是基于这种泛化调用来实现的。服务端开放出来他们的API文档，咱们拿到接口、参数、参数类型经过泛化调用到服务端程序。

public Object $invoke(String method, String[] parameterTypes, Object[] args);

泛化调用指的是一些通信协议的转换，比如将HTTP转换成Thrift。在一些开源的网关中比如Zuul是没有实现的，因为各个公司的内部服务通信协议都不同。比如在唯品会中支持HTTP1,HTTP2,以及二进制的协议，然后转化成内部的协议，淘宝的支持HTTPS,HTTP1,HTTP2这些协议都可以转换成，HTTP,HSF,Dubbo等协议。

如何去实现泛化调用呢?由于协议很难自动转换，那么其实每个协议对应的接口需要提供一种映射。简单来说就是把两个协议都能转换成共同语言，从而互相转换。

什么是API网关 如何设计亿万级统一网关

一般来说共同语言有三种方式指定:

json：json数据格式比较简单,解析速度快，较轻量级。在Dubbo的生态中有一个HTTP转Dubbo的项目是用JsonRpc做的，将HTTP转化成JsonRpc再转化成Dubbo。

https://github.com/2YSP/rpc-spring-boot-starter





/usr/local/mysql/bin/mysql -u root -p


通过更改加密规则和刷新权限的方式
mysql -uroot -ppassword #登录

use mysql; #选择数据库
# 远程连接请将'localhost'换成'%'

ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #更改加密方式

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; #更新用户密码

FLUSH PRIVILEGES; #刷新权限


sudo redis-server redis.conf




链接: https://pan.baidu.com/s/13YM1Fe0iWIhumdlu3WO95g?pwd=1111 提取码: 1111
--来自百度网盘超级会员v5的分享

把ik压缩包解压重命名成ik然后放在es里面的插件包下面就行 注意 ik如果下tr.tz的会报错，因为没有properties文件 需要在idea里面编译打包，不过我试了还是不行。最好还是用压缩文件格式的。

https://blog.csdn.net/qq_24950043/article/details/127895547

localhost:2181
prettyZoo

./zkserver.sh start

https://www.lanzouw.com/b00t6r2id

https://blog.csdn.net/u010355502/article/details/132300051

xattr -rc /Applications/prettyZoo.app




/usr/libexec/java_home -V


"$JAVA" "$XSHARE" -cp "$ES_CLASSPATH" org.elasticsearch.tools.java_version_c    hecker.JavaVersionChecker


#添加jdk判断
if [ -x "$JAVA_HOME/bin/java" ]; then
        JAVA="/Users/zhangxuefeng/Library/Java/JavaVirtualMachines/azul-11.0.21/Contents/Home/bin/java"
else
        JAVA=`which java`
fi



JAVA_HOME=/Users/zhangxuefeng/Library/Java/JavaVirtualMachines/azul-11.0.21/Contents/Home
CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
ES_JAVA_HOME=/Users/zhangxuefeng/Downloads/elasticsearch-7.13.0/jdk
PATH=$JAVA_HOME/bin:$PATH:.

export JAVA_HOME
export PATH
export CLASSPATH
export ES_JAVA_HOME


curl -XGET http://localhost:9200


sudo npm install n -g
sudo n 16.17.0

NODE="/usr/local/bin/node"


bin/kafka-server-start.sh config/server.properties

创建主题
bin/kafka-topics.sh --zookeeper  localhost:2181 --create --topic test --partitions 2 --replication-factor 1

查看主题
bin/kafka-topics.sh --zookeeper  localhost:2181 --describe --topic test 

生产发消息
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

消费消息
bin/kafka-console-consumer.sh --bootstrap-server
localhost:9092  --topic test 


其实，在配置文件里指定好kafka的topic之后，调用send方法会自动帮我们创建好topic，只是创建的topic默认是1个副本和1个分区的，这一般不能满足我们的要求，所以我们还需要在kafka的server.properties里增加或修改以下参数：
```
num.partitions=3
auto.create.topics.enable=true
default.replication.factor=3
```
之后，kafka自动帮我们创建的主题都会包含3个副本和3个分区。
另外，也可以通过一些api帮我们创建好主题，这个就需要自己手动去实现创建topic的方法。

current-offset：当前消费组的已消费偏移量
log-end-offset：主题对应分区消息的结束偏移量(HW)
lag：当前消费组未消费的消息数

run方法代码
这个方法完成的事情有：

记录开始时间
获取和开启监听器
准备环境(创建并配置环境)
打印Banner(控制台打印的图标)
实例化上下文对象
准备上下文，以及实例化bean对象
刷新上下文，并启动tomcat
计算耗时
打印日志，打印耗时
通知监听器启动完成
通知监听器ready


xattr -r -d com.apple.quarantine  {应用程序的位置}

例如：xattr -r -d com.apple.quarantine xattr -r -d com.apple.quarantine /Applications/Termius.app


csrutil disable
csrutil enable


跳板机  内网穿透

Java 8 中需要知道的4个函数式接口-Function、Consumer、Supplier、Predicate

Java工具库Guava的不可变集合和新集合类型Multiset、Multimap、BiMap、RangeSet、RangeMap等的使用示例
ImmutableMap  ImmutableList

Java常用工具类 : StringUtils、CollectionUtils、ArrayUtils、Lists、Maps等

jobdetail job
trigger
scheduler
@DisallowconcurrentExecution   避免并发
@PersistJobDataAfterExecution   持久化



优先级
misfire  错过触发

schedulerFactory jobstore  simpleExecutor

start stop pause resume

监听器
作业存储方式


elastic-job
作业分片
注册中心
策略
选举
实例
分片
故障转移
Simple、DataFlow、Script 作业类型

支持并行调度
支持任务分片，任务分片是指将一个任务分为多个小任务项在多个实例同时执行。
作业分片一致性
当任务被分片后，保证同一分片在分布式环境中仅一个执行实例。
zookeeper集群管理
latch分布式锁

leader节点
instances节点
sharding节点  分片信息

分布式锁节点为：${namespace}/jobname/leader/failover/latch，谁先获得锁，则执行失效故障转移具体逻辑(FailoverLeaderExecutionCallback)

故障实现转移基本实现思路为：当一个任务节点宕机后，其他节点会监听到实例删除事件，从实例目录中获取其实例ID，并从ZK中获取原先分配故障实例的分片信息，并将这些分片标记为需要故障转移(创建${namespace}/jobname/leader/failover/items/{item}持久节点)，然后判断是否需要执行故障转移操作。

执行故障转移操作的前提条件是：

当前任务实例也调度该job；
存在${namespace}/jobname/leader/failover/items节点并有子节点。如果满足上述两个条件，则执行失效转移，多个存活节点进行选主(LeaderLatch),创建分布式锁节点(${namespace}/jobname/leader/failover/latch),获取锁的节点优先执行获取分片节点，其具体过程如上述所示，每个存活节点一次故障转移只竞争一个分片。



insert 插入意向锁  是行锁还是表锁 没有索引怎么锁  不同隔离级别表现
自增锁
Optional

mapstruct
nullValueMappingStrategy 
collectionMappingStrategy = CollectionMappingStrategy.ADDER_PREFERRED


expireAfterAccess:缓存项在单位时间内未发生过读/写操作 即被回收;

expireAfterWrite:缓存项在单位时间内未被更新即被回收;

refreshAfterWrite:缓存项离上一次更新操作多久之后会被 更新。


getMessage 异常原因
toString 异常类型 异常原因
printStackTrace 异常堆栈信息

Iterable   lsof

uv hyperloglog
本地缓存 所有用户帖子数量 所有用户帖子按热度排序分页
发帖过滤敏感词
帖子类型 置顶  普通
帖子状态 正常 精华
帖子分数

评论
评论类型


关注
点赞
发帖
删帖


MockMvc


ftp  21
sftp 22



setIfAbsent
redisson 可重入 lua脚本 hash
分布式session
ACL

type  
object encoding
SDS
int embstr(<=44字节)  raw

用户购物车
hash    hset  hincrby  hlen  hdel  hgetall

微博和微信公众号消息流
lpush  lrange

抽奖 set sadd smembers 
srandmembers key [count]
spop key [count]

点赞，收藏，标签
set sadd srem  sismember scard

sinter
sunion
sdiff
共同关注 sinter
我关注的人也关注他 sismember
我可能认识的人  sdiff

zscore zrevrange

dbsize

getrange
setrange

hscan渐进式遍历hash  hkeys  hvals

zcount  zinterstore

JedisPool

pub/sub
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-18_02-04-15.png)




![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-18_02-29-13.png)


![](https://gitee.com/mosheng123456789/pics/raw/master/img/360%E6%88%AA%E5%9B%BE20220113155136943.jpg)


防盗链
<meta name="referrer" content="no-referrer">

modheader referrer

xhr请求


use
require是node中的一个方法,他的作用是用于引入模块、 JSON、或本地静态文件
require.context
let var const

let声明和var声明用法是一样，都是定义变量，使用let声明的变量没有var那样的变量提升，let声明的变量只在当前作用域中有效
虽然const变量不能修改指针，但是可以修改值，比如我们定义一个对象，我们就可以修改对象里的属性值，但是不可以重写整个对象
暂时死区
export default  向外暴露成员

__proto__和constructor属性是对象所独有的；② prototype属性是函数所独有的。但是由于JS中函数也是一种对象，所以函数也拥有__proto__和constructor属性

=>箭头函数  和普通函数区别
语法不同
this 的指向不同
作为方法的用法不同
作为构造函数的用法不同 
普通函数可以作为构造函数，使用 new 关键字实例化。而箭头函数不可以，否则会抛出类型错误的异常
arguments 绑定不同
箭头函数没有原型，以及在箭头函数中不能使用 yield 关键字，因为它不能作为 generator 函数

vuex中的this.$store.commit的使用
this.$store.dispatch

双向数据绑定
v-if
v-model
v-for

V8 作为一个 JavaScript 引擎 c++开发 chrome

原型链，作用域

zset  延迟队列 zadd 时间戳是score  轮询zrangebyscore zrem

setbit getbit bitcount 

bitmap 排序去重   黑白名单
n  元素个数
m  位数组长度
k  hash函数个数
两个hash值模拟多个hash函数值
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-18_22-37-52.png)

RedisBloom 插件  Jrebloom
Redisson客户端
单机用Google布隆过滤器

hyperloglog 不精确去重计数
概率论伯努力试验 结合极大似然估算方法  做分桶优化  调和平均数
标准误差 0.81%

![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-19_11-59-34.png)
value 64bit 16384*6/8/1024=10kB

GEO 地理信息定位  附近位置  摇一摇

地理位置距离排序算法 GeoHash  zset实现
geoadd  geopos  geodist  georadius

redis命令周期 
发送 排队 执行 返回
config set
config rewrite
slowlog get
slowlog len

pipeline 单个指令批量指令  客户端行为  不保证原子性（tcp拆包）
内核输入输出缓冲区  4k-8k
TCP包大小  1460 1500（MTU）

redis事务  服务端行为  被服务器缓存
语法错，整个事务不执行
运行错，执行前面的命令，不回滚
watch命令  乐观锁
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-19_15-12-52.png)

eval "return redis.call()"

script load
evalsha
script flush
lua脚本会持久化吗？

publish subscribe  不可靠
pubsubscribe numsub 查看订阅数

Stream  pending_ids袋确认消息
xadd  xlen xrange xdel 
xread  count 1 streams sss $  单消费者
xgroup create
xinfo stream
xinfo groups 
xinfo consumers
xreadgroup GROUP
xack
xpending
xclaim 消息在消费者之间进行转移（消费异常时）
不支持分区
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-19_21-43-45.png)
基数树
lastgenerated_id



Optional学习




深入理解kafka核心设计与实践原理
redis设计与实现
redis深度历险
深入理解es

wget https://d.frps.cn/file/openvpn/openvpn-install.sh && bash openvpn-install.sh

sudo brew services start openvpn
/opt/homebrew/opt/openvpn/sbin/openvpn --config /opt/homebrew/etc/openvpn/openvpn.conf



tomcat
RequestFacade Request
ajp,http协议
bio nio
war包
server.xml  Context标签
多线程部署web应用 （线程池）

4个容器
Wrapper  Servlet类
Context 应用
Host  虚拟主机
Engine  可以理解成集群
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-20_18-14-28.png)
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-20_18-30-42.png)

StandardWrapper生成Servlet实例
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-20_18-49-58.png)


linux: tcp_connect()  建立TCP连接
socket0  openjdk源码 jni  操作系统
Java字节码调用C/C++的解决方案，JNI描述的是一种技术

Connector
tomcat7 NIO,BIO(默认)
tomacat8 NIO

每个socket连接用一个线程池中的线程处理

kafka生产者发送消息原理
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2023-11-20_23-29-24.png)

bin/kafka-run-class.sh加配置JMX_PORT=



# nameserver
172.16.117.103 rocketmq-nameserver1
172.16.117.104 rocketmq-nameserver2
# broker
172.16.117.103 rocketmq-master1
172.16.117.104 rocketmq-master2
172.16.117.105 rocketmq-slave1
172.16.117.106 rocketmq-slave2

nohup sh mqnamesrv &

mkdir /opt/software/rocketmq-all-4.4.0-bin-release/store
mkdir /opt/software/rocketmq-all-4.4.0-bin-release/store/commitlog
mkdir /opt/software/rocketmq-all-4.4.0-bin-release/store/consumequeue
mkdir /opt/software/rocketmq-all-4.4.0-bin-release/store/index


nohup sh mqbroker -c /opt/software/rocketmq-all-4.4.0-bin-release/conf/2m-2s-sync/broker-a.properties &

nohup sh mqbroker -c /opt/software/rocketmq-all-4.4.0-bin-release/conf/2m-2s-sync/broker-b.properties &

nohup sh mqbroker -c /opt/software/rocketmq-all-4.4.0-bin-release/conf/2m-2s-sync/broker-a-s.properties &

nohup sh mqbroker -c /opt/software/rocketmq-all-4.4.0-bin-release/conf/2m-2s-sync/broker-b-s.properties &


mqadmin
rocketmq-console


nohup java -jar rocketmq-console-ng-1.0.0.jar &



git config --global -l 

 git ls-files

git reset --soft 工作区和暂存区都有
git reset --mix 默认 工作区有 暂存区无
git reset --hard 工作区和暂存区都无 

 git diff 默认工作区和暂存区
  git diff HEAD 暂存区和版本库

  HEAD^
  HEAD~  
  HEAD~2  之前2个版本


git rm 
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-06_22-18-38.png)


.gitignore 不加入版本库

** 匹配中间目录



git remote add origin

git remote -v

git push -u origin main


GUI工具  sourcetree



git branch

git checkout

git switch

git branch -d 删除已合并的分支
git branch -D 删除分支

git merge --abort 中断合并

git chekout -b

hotfix

git tag



join_buffer

order by索引测试

CREATE TABLE `t_name` (
  `id` int NOT NULL COMMENT '用户id',
  `name` varchar(255)  NOT NULL COMMENT '用户姓名',
  `age` int not NULL COMMENT '年龄',
  `sex` varchar(255) not NULL COMMENT '性别',
  PRIMARY KEY (`id`) 
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

create index idx_age on t_name(name,age,sex);

explain select * from t_name where age=18 order by name;

alter table t_name add column `password` varchar(255) not null comment '密码';


sort_buffer

max_length_for_sort_data

全字段排序

rowid排序


group by

SQL_BIGRESULT


show engine innodb status;  (看last detected deadlock 死锁)

自增锁模式 表锁
show variables like '%innodb_autoinc_lock_mode%';

0  传统 自增锁互斥
1  连续 能确定插入条数 自增值预留 不使用自增锁 使用mutex分配id insert into  ;不能确定插入条数  使用自增锁 同0
2 交叉 只用mutex获取id  但主从同步binlog采用statement格式记录sql语句主从同步会有问题


-- mysql 8
SELECT * FROM performance_schema.data_locks;

-- mysql 5.7
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;

查死锁表
SELECT * FROM performance_schema.data_locks;

等待锁的事务
SELECT * FROM performance_schema.data_lock_waits;

是否锁表
show open tables where in_use>0;


死锁杀进程

show processlist
select * from information_schema.innodb_trx;

show engines;

select * from information_schema.engines;

set default_storage_engines='';

控制块 (表空间 数据页编号 内存页地址)  哈希表
每一页有页目录可二分查找,最小最大记录,前一个页面和后一个页面指针

传统lru
预读失效(局部性原理) 全表扫描导致失去热点数据

改进lru
新旧 5:3

预读先放中间点,访问到放入新链表头部

老生代停留时间窗口

线性预读,随机预读(将当前区剩余页放入buffer pool)
逻辑存储结构  逻辑存储空间中 表空间 
段 区 页(块)

数据段 索引段 回滚段
区由连续页组成 1MB  innodb每次从磁盘申请4-5个页 页16KB 一个区有64个页
innodb_read_ahead_threshold 连续访问多少个页面后把下一个区页面读入buffer pool 不能超过64


随机预读
innodb_read_ahead  废弃

buffer pool参数优化
innodb_buffer_pool_size
服务器内存
小于1GB 128MB
1-4GB *0.5
>4GB *0.75

innodb_log_file_size 自动配置的日志文件大小
缓冲池大小
不到8GB  512MB
8GB-128GB 1024
>128GB  2048


innodb_old_blocks_pct  37   设置100则退化为普通lru

innodb_old_blocks_time   ms单位    老生代停留时间窗口 数据检索一般几毫秒,几十毫秒

# Buffer pool三种page和链表
free  未使用    free链表(放控制块)
clean 已使用(加载磁盘数据) 页面未修改 lru链表
dirty 页面已修改   lru链表  flush链表(控制块)   page cleaner线程刷盘


# change buffer 写缓冲区
非唯一索引(不需要做唯一性校验)
辅助索引页更改

辅助索引页不在buffer pool时用change buffer记录更改,某时刻(有查询操作 buffer pool将页加载进来)加载到buffer pool合并,减少访问一次磁盘

后台线程定期merge,数据库正常关闭也会merge

innodb_change_buffering
all
none
inserts
deletes (索引记录最初被标记为删除,不是物理删除)
cahnges
purges 后台物理删除

innodb_change_buffer_max_size  百分比 默认20 最大25

# 自适应哈希索引
innodb_adaptive_hash_index
innodb存储引擎会监控表上索引页(二级索引)的查询,自动建立合适的hash索引,提升数据页访问效率 O(1)
降低对二级索引树的频繁访问
缺点  
占用buffer pool
只适用等值查询
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-08_18-39-39.png)

二级索引列->数据页
# log buffer 日志缓冲区
增删改,可靠性(不丢失,可还原),记录日志(操作数据的轨迹)
定期或满了刷到磁盘
默认16MB
innodb_log_buffer_size
innodb_flush_log_at_trx_commit

![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-08_18-48-05.png)
log buffer->os cache->判断
# innodb存储结构
表空间 存储表结构和数据(含索引)
## 系统表空间  
change buffer
ibdata1
innodb_data_file_path  系统表空间文件大小和数量
innodb_autoextend_increment   自动增加64M
## 独立表空间 
单个表数据和索引
innodb_file_per_table

.ibd文件

## 通用表空间 
共享表空间包括系统表空间和通用表空间,可以被多个表共享
create tablespace myts;
show variables like '%datadir%';
drop tablespace myts;
create tablespace myts; add datafile 'myts.ibd' engine=innodb;
create table  tablespace myts;
alter table t1 tablespace innodb_file_per_table;
## 临时表空间 
外部临时表
create temporary table
show tables 不显示临时表信息
内部临时表
group by,order by,distinct,union 
undo回滚时,如果空间不足,mysql内部使用自动生成的临时表

会话临时表空间
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-08_20-07-23.png)
初始10个临时表空间
.ibt
innodb_temp_spaces_dir
全局临时表空间
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-08_20-04-48.png)
select @@innodb_temp_data_file_path;
ibtmp1
12M
## undo表空间
show variables like 'innodb_page_size';
innodb_undo_directory
innodb_undo_tablespace
8 初始16M 翻倍扩容
8之前 10M
127个
undo_001 undo_002

select tablespace_name,file_name from information_schema.files;
# 双写缓冲区

写失效(部分页失效)
innodb_doublewrite

# redo log
ib_logfile0  ib_logfile1

每个回滚段1024 undo log segment
128
innodb_rollback_segments



#  binlog
binlog_format
log_bin
max_binlog_size


![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-08_23-00-48.png)

max_allowed_packet

# 主从同步
stop slave
reset master

锁表
flush tables with read lock;

unlock tables;

故障分类 故障时间乘以权重

业务指标 用户访问量 订单量 查询量
技术指标 cpu,磁盘,内存
## MMM

## MHA

## QMHA
zk集群通知变更
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_14-21-38.png)

## PXC MGR
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_14-25-17.png)


![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_14-50-06.png)

![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_14-52-49.png)


系统耦合,开发效率低
高性能,高可用 低成本  安全
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_15-10-08.png)

能力,成本,时间

![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_15-15-10.png)


PACELC

![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_15-35-53.png)

Bully 霸道集群选主  长者为大
MongoDB 节点最后操作时间戳来表示id,时间戳最新节点最大
选举速度快,算法简单
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_20-03-11.png)

民主选举算法 一致性算法/协议
强一致 paxos raft zab
弱一致 gossip

basic paxos
multi paxos

任何写入操作 数据同步 选举

角色定义:提案 批准 学习

# raft 可靠 可复制 冗余 容错
leader选举
日志复制 (数据同步)

随机选举超时算法 300ms
# ZAB 原子广播
消息广播 (数据同步)
崩溃恢复 (选举)

zxid 64位 高32位epoch 低32位递增(客户端事务请求加1)
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_22-10-06.png)

分布式锁

数据量大 分表  存储和查询
并发高 分库 数据库连接
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-09_23-43-44.png)

查表法 灵活

正常情况主从延迟不会超过10ms



多级缓存  热点数据 避免oom
读写分离

服务等级协议 SLA
![](https://gitee.com/mosheng123456789/pics/raw/master//Users/zhangxuefeng/Desktop/img/Snipaste_2024-06-10_16-17-28.png)

redis查看信息
info
info memory

oom
jvm参数 系统挂了生成dump文件
jmap -dump
jvisualvm gcroot 线程引用定位问题

jmap -heap 堆信息

jmap -histo 堆统计信息

jstat -gc 垃圾收集信息

cpu飙高
top
top -H -p 1234
jstack  |grep  -A 20


7种sql进阶
自定义排序  order by field()
空值排序   order by if(isnull(),0,1)
case when
分组连接  group_concat
with rollup

with as

insert ignore into

replace into

on duplicate update

springboot加速启动

hutool

io流

多线程编排  asyncTool


同步/异步: 线程主动调用系统调用把准备好的数据从内核拷贝到用户空间(同步),内核自己把数据拷贝到用户空间直接通知线程(异步)

阻塞/非阻塞:recvfrom系统调用是否在数据未就绪阻塞