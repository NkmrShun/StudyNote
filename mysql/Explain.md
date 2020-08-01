# Mysql 命令 #
## Explain ##
explain命令可以检查SQL语句执行效率、索引使用情况等。
explain命令输出id,select_type,table,type,possible_keys,key,key_len,ref,rows,extra。

1. id
	包含一组数字，表示查询中执行SELECT子句或操作表的顺序。
    - 如果id相同执行顺序由上至下
    - 如果id不相同，id的序号会递增，id值越大优先级越高，越先被执行
	    - 一般有子查询SQL语句id就会不同
2. select_type
	select查询的类型
    - **SIMPLLE**:简单查询，该查询不包含UNION或子查询
	- **PRIMARY**:如果查询包括UNION或子查询，则最外层的查询被标识为PRIMARY
	- UNION:表示此查询是UNION中的第二个或者随后的查询
	- DEPENDENT:UNION满足UNION中的第二个或随后的查询，其次取决于外面的查询
	- UNION RESULT:UNION的结果
	- **SUBQUERY**:子查询中的第一个select语句（该子查询不在from字句中）
	- DEPENDENT SUBQUERY:子查询中的第一个select，同时取决于外面的查询
	- **DERIVED**:包含在from字句中子查询（也成为派生表）
	- UNCACHEABLE SUBQUERY:满足是子查询中的第一个select语句，同时意味着select中的某些特性阻止结果被缓存于一个ITEM_CACHE中
	- UNCACHEABLE UNION:满足次查询是UNION中的第二个或者随后的查询，同时意味着select中的某些特性阻止结果被缓存于一个ITEM_CACHE中

3. table
	该列显示了对应行正在访问哪个表(有别名就显示别名)。
4.  type
	该列称为关联类型或者访问类型，它指明了MySQL决定如何查找表中符合条件的行，同时是我们判断查询是否高效的重要依据。
	- all：全表扫描，这个类型是性能最差的查询之一。
		all类型，是因为这样的查询，在数据量最大的情况下，对数据库的性能是巨大的灾难
	- index：全索引扫描，和all类型类似，只不过all类型是全表扫描，而index类型是扫描全部的索引，主要优点是避免了排序，但是开销仍然非常大。如果在extra列看到using index，说明正在使用覆盖索引，只扫描索引的数据，它比按索引次序全表扫描的开销要少很多。
	- range：范围扫描，就是一个有限制的索引扫描，它开始于索引黎的某一点，返回匹配这个值域的行。这个类型通常出现在`=、&lt;&gt;、&gt;、&gt;=、<、<=、IS NULL、<=>、BETWEEN、IN()`的操作中，key列显示使用了哪个索引，当type为该值时，则输出ref列为null，并且key_len列是此次查询中使用到的索引最长的那个。
	- ref：一种索引访问，也称索引查找，它返回所有匹配某个单个值得行。此类型通常出现在多表的join查询，针对于非唯一或非主键索引，或者是使用了最左前缀规则索引的查询。
	- eq_ref使用这种索引查找，最多只返回一条符合条件的记录。在使用唯一性索引或主键查找时会出现该值，非常高效。
	- const、system：表示该表至多有一个匹配行，在查询开始时读取，或者该表是系统表，只有一行匹配。其中const用于在和primary key或 unique索引中有固定值比较的情形。
	- null：在执行阶段不需要访问表
5. possible_keys
	这一列显示查询可能使用那些索引来查找
6. key
	这一列显示mysql实际决定使用的索引。如果没有选择索引，键是null。
7. key_len
	这一列显示了再索引里使用的字节数，当key列的值为null时，则该列也是null
8. ref
	这一列显示了那些字段或者常量被用来和key配合从表中查询记录出来
9. rows
	这一列显示了估计要找到所需要的行而需要读取的行数，这个值是估计值，原则上越小越好。
10. extra
	其他信息
	- usingindex：使用覆盖索引，表示查询索引就可查到所需数据，不用扫描表数据文件，往往说明性能不错。
	- usingwhere：在存储引擎检索行后再进行过滤，使用了where从句来限制哪些行将与下一张表匹配或者是返回给用户。
	- using temporary：再查询结果排序时会使用一个临时表，一般出现于排序、分组和多表join的情况，查询效率不高，建议优化。
	- usingfilesort：对结果使用一个外部索引排序，而不是按索引次序从表里读取行，一般有出现该值，都建议优化去掉，因为这样的查询对cpu资源消耗大。
