# 介绍
是基于Hadoop的数据仓库，将结构化的数据文件映射成表，提供类Sql查询功能。
Sql会转化为MapReduce作业，然后提交到Hadoop运行；通过表查询数据，底层会查询hadooop对应的文件数据

## 特点
- 提供类sql查询语言hql，门槛低
- 灵活性高，可以自定义用户函数UDF及存储格式
- 支持超大数据集的存储、计算，集群可扩展
- 统一元数据管理，可与presto/impala/sparksql共享数据
- 执行延迟高，不适合实时数据处理，适合海量数据的离线处理

## 操作方式
- 命令行shell:交互式命令行
- thrfit/jdbc

## 元数据Metastore
- 表名
- 表结构
- 字段名
- 字段类型
- 表分隔符
- 字段分隔符
- ...等

元数据默认在HIVE内置的derby数据库，实际生产通常用Mysql。
元数据统一管理，在HIVE、Presto、impala、sparksql某个里对元数据的更改，其余都能直接感知、复用

## HQL执行流程
HIVE执行HQL
- 语法解析（Antlr：sql->ast tree）
- 语义解析（ast tree-> QueryBlock）
- 生成逻辑执行计划（queryBlock->OperatorTree）
- 优化逻辑执行计划
- 生成物理执行计划：遍历OperatorTree->MapReduce任务
- 优化物理执行计划

## 数据类型
- TINYINT/SMALLINT/INT/BIGINT
- BOOLEAN
- FLOAT/DOUBLE
- DECIMAL
- STRING（指定字符集）/VARCHAR（最大长度限制）/CHAR（固定长度）
- TIMESTAMP/TIMESTAMP WITH LOCAL TIME ZONE
- DATE
- BINARY


## 隐式转换
## 复杂类型
-STRUCT
-MAP
-ARRAY

## 内容格式：分隔符
- \n 换行符分割记录
- ^A 分割字段 \001
- ^B 分割Array、struct、map, \002
- ^C Map中键值对的分割、003

## 存储格式
Hive的数据库对应HDFS的一个目录，表对应目录下的子目录，表的数据存储在对应表目录下。
### Textfile
- 纯文本
- 默认
- 不压缩
- 磁盘开销大
- 解析数据开销大

### SequenceFile
- Hadoop API提供的一种二进制文件
- <key,value>形式序列化（Hadoop标准的Writable接口）到文件中；key为空，避免排序
- 与Hadoop API的MapFile兼容

### RCFile
- facebook开源
- 行组、行组内列存储，每列数据分开存储

### ORC Files
- 对RCFile优化

### Avro Files
- 数据序列化系统，支持二进制序列化
- 支持大批量数据交换
- 动态语言友好

### Parquet
- 基于Dremel，面向分析型业务的列式存储
- 按列压缩、编码

推荐使用ORC、Parquet，综合性能突出

## 指定存储格式
- STORED AS TEXTFILE
- STORED AS SEQUENCEFILE
- STORED AS ORC
- STORED AS PARQUET
- STORED AS AVRO
- STORED AS RCFILE

## 内部表、外部表
### 内部表
管理表Managed Table，默认。
- 存储位置hive.metastore.warehouse.dir指定，默认在HDFS的/user/hive/warehouse/数据库名.db/表名/目录下
- 导入数据后，HIVE管理内部自己数仓目录下的数据及数据生命周期
- 删除表会删除元数据和数据文件

### 外部表
External table，使用External修饰
- 存储位置创建表时由 Location 参数指定
- 外部表不会将数据移动到数仓目录下，只是元数据存储了数据的位置
- 删表时：只删除元数据

# 安装、使用
## HiveServer
- HiveServer不支持多客户端的并发请求
- HiveServer2支持多并发
- HiveServer2的Beeline-CLI工具重点维护，非Hive CLI

## Hive CLI
### 命令行执行sql语句：
```sql
hive -e 'sql 语句'
```

...

## Beeline
..

# hive常用ddl
- database
  - 查看列表
  - 使用某个库
  - 新建库
  - 查看库信息
  - 删除库
- 创建表
  - 创建
  - 内部、外部表
  - 分区表（PARTITIONED BY，不用扫描整个表，只需扫描指定分区表，常用）
  - 分桶表（CLUSTERED BY，SORTED BY，加载数据只能通过CTAS方式触发MR，不能通过load）
  - 倾斜表
  - 临时表
  - CTAS
  - 复制表结构
  - 加载数据到表
- 修改表
  - 重命名
  - 修改列（字段名、类型、字段顺序、注释）
  - 新增列
  - 清空表（内部表的整个表或者指定分区）
  - 删除表（内部表删除数据+元数据，外部表只会删除元数据）

# 视图
分类：
- 逻辑视图
- 3.0.0物化视图

特点：
- 只读
- 修改表不会更新视图
- 删除表不会删除视图

操作：
- 增删改查
- 修改视图属性

# 索引（3.0版本移除）
提升查询速度，否则会处理整个表或者整个分区
原理： 索引表(索引列的值，该值对应HDFS文件路径，该值在文件的偏移量)

操作：
- 增删改查、重建

注意：
- 索引生效需要通过配置开启
- 索引表无法自动rebuild（表数据新增、删除时，无法更新）
- 3.0移除：物化视图；列式存储格式ORC、Parqueet支持选择性扫描；两种等同索引效果

# DML
- load文件数据到表
  - 本地文件
  - HDFS的文件
  - 追加/覆盖
  - 加载到表/某个分区
  - 文件格式与表格式要相同
- 查询结果插入到表
- sql插入数据
- 更新、删除数据（只能在支持事务ACID的表上执行）
- 查询结果写出到文件系统

## HIVE的事务表限制
- 必须时bucket table
- 仅支持ORC格式
- 不支持LOAD DATA语句

# HIVE数据查询
## 单表
- SELECT
- WHERE
- DISTINCT
- 分区查询
- LIMIT
- GROUP BY
- ORDER BY（全局有序，时间长, hive.mapred.mode&limit）
- SORT BY（局部有序，全局不保证）
- HAVING
- DISTRIBUTE BY(相同key的发送到同一个reducer处理，结合SORT BY保证单个reducer顺序)
- CLUSTER BY（DISTRIBUTE BY与SORT BY的是同一个字段，且SORT BY是ASC时；全局有序）

## 多表
- 内INNER JOIN .. ON
- 外FULL OUTER JOIN ..ON
- 左外LEFT OUTER JON ..ON
- 右外RIGHT OUTER JOIN ..ON
- 左半连接：LEFT SEMI JOIN
- 笛卡尔连接JOIN 开销大

### JOIN优化
- /*+ STREAMTABLE() */标注数据最大表
- /*+ MAPJOIN() */标注最小表

# 附录
- https://github.com/heibaiying/BigData-Notes/blob/master/notes/Hive%E7%AE%80%E4%BB%8B%E5%8F%8A%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5.md
- https://cwiki.apache.org/confluence/display/Hive/GettingStarted
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
- https://tech.meituan.com/2014/02/12/hive-sql-to-mapreduce.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Types
- https://cwiki.apache.org/confluence/display/Hive/Managed+vs.+External+Tables
- https://cwiki.apache.org/confluence/display/Hive/AdminManual+Configuration
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL+BucketedTables