# HDFS
分布式文件系统，高容错、高吞吐，可部署在低成本硬件;存储结构化、半结构化、非结构化数据，海量数据存储的最佳方法

## 架构
- NameNode 单个
  - 目录、文件的创建、移动、删除、重命名等操作
  - 配置用户及访问权限
  - 存储集群元数据
- DataNode 多个
  - 读写请求

## 容错性：数据复制
多副本,默认3；
块：默认128M

## 稳定性
- 心跳机制
- 重新复制
- 数据完整性校验：校验和
- 元数据磁盘故障：fsimage、editlog多副本
- 数据快照

## 特点
### 高容错
多副本
### 高吞吐量
重在高吞吐，非低延迟 
### 大文件存储支持
GB到TB 
### 一次写入，多次读取
内容追加到末尾，不支持随机访问数据，不能从任意位置新增数据

## Java API
- 创建目录（支持递归）
- 创建指定权限的目录
- 创建文件，写入内容
- 文件是否存在
- 查看文件内容
- 文件重命名
- 删除目录、文件
- 上传文件到HDFS
- 上传大文件并显示进度
- 下载文件
- 查看指定目录下所有文件信息
- 递归查看指定目录下的文件信息
- 查看文件块信息

# MapReduce
分布式计算框架，并行处理大规模数据
## 流程
- 输入input
- 拆分splitting
- 拆分mapping：业务自定义实现
- 合并shuffling
- 汇总reducing：业务自定义实现

## Combiner
简单合并combiner：有的场景有（总数、最大、最小等，平均值不适用），可极大降低数据传输量
## Partitioner
自定义分类规则

# Yarn
集群资源管理系统，统一资源管理、分配

## 架构
###ResourceManager
###NodeManager
###ApplicationMaster
协调资源、监控资源使用情况、监视任务及容错
### Container
资源抽象，某个节点上的多维度如内存、cpu、磁盘、网络等资源

## 流程
- 作业提交
- 作业初始化
- 任务分配
- 任务运行
- 进度、祖杭台更新
- 作业完成

# 附录
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://zhuanlan.zhihu.com/p/28682581
- https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/YARN.html
- https://hadoop.apache.org/docs/r3.1.2/hadoop-project-dist/hadoop-hdfs/HDFSHighAvailabilityWithQJM.html
- https://hadoop.apache.org/docs/r3.1.2/hadoop-yarn/hadoop-yarn-site/ResourceManagerHA.html
- https://www.cnblogs.com/lushilin/p/11239908.html