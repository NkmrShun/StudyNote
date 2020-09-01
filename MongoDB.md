MongoDB
##什么是MongoDB## 
MongoDB是一个文档数据库，提供好的性能，领先的非关系型数据库。采用BSON存储文档数据。
BSON（）是一种类json的一种二进制形式的存储格式，简称Binary JSON.
相对于json多了date类型和二进制数组。

##有什么优势##

- 面向文档的存储：以 JSON 格式的文档保存数据。
- 任何属性都可以建立索引。
- 复制以及高可扩展性。
- 自动分片。
- 丰富的查询功能。
- 快速的即时更新。

##对比##

|  MongoDB   | 关系型数据库  |
|  ----  | ----  |
| Database  | Database |
| Colletcition/Table  | Table |
| Document  | Record/Rod |
| Field | Column |
| Embedded Documents  | Table Join |

1. 创建数据库
```
//命令
use DATABASE_NAME
//创建users数据库
use users
//查看所有数据库
//要数据库有数据才能查询到
show dbs
//删除数据库
//删除当前数据库
db.dropDatabase()
```
2. 创建集合
```
//命令
db.createCollection(name, options)
//查看集合
show collections
//在 MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。
>db.mycol2.insert({"name" : "菜鸟教程"})
>show collections
mycol2
//删除集合
db.mycol2.drop()
```

3. 插入文档
```
db.COLLECTION_NAME.insert(document)
db.COLLECTION_NAME.insertOne(document)
db.COLLECTION_NAME.replaceOne(document)
db.COLLECTION_NAME.insertMany()
```
```
db.col.insert({title: 'MongoDB 教程'})
db.col.insertMany([{title: 'MongoDB 教程'},{title: '数据库教程'}])
```
4. 更新文档
```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
```
5. 删除文档
```
>db.col.remove({'title':'MongoDB 教程'})
```
6. 查询文档
```
db.collection.find(
	{
		key1:value1,
		key2:{$gt : 100},
		$or: [
			{"by": "菜鸟教程"},
			{"title": "MongoDB 教程"}
		]
	}, projection)
//一条数据
db.collection.findOne(query, projection)
//易读式
db.collection.find(query, projection).pretty()
//通过类型获取数据
db.col.find({"title" : {$type : 2}})
db.col.find({"title" : {$type : 'string'}})
//限制获取数量
db.col.find({},{"title":1,_id:0}).limit(1).skip(1)
//排序，1升序，-1降序
db.col.find({},{"title":1,_id:0}).sort({"likes":-1})
```
	- (>) 大于 - $gt
	- (<) 小于 - $lt
	- (>=) 大于等于 - $gte
	- (<= ) 小于等于 - $lte

7. 索引
```
db.collection.createIndex(keys, options)
//查看集合索引
db.col.getIndexes()
//查看集合索引大小
db.col.totalIndexSize()
//删除集合所有索引
db.col.dropIndexes()
//删除集合指定索引
db.col.dropIndex("索引名称")
```

7. 聚合
```
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)
db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
等同于
select by_user, count(*) from mycol group by by_user
```




