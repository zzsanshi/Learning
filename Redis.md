# 2021/3/26

## 1.基本命令

启动:redis-server.exe

​		redis-cli.exe -h 127.0.0.1 -p 6379

查看:key:keys  *   , get key,exists key

清空:flushall ,flush 

移除key:move key

设置过期时间: expire key time

查看剩余时间:ttl key

选择表: select index

查看key的类型: type key

## 2.数据类型

### String

append key "value":追加数据

strlen:获取字符串长度

incr:自增+1

decr:自减-1

incrby key number: 设置步长指定增量 +number

decrby:同上

getrange key   start key:查看字符串范围   ,[0,-1]获得全部字符串

setrange key  start xx:修改字符串

setex key  times"":设置过期时间

setnx key "" :不存在再设置,存在则失败

mset:批量设置

 msetnx: 原子操作,要么一起成功,一起失败

getset:先获得再设置 

### List

基本数据类型,列表

Lpush list :插入到列表头部

Rpush list:放到尾部

Lrange list:获取区间的值

Lpop list:移除

Rpop list:移除

Lindex list:通过下标获取值

lrem list count value: 移除指定个数的值

ltrim list  start end :通过下标截取指定长度

rpoplpush:移除列表最后一个元素并移动到新的列表中

lset list:设置元素值,不存在会报错

linsert key before/after "" "":将某个具体的value插入某个列表的前/后

### set

值不能重复

sadd:添加值

srem:移除元素

spop:随机删除

smembers:查看元素

sadd: 

smove:移动元素

adiff:差集

sinter:交集

sunion:并集

### hash

key-<key,value>

hset myhash field ss:设置

hget:获得值

hmset:设置多个字段

hmget:获取多个字段

hgetall:获得所有值

hdel:删除

hlen:获取长度

hsetnx:

hincrby

### zset

在set的基础上增加了一个值

zadd:

zrangebyscore  myzet -1 0 (升序排列)

zcard :获取集合中的个数

zcount:判断区间数据个数

# 2021/4/5

## 3.特殊数据类型

### 1.geospatial地理位置--地择原理就是zset

定位,附近的人

**geoadd**:添加地理位置

**geodist:**查看距离

**georadius**:以给定的经纬度为中心,找出某一半径内的元素

**georadiusbymember:**找出指定元素周围的其他元素

geohash:返回一个或多个位置元素的geohash表示

### 2.hyperloglog--统计数量

**pfadd**:创建元素

**pfcount**:统计元素基数数量

**pfmeger**:合并两组

### 3.Bitmap--位存储

统计状态.

操作二进制位进行操作,只有0和1两个状态

**setbit**:设置记录

**bitcount**:统计

## 4.事务--同时成功,同时失败,

redis事务本质:一组命令的集合!一个事务中的所有命令都会被序列化,在事务执行过程中,会按照顺序执行!

redis单条事务保证原子性,redius事务不保证原子性

**执行事务:** 

1. multi :开始事务
2. 添加命令
3. 执行事务:exec

**放弃事务**:

​	**discard**:取消事务

**异常**:

​	**编译型异常**:命令错误,所有命令都不执行

​	**运行时异常**:其他命令正常执行,错误命令抛出

**实现乐观锁 **:使用watch充当乐观锁

1. watch:监视
2. unwatch:取消驾驶

## 5. redis配置文件,redis.conf

启动的时候,通过配置文件启动

1. 配置文件对大小写不敏感

2. 包含其他配置文件

3. 绑定ip,保护模式,端口

4. 通用配置:

   1. 以守护进程方式运行
   2. pid文件
   3. 日志文件

5. 快照--

   ​	持久化:在规定时间内,执行了多少次操作,则会持久化到文件.rdb.aof

   ​	redis是内存数据库,如果没有持久化,那么数据断电则消失

   1.  save 900 1  --900秒内有1个操作就保存 

6. replication复制,

7. security安全

   1. 设置密码 requirepass

8. client 客户端限制

   1. maxclients:设置最大内存容量
   2. maxmemory:最大内存容量

9.  append only :aof配置

## 6.Redis持久化

### 1.RDB(Redis DataBase)

在指定的时间间隔内将内存的数据集快照写入磁盘,即Snapshot快照,恢复就是将快照文件读到内存

Redis会单独创建一个(fork)一个子进程进行持久化,会先将数据写入到一个临时文件中,待持久化过程快结束了,再用这个临时文件替换上次持久化好的文件,主进程不进行任何IO操作.这样就确保了极高的性能.如果需要进行大规模数据的恢复,且对于数据恢复的完整不是非常敏感,那RDB方式要比AOF方式更加高效.RDB的缺点是最后一次持久化后的数据可能丢失.

 临时文件:dump.rdb

**触发机制**:

1. save命令
2. 执行flushall命令
3. 退出redis

优点:

1. 适合大规模的数据恢复
2. 对数据的完整性要求不高

缺点:

1. 需要一定的时间间隔进程操作!如果意外宕机了,这个最后一次修改数据就没有了
2. fork进程需要占用内存空间

### 2. AOF(Append Only File)

将我们的所有命令都记录下来,history,恢复的时候就把这个文件再执行一遍

优点:

1. 每一次修改都同步,文件完整性更好
2. 每秒同步一次,可能会丢失一秒的数据
3. 从不同步,效率更高

缺点:

1. 相对于数据文件来说,aof远远大于rdb,恢复速度也慢
2. aof运行效率慢

## 7. Redis订阅发布

Redis发布订阅(pub/sub)是一种消息通信模式:发送者(pub)发送消息,订阅者(sub)接收消息

Redis客户端可以订阅任意数量的频道.

subscribe:订阅一个频道

publish:发布命令

## 8. 主从复制

将一台Redis服务i器的数据,复制到其他的Redis服务器.前者称为主节点,后者称为从节点数据的复制是单向的,只能从主节点到从节点.



![image-20210405192844978](.\图片\image-20210405192844978.png)



**salveof  ip**:端口  --认主机

## 9.哨兵模式

自动选举主节点

## 10.缓存雪崩

如果缓存中没有就会取数据库中查询

**缓存穿透**:缓存中没有,一直在数据库中查找--查不到

**缓存击穿**:对一个点大量查询--查太多

解决方案:

1. 分布式锁
2. 设置过期时间

雪崩:缓存集中过期.


















