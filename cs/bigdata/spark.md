# 简介
加州大学伯利克分校AMPLab,Spark相对于MapReduce批处理计算，性能提升上百倍，分布式计算框架,scala开发。

# 特点
- 先进的DAG调度程序，查询优化器、物理执行引擎，保证性能
- 支持多语言
- 提供足够的API
- 批处理、流处理、复杂的业务分析
- 丰富类库支持：sql、mlib、graphx、spark streaming等，可以无缝组合
- 丰富部署模式：本地、集群、hadoop、mesos、kubernetes
- 多数据源支持：支持访问HDFS、Alluxio、Cassandra、Hbase等数百个

# 集群架构
- Application：spark应用程序，一个Driver节点，多个Executor节点
- Driver program：主应用程序
- Cluster manager：集群资源管理器
- Worker node：计算任务的工作节点
- Executor：应用进程
- Task：工作单元

# 核心组件
## Spark Sql
处理结构化数据
特点：
- 可使用Sql、dataframe api对结构化数据查询
- 多种数据源支持：hive、avro、parquet、orc、json、jdbc
- 支持HiveSql及UDF，能访问已有HIVE库
- 支持标准JDBC、ODBC连接
- 支持优化器、列式存储、代码生成等，提高查询效率

## Spark Streaming
- 用于快速构建可扩展、高吞吐量、高容错的流处理程序，Flink streaming也是流处理器
- 支持从HDFS、Flume、Kafka、Twitter、Zeromq读数据
- 本质是微批处理，将数据进行极小粒度拆分为多个批处理;strom/flink是真正的流计算框架

## MLib
Spark的机器学习库，让机器学习简单、可扩展。
- 常见机器学习算法
- 特征化
- 管道
- 持久性
- 实用工具

## Graphx
图形计算、图形并行计算

# RDD弹性数据集
RDD是spark的最基本数据抽象
- 一个RDD由一个或者多个分区组成，只读
- RDD存储彼此间的依赖关系（某分区数据丢失，可以只计算该分区数据）
- KV的RDD有分区器，hash分区、范围分区
- 一个优先位置列表，存储每个分区的优先位置，计算任务分配到要处理数据的节点上

## 创建RDD
- 现有集合
- 外部存储系统的数据集

## 操作RDD
- 转换transformations，惰性
- 执行actions

## 缓存RDD
缓存级别
- 只缓存在JVM内存中
- 内存和磁盘
- 。。

## shuffle
一般一个任务对应一个分区，不会跨分区操作数据；但是shuffle从所有分区操作数据，极大影响性能。

导致shuffle的操作：
- 重新分区操作：repartition、coalesce
- ByKey的操作：如groupByKey、reduceByKey、除了countByKey
- 联结操作：如cogroup、join

## 依赖类型
- 宽依赖：父RDD的一分区被子RDD的多个子分区依赖
- 窄依赖：父RDD的一分区被子RDD的至多1个子分区依赖

窄可以以流水线计算父分区数据；宽依赖需要计算好所有的父分区数据，然后执行shuffle
窄更有助于恢复有问题分区的数据；宽依赖需要重新计算所有父分区数据再shuffle

## DAG
RDD及之间的依赖关系组成的DAG（有向无环图）

# 常用算子
## Transformation算子
- map
- fiter
- flatMap
- mapPartitions
- mapPartitionsWithINdex
- sample
- union
- intersection
- distinct
- groupByKey
- reduceKey
- aggregateByKey
- sortByKey
- inner join/leftOuterJoin/rightOuterJoin/fullOuterJoin/left semi join/left anti join/cross join/natual join
- cogroup
- cartesian
- coalesce
- repartition
- repartitionAndSortWithinPartitions
## Action算子
- reduce
- collect
- count
- first
- take
- takeSample
- takeOrderd
- saveAsTextFile
- saveAsSequenceFile
- saveAsObjectFile
- countByKey
- foreach

# 部署&作业提交
## 作业提交
spark所有模式都是通过spark-submit命令提交作业：
```sql
./bin/spark-submit \
  --class <main-class> \        # 应用程序主入口类
  --master <master-url> \       # 集群的 Master Url
  --deploy-mode <deploy-mode> \ # 部署模式
  --conf <key>=<value> \        # 可选配置       
  ... # other options    
  <application-jar> \           # Jar 包路径 
  [application-arguments]       #传递给主入口类的参数  

```

deploy-mode：
- cluster
- client

## 部署模式
- Local
- StandAlone（内置的集群模式，通过内置资源管理器管理,单独部署spark集群，一个master，多个worker）
- Spark on Yarn模式（Yarn管理资源，依赖Hdfs&Yarn，不需要启动spark的master、worker节点）

# 累加器&广播变量
共享变量，用于累计计数，节点间分发大对象

# DataFrame&DataSet
## DataFrame
DataFrame（等同于关系数据库的表概念）面向结构化数据，RDDS面向非结构化数据
选择：
- 函数式编程，RDDS
- 数据非结构化，如流媒体，字符流，RDDS
- 结构化数据如RDBMS的表数据，半结构化如日志，DataFrame&dataset
- dataset强类型，datafram非强类型
- Dataset、dataframe、sql底层都依赖RDDS API

## DataSet
集成DataFrame、RDD优点：
- 强类型
- 支持lambda

## spark运行原理
- datafram/dataset/spark sql开发
- 编译，转换为逻辑计划
- 逻辑计划->物理计划，代码优化
- spark在集群上执行物理计划（基于RDD）

# Structured API
## 创建dataframe、dataset
- 创建dataframe
- 创建dataset
- RDD创建dataframe（反射&指定scheme2种）
- dataframes、datasets互相转换
## columns列操作
- 引用列
- 新增
- 删除
- 重命名

## 基本查询(structured api & spark sql)
- select
- filter
- limit
- orderby
- distinc
- groupby
- ..

# 读写外部数据源
- CSV
- JSON
- Parquet,spark默认格式
- ORC
- JDBC/ODBC
- Plain-text files
- 关系数据库，需要上传驱动jar到spark的jars目录
- ...其他上百种

## 读外部数据

## 写数据
- 追加
- 覆盖
- 忽略
- 异常

## 并行读
## 并行写
## 分区写入
## 分桶写入


# SparkSql常用聚合函数
- count
- countDistinct
- approx_count_distinct
- fisrt&last
- min&max
- sum&sumDistinct
- avg
- 数据函数
- 聚合数据到集合
- 分组聚合groupby,grouby.agg
- 自定义聚合

# 流处理器
- 时效性
- 承受更大数据量
- 贴近现实的数据模型
- 适合微服务架构

## Spark Streaming
数据源：
- socket
- 文件系统
- kafka
- flume
- kinesis
- 监听HDFS目录

### DStream
一系列连续的RDD表示
独有算子：
- updateStateByKey

### 整合flume
- 推送：spark streaming监听端口
- 拉取：sparkstreaming从SparkSink接收器拉取数据，基于事务，更可靠，容错强。

### 整合kafka

# 附录
- https://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-programming-guide
- http://shiyanjun.cn/archives/744.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-programming-guide
- https://spark.apache.org/docs/latest/sql-data-sources.html
- https://spark.apache.org/docs/latest/streaming-programming-guide.html
- https://www.ververica.com/what-is-stream-processing