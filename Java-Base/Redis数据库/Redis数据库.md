@[TOC](目录)
# Redis介绍与安装

> Redis是一个高性能的NoSql数据库（not only sql）
> Redis是一个非关系型数据库，而mysql、oracle这些数据都是关系型数据库
> 作用是将常用的数据存到缓存中，这个Redis数据相当于缓存的作用，提供保存常用的数据
> Redis中的数据大部分时间都是存储在内存中的
> Redis访问效率极高，但不适合存储数据量大的数据
> Redis支持持久化，可以将数据存到本地硬盘中
******
> Redis安装管网：[Redis](http://www.redis.cn/)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/8a12bd0f037a4213994fd4adb1719cae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 下载好之后放到Linux系统内![在这里插入图片描述](https://img-blog.csdnimg.cn/c09ff1184ef84208b5f89294eec22fa7.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3f2e96aaca0242b6a4fc90d1eccd6da1.png)
解压命令：tar -zxvf redis-6.2.6.tar.gz -C /opt
下一步安装GCC编译器（c/c++的编译器）命令：yum -y install gcc
![在这里插入图片描述](https://img-blog.csdnimg.cn/7b5ff401065949d590d2ae8fa7daf938.png)
执行gcc：yum -y install gcc automake autoconf libtool make
查询gcc版本：gcc -v
![在这里插入图片描述](https://img-blog.csdnimg.cn/251cecfaa3f7403d961d4950f1071074.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
下一步进入redis目录下：cd redis-6.2.6/
清空上次编译：make distclean
执行命令：make
![在这里插入图片描述](https://img-blog.csdnimg.cn/8815b53ab4c145109e56718a5e98e023.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
创建命令快捷方式：make install
![在这里插入图片描述](https://img-blog.csdnimg.cn/6bb3adaebc2549669c9651aaba3e7416.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_14,color_FFFFFF,t_70,g_se,x_16)
启动Redis（前台）：redis-server
启动Redis（后台）：redis-server &
![在这里插入图片描述](https://img-blog.csdnimg.cn/27645b0aa19e43299a1fc554a2d0d98b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
查看redis服务：ps -ef|grep redis
![在这里插入图片描述](https://img-blog.csdnimg.cn/a9cc508d28da47779fcac91a2dd3b303.png)
# Redis命令与使用
## Redis服务/客户端
> 启动Redis服务后台：redis-server &
> Redis默认端口号：6379
> 启动Redis服务时，指定配置文件：redis-server redis.conf &
> 配置文件在redis目录下的redis.conf
> ****
> ***关闭Redis服务***
> 关闭服务命令：redis-cli shutdown
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/607ed44786334187b7dfae7786e108f1.png)
> ****
> ***进入Redis客户端***
> redis默认ip为：127.0.0.1
> redis默认端口为：6379
> 默认连接127.0.0.1:6379
> 连接客户端命令：redis-cli
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f0c6db1120bc4121bfe6615722ed8440.png)
> 指定端口号连接：redis-cli -p 端口号
> 指定ip地址和端口号连接：redis-cli -h ip地址 -p 端口号
> ****
> ***退出Redis客户端***
> 退出客户端命令：exit或者quit
> ****

## Redis基本知识

> 命令（测试redis服务的性能）：redis-benchmark
> 命令（测试redis的连接稳定，如果没问题会显示PONG）：ping
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/744a2feec93b4abd810f72209e0362c9.png)
> 命令（查看redis服务器的统计信息）：info
> 查看指定统计信息：info 信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/218bc3c0577c44aa83950884a792794b.png)
> ****
> ***Redis的16个数据库实例***
> Redis中的数据库只能有redis服务来创建和维护，开发人员不能修改和自行创建数据库
> Redis在启动的时候默认自动刚创建了16个数据库实例
> 16个数据库没有名字只有编号，从0开始到15，在进入redis客户端默认进入0号数据库
> 可以通过redis,conf配置文件指定创建数据库个数
> ****
> ***切换数据库***
> 如果想从0号数据库切换到其他数据库，命令：select [index]
> 比如要进入1号数据库：select 1
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1cdacfd91eba49aca7dbb91e273f8261.png)
> ****
> ***查看当前数据库的key数***
> 命令（可以查看当前数据的所有数据条数）：dbsize
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/28210d370cb64ff98d0bfed9bce5c5ea.png)
> 0号数据库默认创建四个实例数据
> ****
> ***查看当前数据库中的所有key***
> 命令（redis数据库中基本都是以键值对的形式存储数据key——value）：keys *
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1c7fe3c71e5443f5a3b97e65ac1795f4.png)
> ****
> ***清空数据库***
> 命令：flushdb
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/5859dff5cbab4ab0bb1604f61603b58e.png)
> ***清空所有数据库的数据***
> 命令：flushall
> ****
> ***查看redis中的配置信息***
> 命令（查看所有配置）：config get *
> 查看指定配置信息：config get 配置信息
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/6b025697b90240f58589b7d3de0b0f4f.png)
## Redis数据结构
> Redis的五种数据类型：
> 1：string字符串
> 特点：单个key对应单个value，比如key1 value1
> ****
> 2：list列表
> 特点：单个key对应多个value（有序value），key1 -value1 -value2 -value3
> ****
> 3：set集合
> 特点：单个key对应多个value（无序value），key1 -value -value -value
> ****
> 4：pojo
> 特点：存储对象类型：key1 student(stuId:1,stuName:zs)
> ****
> 5：zset
> 特点：单个key对应多个value（根据指定的值排序）
> 比如根据age排序key1 [-value1 age 12] \[-value2 age 13] \[-value3 age 18]
### Redis对key的操作命令
> 命令（查询数据库的所有的key）：keys *
> 通配符\*号表示匹配0个或多个字符
> keys k\*（表示查询数据库中以k开头的key）
> keys h\*o（查询数据库中以h开头以o结尾的数据）
> 通配符?表示匹配一个字符
> keys h?o（表示查询以h开头o结尾并且中间只有一个字母的，全部只有三个字母的数据）
> 通配符\[]，用来匹配\[]内的一个字符
> keys h[abce]llo（表示查询数据库中以h开头llo结尾，并且h后边只能取\[abce]内的其中一个字符）
> ****
> 判断key在数据库中是否存在
> 命令：exists key
> 判断多个key是否存在：exists key1 key2 key3
> 如果key存在返回1，不存在返回0
> ****
> 移动指定key到指定的数据库实例
> 把key移动到一号数据库的命令：move key名 数据库编号
> ****
> 查看指定key的剩余生存时间
> 默认key没有设置生存时间会返回-1
> 没有查询到key返回-2
> 命令（以秒为单位）：ttl key
> ****
> 设置key的最大生存时间命令：expire key 秒
> ****
> 查看指定key的数据类型
> 命令：type key
> ****
> 修改key的名称
> rename key 新名字
> ****
> 删除key（删除一条数据）
> 返回实际删除成功的数量
> 命令：del key
> 删除多个key：del k1 k2 k3 k4
### string字符串数据类型
> ***添加string数据***
> 添加命令：set key键 value值
> 注：如果新添加的key和数据中的key重名，会覆盖掉之前的key
![在这里插入图片描述](https://img-blog.csdnimg.cn/450f897e67774b9ba7a780580883281b.png)
> ****
> ***根据key获取value***
> 命令：get key键
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/de6375f5a3864184bc96f4a52911c3ed.png)
> ****
> ***追加字符串***
> 命令：append key value
> 返回追加之后的字符串长度
> 如果key不存在，自动创建新的key，并且value设置
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/df0a2f464b1d44d6b01c6c2105d7b952.png)
> ****
> ***获取字符串的长度***
> 命令：strlen key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/463dca1d4ad449feb799b9567159e0e2.png)
> ****
> ***数值的字符串加1运算***
> 命令：incr key
> 如果key不存在，会创建一个key，值初始化为0然后进行加1运算
> 注：value必须是数值，不能是字符
![在这里插入图片描述](https://img-blog.csdnimg.cn/cbb8d91e373740acbfad215736e44431.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b8332503b6ac40e6bdd5f6460996d1b5.png)
> ****
> ***数值的字符串减1运算***
> 命令：decr key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d8cf1b229ba84b80b7f649a4b5e5ea21.png)
> ****
> ***将数值字符串添加指定数值***
> 命令：incrby key 数值
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/454a4dd39c3043d89b976640f9cf5b28.png)
> ****
> ***将数值字符串减指定的数值***
> 命令：decrby key 数值
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/abfc6e0bd22d4823a0df3c3f2e1a186e.png)
> ****
> ***获取字符串的子串***
> 命令（闭区间）：getrange key 开始下标 结束下标
> 获取的下标可以为负数（负数从右到左取）
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/2a4daeeaeccc4689aa2807652a97d149.png)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/94e848c029d5435889b769181a4c9daf.png)
> ****
> ***覆盖字符串***
> 命令：setrange key 开始下标 value
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/8ba8bbe6f27149af894c96d864d7a637.png)
> ****
> ***设置key的同时，指定最大生命周期***
> 命令：setex key 秒 value
![在这里插入图片描述](https://img-blog.csdnimg.cn/838e2d9f0fd440f4be21e6e699fa0eff.png)
> ****
> ***设置key与value，当key不存在时设置成功，当key存在时，设置失败***
> 命令：setnx key value
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/4daba53156ea44d0969f661e79e1af17.png)
> ****
> ***批量设置key与value***
> 命令：mset key键1 value值 key键2 value值 key键3 value值
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/68a2c6a0c2024d10bda5e5b03ea308e5.png)
> ****
> ***批量根据key获取value***
> 命令：mget 键1 键2 键3
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/98dfb92844f147bfacdf3b55a6362180.png)
> ****
> ***批量设置当key不存在时设置成功，当key存在时设置失败***
> 命令：msetnx 键1 值 键2 值 键3 值
> 注：只要有一个key设置失败，则全部放弃设置
### list列表数据类型
> ***单个key对应多个value，最左侧是表头，最右侧是表尾，每个元素都有下表，表头下标为0依次递增，表尾的下标为-1***
> ****
> ***将一个或者多个值依次按照顺序插入到列表的表头***
> 命令：lpush key value value value
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f56cf5b7014f4ddbbb4527b9f5171d5a.png)
> ****
> ***获取指定列表的下表区间的元素***
> 命令：lrange key 开始下标 结束下标
![在这里插入图片描述](https://img-blog.csdnimg.cn/f6eb6fc7657d4561a4609981a76cb0a7.png)
> ****
> ***将一个或者多个值依次按照顺序插入到列表的表尾***
> 命令：rpush key value .....
![在这里插入图片描述](https://img-blog.csdnimg.cn/f1dd0d12a9004f2bbccc47c3bb6ef163.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/69948e13192742f29e4b48b865bf2334.png)
> ****
> ***删除一个列表中的表头数据***
> 删除并且返回表头元素
> 命令：lpop key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d88d6b94064c492fafba78086fb862a0.png)
> ****
> ***删除一个列表中的表尾数据***
> 命令：rpop key
> ****
> ***获取列表中指定下标的元素***
> 命令：lindex key 下标
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/91643120a5834be288e1aa3b671e823a.png)
> ****
> ***获取指定列表的长度***
> 命令：llen key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/abbbf05206f54c2a988e6540de263a05.png)
> ****
> ***移除指定列表中指定条件的数据***
> 命令：lrem key 数量 条件
> 数量的开始下标如果是-1则从右边开始向左删，如果是大于0从左边开始
> 如果lrem key 0 条件（这个表示删除所有符合条件的元素）
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/13149065e4b3428cbceedf9fe52b6fac.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/bc8a1902b4fe4ee086d1bc942268cb1b.png)
> lrem list 1 3表示从左边开始删除value是3的一个元素
### set集合数据类型
> ***一个key对应多个value，而且value无序，不可重复，直接通过value操作数据***
> ****
> ***将一个数据或多个数据添加到集合***
> 命令：sadd key value value value
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/536d0a7577b343b39b518aaa6826047d.png)
> ****
> ***获取集合中所有元素***
> 命令：smembers key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d5602c5d5d4845099a79da9ac399ca2f.png)
> ****
> ***判断指定元素在集合中是否存在***
> 命令：sismember key value
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d2c3aede97c7483080be65730d2fb479.png)
> ****
> ***获取集合的长度***
> 命令：scard key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/62e5b29802d64ab39bbf1993279e3f29.png)
> ****
> ***删除集合中一个或多个元素***
> 命令：srem key value value value
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d58c0a1491cc49f39b9a4750bf6fcf5f.png)
> ****
> ***随机获取集合中的一个元素***
> 命令：srandmember key
> 获取多个随机元素：srandmember key 数量
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f2362107c7c542c197199e4fa21f37d8.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f0648c337bc541fcaf0de5c05465b75d.png)
> 随机获取多个元素的元素不会重复
> ****
> ***从集合中随机移除一个或多个元素***
> 命令：spop key 个数
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0701d418b7e14253af02c836e375883c.png)
> ****
> ***将指定集合中的指定元素移动到另一个集合***
> 命令：smove key key 元素
> ****
> ***返回一个集合中有，其它集合中没有的元素（求差集）***
> 命令：sdiff key key key
> ****
> ***获取所有指定集合中都有的元素组成新的集合（求交集）***
> 命令：sinter key key key
> ****
> ***获取所有指定集合中所有元素的大集合（求并集）***
> 命令：sunion key key key
### hash哈希数据类型
> ***添加一个或多个键值对哈希数据***
> 命令：hset 哈希表的key 属性的key1 属性的值value 属性的key2 属性的值value
> 如果属性key重复会覆盖之前的属性key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/54918c23fc0e424584fe0c5faabb7d91.png)
> ****
> ***获取哈希表中的指定的属性值***
> 命令：hget 哈希表key 属性key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/01eeaef4b2b44039830cf2bc9d12d4b2.png)
> ****
> ***可以批量将多个属性key和value设置到哈希表中***
> 命令：hmset 哈希表的key 属性的key1 属性的值value 属性的key2 属性的值value
> 功能和hset相同
> ****
> ***批量获取多个哈希表中的属性值***
> 命令：hmget key 属性key 属性key
![在这里插入图片描述](https://img-blog.csdnimg.cn/b5493035bbdb4e35a0e20a3cd4d57ff9.png)
> ****
> ***获取哈希表中所有的属性和值***
> 命令：hgetall key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/cc258bf129614e679871bdfe21b228b3.png)
> ****
> ***删除哈希表中一个或多个的属性和值***
> 命令：hdel key 属性key 属性key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/681de807405d4ff4833b2b7927ae833a.png)
> ****
> ***获取哈希表中的所有的属性个数***
> 命令：hlen key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/97e0931527234368841f61922e3fbe71.png)
> ****
> ***判断哈希表中是否存在指定的属性***
> 命令：hexists key 属性key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f0549241aa1b4c3898877526204bd41b.png)
> ****
> ***获取哈希表中属性或者值***
> 命令：hkeys key
> 获取值：hvals key
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1da4a5a0756444fba5666668bf4acfac.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9044ec43fac74f41a5ade1309700d8b5.png)
> ****
> ***如果是属性的值是数值型执行运算***
> 整数加法命令：hincrby key 属性key 数值
> 浮点型加法运算：hincrbyfloat key 属性key 数值
> ****
> ***如果属性key存在则放弃设置，如果没有属性key，则设置属性key***
> 命令：hsetnx key 属性key 值 属性key 值
### 有序zset集合数据类型
> ***zset和set一样也是string类型元素的集合，但是不允许元素重复，按照自己业务的数值进行排序***
> ****
> ***添加一个或多个有序集合数据***
> 命令：zadd key 排序数值 value 排序数值 value
> 如果元素已经存在，添加重复会覆盖掉排序数值
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d728a88eda944f8db431236d9a87226f.png)
> ****
> ***获取指定区间的元素***
> 命令：zrange key 开始下标 结束下标
> 获取元素及排序数值：zrange key 开始下标 结束下标 [withscores]
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/7f35dd4475f84863a4f5379b15d18afe.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ad751810926044a4b85e4371d9de721f.png)
> ****
> ***根据排序数值获取数据（闭区间）***
> 命令：zrangebyscore key 排序数值最小 排序数值最大 [withscores]
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/4083b013f7bf4b13aca4ae318451502e.png)
> ****
> ***删除指定的一个或多个数据***
> 命令：zrem key 元素 元素
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/f66f12c791294ac1a8290707bff74b11.png)
> ****
> ***获取集合的长度***
> 命令：zcard key
> 获取指定集合中排序数值在指定区间内的元素个数：zcount key min max
> 根据元素获取排序数值：zscore key 元素
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/fc08ebbff8824ff1a8cb52fad585f2a1.png)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d3bc2214235c4e38a391404f8753bc09.png)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/eb50cb39d73e48be97660dfd8ac9b3c9.png)
> ****
> ***获取集合中指定的元素的排名***
> 命令：zrank key 元素
> 获取集合中某个元素的排序（反向）：zrevrank key 元素
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/913b7b2416f14ea4a92d1fc2de3a67dc.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/af0e05c6aae945a58d955954e8a2554e.png)
# Redis配置文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/671f26c7f101457f82d2c86b7af25af5.png)
> 如果不使用配置文件redis会按照默认参数运行
> 如果修改过配置文件使用配置文件，启动redis服务时需要指定配置文件

***redis中关于网络的配置***
> port（端口号）：默认6379
> bind（ip地址）：默认127.0.0.1
> 如果配置了端口和ip启动服务时必须指定端口号和ip
> 连接客户端：redis-cli -h ip地址 -p 端口号
> 如果关闭服务也要指定：redis-cli -h 地址 - 端口号 shutdown
> ****
> tcp-keepalive（TCP保活策略）：服务器每隔一段时间会向客户端发起一个虚拟请求，来查看客户端是否存活

## Redis持久化

> Redis是一种内存数据，大多数数据是存在内存的，Redis也可以将数据存到本体硬盘中，延长数据的生命周期
### RDB策略
> 在指定时间间隔内，redis服务执行指定次数的写操作，会自动触发一次持久化操作
> redis的默认时间间隔和次数（以秒为单位）900 1 = 900秒内修改一次会执行写操作
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/768b124f7a2e4216988e1cb779637553.png)
> 指定持久化数据保存的文件（配置）：dbfilename
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/29b27881e5cf4f39a62eb64cc63113df.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20641a21bc9e4f81a5da33a7548c8410.png)
指定目录：./
### AOF策略
> 采用操作日志来记录进行每一次写操作
> 每次redis服务启动时，都会执行一次日志内的操作
> 缺点：效率较低
> 配置（表示是否开启AOF，yes表示开启）：appendonly
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/73f1f8a49af34911a62a2aef41953833.png)
> 日志文件名
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/89f58b496a9f45d9aa07addeaeffd5ce.png)
# Redis事务机制
> 大多数数据库的事务把一组数据库的操作放在一起执行，保证操作的原子性（要么同时成功，要么同时失败）
> Redis的事务把一组命令放在一起执行，把命令进行序列化执行，但不能保证原子性

***事务命令***

> ***标记事务的开始***
> 命令：multi
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/5352fa7e23224c7f80816b0ec8e844ab.png)
> ****
> ***执行事务队列中所有的命令***
> 命令：exec
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a6243aa143445f2a77fe922d9939236.png)
> ****
> ***如果一组命令中，又在压入队列过程中发生错误的命令，则本事务中所有的命令都不执行，保证事务的原子性***
> ***如果一组命令中，在压入队列过程中正常，在执行事务的时候发生错误，则其他命令正常执行，不能保证事务的原子性***
> ****
> ***放弃已经压入队列的命令***
> 命令（清除所有已经压入队列的命令，并且结束整个事务）：discard
> ****
> ***监控某一个键key，当事务在执行过程中，此键key的值发生变化，则本事务放弃执行***
> 命令：watch key
> 放弃监控：unwatch key
# Redis消息的发布与订阅
> redis客户端订阅频道，消息的发布者往频道上发布消息，所有订阅此频道的客户端都可以收到这个消息
> ****
> ***订阅一个或多消息***
> 命令：subscribe 频道名
> 支持通配符的订阅命令：psubscribe 频道名
> ****
> ***将消息发布到指定频道***
> 命令：publish 频道名 消息
# Redis的主从复制（集群）
> 主机数据更新后根据配置和策略，自动同步到从机的master/slave机制，master以写为主，slave以读为主
> 主写从读，读写分离

***集群搭建***

> 搭建三台redis服务
> ***正常多台redis服务都是一个linux系统使用一个redis服务，从而搭建多台linux系统***
> 这里直接在一台linux系统上搭建三台服务
> 复制redis.conf配置文件多份
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/8f601d80093c43b9a7166072edc0bdb2.png)
> 修改每个配置文件的参数：
> bind 127.0.0.1
> port 端口号
> pidfile
> logfile
> dbfilename
> ****
> ***使用三个配置文件启动redis服务***
> redis-server redis端口号.conf &
![在这里插入图片描述](https://img-blog.csdnimg.cn/1fad36762fe449fab0e458e3aabad316.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b626d00b397a46e68c0d0ba0e2415d76.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5c86dc8ad7bf458db825adec8280dc83.png)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/5d6e0547aac14405948470c0f3d61593.png)
> ***查看三台redis服务在集群中的主从角色***
> 命令：info Replication
> 默认情况下所有的reids服务都是主机
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/cfaacb22917245a5a7020776bb628643.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> ***设置主从关系***
> 在6380和6381上设置从机
> 命令：slaveof 127.0.0.1 6379
![在这里插入图片描述](https://img-blog.csdnimg.cn/461f8f42451040a782f1fd01233c26e6.png)
> 此时主机从机的数据都同步了
> 全量复制：一旦主从关系确定，会自动把主机上已有的数据同步复制到从库
> 增量复制：主机写数据会自动同步到从库
> 注：在从机上不能增添数据，从机可以读数据
> ****
> ***如果主机宕机，从机原地待命***
> ***如果从机宕机，主机少一个从机，其他从机不变，如果从机恢复需要重新设置从机的主从关系***
> ***从机上位：如果主机宕机无法修复，上位的从机断开主从关系（命令：slaveof no one）***
> ***然后重新设置其他从机的主从关系***
> ***如果之前宕机的主机修复好了，可以将宕机的机器加入到集群，变成从机主机都可以***
> ***注：一个从机还可以有它的从机，一个主机还可以有它的主机***
## Redis哨兵模式
> 作用：查看主机宕机时间，设置从主关系，及时补损
> ****
> ***部署哨兵***
> 提供哨兵的配置文件（默认是）sentinel.conf
> 建议自己创建文件：redis-sentinel.conf
> 文件内容：sentinel monitor dc-redis 127.0.0.1 6379 1
> 启动哨兵服务
> 命令：redis-sentinel redis-sentinel.conf &
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/14168331dd25453e88ecbe8137ec376a.png)
> 如果主机宕机后：
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/eada7a9420cc4f74a9b357aabac64f9d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATG90eg==,size_20,color_FFFFFF,t_70,g_se,x_16)
> 哨兵会进行投票，让其他某一个从机成为主机
> 如果主机恢复，哨兵会让他成为从机
# Jedis操作Redis
> Jedis是在java应用中操作Redis
> 在redis中的指令和jedis中的方法同名，创建Jedis对象调用即可
> 而事务也是一个事务对象，调用事务对象的方法即可开启事务等操作

***创建maven项目***
![在这里插入图片描述](https://img-blog.csdnimg.cn/b0d47b88f2a64cbfb9cfd2d74196c1cb.png)
***加入依赖***
```html
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.0.0</version>
</dependency>
```
***关闭linux防火墙***
> systemctl stop firewalld
> 修改配置文件bind
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/52bb102b5caf47749bf7760ddcd6c729.png)
```java
package com.redis;

import org.junit.Test;
import redis.clients.jedis.Jedis;

public class TestApp {
    @Test
    public void test(){
        //连接redis, 参数：ip 端口号
        Jedis jedis = new Jedis("192.168.73.132",6379);
        //使用jedis对象操作redis的ping指令
        System.out.println(jedis.ping());
    }
}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/17fee5cb86404eb9870f8a41af7a1805.png)

