# MySql Index
## 索引类型
* index（普通索引）
* unique（唯一索引）
* fulltext（全文索引）

## explain columns
#### select_type
* `simple`表示简单查询，没有union和子查询
* `primary`union或子查询第一个查询是primary
* `union`union查询除了`primary`之外都是`union`类型
* `dependent union`
* `union result`
* `subquery`
* `dependent subquery`
* `derived`

#### table
表名，如果是<>可能是union或子查询产生的临时表
#### type
* `system`表中只有一行数据或者是空表，且只能用于myisam和memory表。如果是Innodb引擎表，type列在这个情况通常都是all或者index
* `const`使用唯一索引或者主键，返回记录一定是1行记录的等值where条件时，通常type是const。其他数据库也叫做唯一索引扫描
* `eq_ref`出现在要连接过个表的查询计划中，驱动表只返回一行数据，且这行数据是第二个表的主键或者唯一索引，且必须为not null，唯一索引和主键是多列时，只有所有的列都用作比较时才会出现eq_ref
* `ref`不像eq_ref那样要求连接顺序，也没有主键和唯一索引的要求，只要使用相等条件检索时就可能出现，常见与辅助索引的等值查找。或者多列主键、唯一索引中，使用第一个列之外的列作为等值查找也会出现，总之，返回数据不唯一的等值查找就可能出现。
* `fulltext`全文索引检索，要注意，全文索引的优先级很高，若全文索引和普通索引同时存在时，mysql不管代价，优先选择使用全文索引
* `ref_or_null`与ref方法类似，只是增加了null值的比较。实际用的不多。
* `unique_subquery`用于where中的in形式子查询，子查询返回不重复值唯一值
* `index_subquery`用于in形式子查询使用到了辅助索引或者in常数列表，子查询可能返回重复值，可以使用索引将子查询去重。
* `range`索引范围扫描，常见于使用>,<,is null,between ,in ,like等运算符的查询中。
* `index_merge`表示查询使用了两个以上的索引，最后取交集或者并集，常见and ，or的条件使用了不同的索引，官方排序这个在ref_or_null之后，但是实际上由于要读取所个索引，性能可能大部分时间都不如range
* `index`索引全表扫描，把索引从头到尾扫一遍，常见于使用索引列就可以处理不需要读取数据文件的查询、可以使用索引排序或者分组的查询。
* `all`这个就是全表扫描数据文件，然后再在server层进行过滤返回符合要求的记录。

#### possible_keys
查询可能使用到的索引
#### key
查询真正使用到的索引，select_type为index_merge时，这里可能出现两个以上的索引，其他的select_type这里只会出现一个。
#### key_len
用于处理查询的索引长度，如果是单列索引，那就整个索引长度算进去，如果是多列索引，那么查询不一定都能使用到所有的列，具体使用到了多少个列的索引，这里就会计算进去，没有使用到的列，这里不会计算进去。留意下这个列的值，算一下你的多列索引总长度就知道有没有使用到所有的列了。要注意，mysql的ICP特性使用到的索引不会计入其中。另外，key_len只计算where条件用到的索引长度，而排序和分组就算用到了索引，也不会计算到key_len中。
#### ref
如果是使用的常数等值查询，这里会显示const，如果是连接查询，被驱动表的执行计划这里会显示驱动表的关联字段，如果是条件使用了表达式或者函数，或者条件列发生了内部隐式转换，这里可能显示为func
#### rows
这里是执行计划中估算的扫描行数，不是精确值
#### filtered
表示存储引擎返回的数据在server层过滤后，剩下多少满足查询的记录数量的比例，注意是百分比，不是具体记录数。
#### extra
