#### value数据类型
- string
  - 二进制安全
- list
  - 有序、可重复
- set
  - 无序、不重复
- hash
  - field-value的map
- sorted set
  - 不重复、通过double类型score分数排序

#### 场景
- string
  - 计数器
  - 缓存
  - 分布式锁
  - 访问频率控制
  - 分布式session
- hash
  - 购物车等对象属性灵活修改
- list
  - 定时排行榜
- set
  - 收藏
- sorted set
  - 实时排行榜

#### 持久化
- RDB
  - 定时全量、完整性低、二进制文件小、恢复快
  - 备份、全量复制场景
- AOF
  - 写操作、完整性高、redis操作文件大、恢复慢、相对影响写入
  - 灾难性的误删除紧急恢复

#### 快的原因
- 纯内存
- 非阻塞的IO多路复用
- 避免线程上下文切换

#### 缓存常见现象
- 缓存穿透
  - 缓存无，db无，恶意攻击此类数据
- 缓存击穿
  - 某个热点key失效，走db
- 缓存雪崩
  - 大批量key失效，走db
#### 淘汰策略
- noeviction
- allkeys-lru
- allkeys-random
- volatile-random
- volatile-ttl
- allkeys-lfu
- volatile-lfu

#### 客户端
- jedis
- redission
- lettuce

#### 更新机制
- 先更新db，再更新缓存
  - 并发脏数据问题
- 先删除缓存，在更新db
  - 读请求触发写缓存导致脏数据
- 先更新db，再删缓存
  - 处理缓存比db操作快？

#### pipeline
- 非原子
- 不支持事务
- 批量读、批量写