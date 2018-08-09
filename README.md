# MYSQL数据库的优化技术


对Mysql优化主要包括

- 表的设计合理化（符合三范式）
- sql语句优化
- 添加适当索引（普通索引、主键索引、唯一索引、全文索引）
- 选择合适的存储引擎（MyISAM、InnoDB）
- 分表设计（水平分表、垂直分表）
- 读写分离
- 对Mysql配置优化（配置最大并发数、调整缓存大小）
- 服务器硬件升级
- 定期进行碎片整理

# 表的设计合理化 3NF
- 1NF 表的列具有原子性，不可在分解。只要是关系型数据库自动满足1NF
- 2NF 表中的记录是唯一的。设计一个主键来实现。
- 3NF 表中不要有冗余数据。为了提高效率，可以反3NF在表中设计冗余字段

# sql语句优化
- 通过show global status命令了解sql的运行情况

  Uptime 运行时长
  
  Com_update Com_delete Com_insert Com_select
  
  Connections 链接次数
  
  slow_queries 慢查询次数
  
  show variables like 'long_query_time' 查询慢查询时间，默认10秒
  
  set long_query_time = 1;
  
  show status like '%Handler_read%';查询索引使用情况

- 把慢查询记录到日志中

  my.cnf配置慢日志
  
  slow_query_log =1
  
  slow_query_log_file=/tmp/mysql_slow.log
  
- MySQL性能分析及explain

  explain select * from test1 where id=1;
  
- 日志分析工具mysqldumpslow
  
# 索引

- 二叉数算法索引文件 BTREE
- 全文索引 只支持MyISAM
- 创建索引字段条件
   
   经常查询的条件
   
   字段内容不经常变化
   
- 索引的使用

  联合索引使用最左边的列，索引一般会使用
  
  LIKE查询如果是'%aaa'不会使用到索引，'aaa%'会使用到索引
  
  如有条件中有OR,就要求所有字段都必须建索引
   
- sql优化

  group by会对列进行排序，可以使用order by null禁止排序
  
# 存储引擎

- InnoDB默认引擎、支持事务 建议使用

  行锁机制

- MyISAM支持全文索引、融灾性差、要求定期碎片整理

  表锁机制
  
  optimize table tablename; 
