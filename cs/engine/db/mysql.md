### 常用sql模版：common sql template
#### 从其他表查询插入数据到目标表：insert into x select xx from xxx
```sql
insert into t_table1
select c.x1,t.x2
from
t_t1 t, t_t2 c
where
t.id = c.x_id
and ...;
```

#### 更新表的某个字段为同记录的另外字段：update table column value use other column of same record
```sql
update t_x1 t set c1 = (select c1 from (
  select t.id, w.c1 from t_x1 t, t_x2 w 
  where t.id = w.tid
) inn where inn.id = t.id)
```

```sql
update t_xx a inner join t_xx b on a.id=b.id set a.time1 = b.time2
```

#### 增加索引：add  index
```sql
alter table t_x1 add INDEX INDEX_NAME (`name`, `xx`);
```

#### 建表模版
```sql
CREATE TABLE `t_xxxx` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键id',
  ...
  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  `create_user_id` varchar(32) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT '' COMMENT '创建人id',
  `update_user_id` varchar(32) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT '' COMMENT '更新人id',
  PRIMARY KEY (`id`),
  UNIQUE KEY `UK_XX` (`xx1`, `xx2`),
  KEY `IDX_XX3` (`xx3`)
) ENGINE = InnoDB AUTO_INCREMENT = 1 DEFAULT CHARSET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = DYNAMIC COMMENT = 'xxdesc'
```
