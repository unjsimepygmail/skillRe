# MySql  
## 创建索引   
create index 索引名称 on 表名(索引字段1,索引字段2)
## 查看索引是否生效
explain + 具体select 
* id列: id列的编号是 select 的序列号，有几个 select 就有几个id，并且id的顺序是按 select 出现的顺序增长的。MySQL将 select 查询分为简单查询和复杂查询。复杂查询分为三类：简单子查询、派生表（from语句中的子查询）、union 查询
* select_type列： select_type 表示对应行是是简单还是复杂的查询，如果是复杂的查询，又是上述三种复杂查询中的哪一种。
> + simple：简单查询。查询不包含子查询和union
> + primary：复杂查询中最外层的 select
> + subquery：包含在 select 中的子查询（不在 from 子句中）
> + derived：包含在 from 子句中的子查询。MySQL会将结果存放在一个临时表中，也称为派生表（derived的英文含义）  
explain select (select 1 from actor where id = 1) from (select * from film where id = 1) der;
> + 上述sql中第一个select为primary，后面为子查询 subquery,from之后的子查询为derived  

> + union：在 union 中的第二个和随后的 select 
> + union result：从 union 临时表检索结果的 select  
explain select 1 union all select 1;  
* table列:  这一列表示 explain 的一行正在访问哪个表。
> + 当 from 子句中有子查询时，table列是 <derivenN> 格式，表示当前查询依赖 id=N 的查询，于是先执行 id=N 的查询。当有 union 时，UNION RESULT 的 table 列的值为 <union1,2>，1和2表示参与 union 的 select 行id。

* type列: 
> + 一列表示关联类型或访问类型，即MySQL决定如何查找表中的行。
> + 依次从最优到最差分别为：system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL
> + NULL：mysql能够在优化阶段分解查询语句，在执行阶段用不着再访问表或索引。例如：在索引列中选取最小值，可以单独查找索引来完成，不需要在执行时访问表
> + const, system：mysql能对查询的某部分进行优化并将其转化成一个常量（可以看show warnings 的结果）。用于 primary key 或 unique key 的所有列与常数比较时，所以表最多有一个匹配行，读取1次，速度比较快。
> + eq_ref：primary key 或 unique key 索引的所有部分被连接使用 ，最多只会返回一条符合条件的记录。这可能是在 const 之外最好的联接类型了，简单的 select 查询不会出现这种 type。
> + ref：相比 eq_ref，不使用唯一索引，而是使用普通索引或者唯一性索引的部分前缀，索引要和某个值相比较，可能会找到多个符合条件的行。
> + ref_or_null：类似ref，但是可以搜索值为NULL的行。
> + index_merge：表示使用了索引合并的优化方法。 例如下表：id是主键，tenant_id是普通索引。or 的时候没有用 primary key，而是使用了 primary key(id) 和 tenant_id 索引
> + range：范围扫描通常出现在 in(), between ,> ,<, >= 等操作中。使用一个索引来检索给定范围的行。
> + index：和ALL一样，不同就是mysql只需扫描索引树，这通常比ALL快一些。
> + ALL：即全表扫描，意味着mysql需要从头到尾去查找所需要的行。通常情况下这需要增加索引来进行优化了

* possible_keys列
> + 这一列显示查询可能使用哪些索引来查找。 
> + explain 时可能出现 possible_keys 有列，而 key 显示 NULL 的情况，这种情况是因为表中数据不多，mysql认为索引对此查询帮助不大，选择了全表查询。 
> + 如果该列是NULL，则没有相关的索引。在这种情况下，可以通过检查 where 子句看是否可以创造一个适当的索引来提高查询性能，然后用 explain 查看效果。

* key列
> + 这一列显示mysql实际采用哪个索引来优化对该表的访问。
> + 如果没有使用索引，则该列是 NULL。如果想强制mysql使用或忽视possible_keys列中的索引，在查询中使用 force index、ignore index。

*key_len列
> + 这一列显示了mysql在索引里使用的字节数，通过这个值可以算出具体使用了索引中的哪些列。 
> + 举例来说，film_actor的联合索引 idx_film_actor_id 由 film_id 和 actor_id 两个int列组成，并且每个int是4字节。通过结果中的key_len=4可推断出查询使用了第一个列：film_id列来执行索引查找。




