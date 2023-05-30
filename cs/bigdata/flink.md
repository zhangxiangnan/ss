# 简介
柏林工业大学的研究项目，目前最火热的大数据处理框架，分布式流处理框架。flink核心是流处理，将批处理当作流处理，spark streaming相反。

# 架构
## API & Libraries
API：（DataStream API，DataSet APi）
类库：复杂事件处理CEP库；结构化数据查询SQL & TABLE库；批处理的机器学习库FlinkML；图形处理库Gelly

## Runtime核心层
flink分布式计算框架的核心实现层，作业转换，任务调度，资源分配，任务执行等

## 物理部署层
支持不同平台部署flink应用

# 分层API
## SQL & Table API
同时适用于批处理、流处理，支持自定义标量函数、聚合函数、表值函数

## DataStream API，DataSet APi
数据处理的核心APi，封装了数据读取、数据转换、数据输出

## Stateful stream processing
Process Function是最底层API，对时间、状态进行细粒度控制

# 集群架构
Master-Slave架构
- JobManagers（masters）
- TaskManagers(workers)
- Dispatcher
- ResourceManager（Yarn、mesos、k8s）

## Task & Subtask

## Actor System进行组件间通讯

# 优点
- 基于事件驱动，同时支持流处理、批处理
- 基于内存计算，保证高吞吐、低延迟，性能优越
- 支持精确一次语义，完美保证一致性和准确性
- 分层API
- 高可用、保存点机制，保证安全性与稳定性
- 多样化部署，本地、远端、云端等
- 横向扩展
- 社区活跃、完善生态圈

# flink data source
## 内置data source
- 基于文件
- 基于集合
- 基于Socket

## 自定义data source
- SourceFuntion
- ParallelSourceFuntion
- RichParallelSourceFuntion

# 连接器
## 内置连接器
- Apache Kafka（source & sink）
- Apachecassandra（sink）
- Amazon kinesis streams（source & sink）
- Elasticsearch（sink）
- HDFS(sink)
- RabbitMq（source & sink）
- Apache Nifi（source & sink）
- twitter streaming api（source）
- google pubsub（source & sink）

## 扩展连接器（通过apache bahir）
- APache activeMq（source & sink）
- Apache flume（sink）
- redis（sink）
- akka（sink）
- netty（source）

## 整合kafka

# Transformation
## DataStream Transformations
- Map
- flatmap
- filter
- keyby & reduce
- aggregations
  - sum、min、max、minBy、maxBy等
- union
- connect（comap coflatmap）
- split、select
- project

## 物理分区
- random partitioning
- rebalancing
- rescaling
- broadcasting
- custom partitioning

# 任务链 资源组
- startNewChain
- disableChainging
- slotsharingGroup

# Flink sink
## data sinks
- writeAsText
- writeAsCsv
- rpint printToErr
- witeUsingOutputFormat
- writeToScoket

## Streaeming connectors
## 自定义sink

# Flink windows
窗口window
## 时间窗口
### Tumbing windows
滚动窗口，彼此之间无重叠

### Sliding Windows
滑动窗口，存在重叠

### Session window
会话窗口
### global window
全剧窗口，key相同的在一个窗口里

## 计数窗口
用于以数量为维度进行数据聚合，分滚动、滑动窗口

# 状态管理
支持有状态计算，即保存中间计算结果
## 算子状态operator state
状态和算子绑定，一个算子的状态不能被其他算法访问到
## 键控状态keyed state
keyedSteam
## 键控状态编程
- ValueState
- ListState
- ReducingState
- AggregatingState
- MapState
## 状态有效期

## 算子状态编程
- ListState
- UnionListState
- BroadCaseState

## 检查点机制checkpoint

## 状态管理器
- MemoryStateBackend
- FsStatebackend
- RocksbDBStateBackend

### 配置方式
- 代码，当前作业
- 配置，全局

# 部署
## 单机
## standalone cluster模式
自带的集群模式
Standalone Cluster HA高可用模式
## 第三方平台YARN、MESOS、DOCKER、K8s等

# 附录
- https://nightlies.apache.org/flink/flink-docs-release-1.9/concepts/programming-model.html
- https://nightlies.apache.org/flink/flink-docs-release-1.9/dev/connectors/index.html
- https://ci.apache.org/projects/flink/flink-docs-release-1.9/dev/stream/operators/
- Flink Windows： https://ci.apache.org/projects/flink/flink-docs-release-1.9/dev/stream/operators/windows.html
