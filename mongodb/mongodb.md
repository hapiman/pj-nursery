### Basics
>  MongoDB的默认数据库是test。 如果没有创建任何数据库，那么集合将被保存在测试数据库

> update()方法将现有的文档中的值更新，而save()方法使用传递到save()方法的文档替换现有的文档。

### Operations
* 创建数据库 `use newdb`
* 删除数据库 `db.dropDatabase()`
* 创建集合 `db.createCollection(name, options)` `name`是创建集合的名称，`options`(可选)指定有关内存大小和索引选项
* 删除集合 `db.COLLECTION_NAME.drop()`
* 插入文档 `db.COLLECTION_NAME.insert(document)`
* 查询并格式化结果 `db.mycol.find().pretty()` 格式化关键字`pretty`
* where关键字 `$ne`,`$lt`,`$gt`,`$lte`,`$gte`
* or使用 `{$or:[key1: value1}, {key2:value2}]}` 通过`[]`设置满足的选项
* 单条数据更新：`db.cname.update({key:value},{$set:{})`
* 多条数据跟新：`db.cname.update({key:value},{$set:{},{multi:true})`
* 只获取所需数据,需要使用`find`的第二个参数 `db.cname.find({},{key1:1,key2:0})` 1表示显示，0代表不显示
* 查询在某一数据库某一段的数据 `limit`查询的条数,`skip`跳过的条数  `db.cname.find().limit(NUMBER).skip(NUMBER)`
* 使用sort排序 `db.COLLECTION_NAME.find().sort({KEY:1})` 1表示升序,-1表示降序
* 创建索引 单字段索引`db.mycol.ensureIndex({"title":1})`,多字段索引 `db.mycol.ensureIndex({"title":1,"description":-1})`
* MongoDB中的聚合aggregate 直接上代码

求总和：`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])`  

求平均：`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])`

求最大 `db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}])`

数据中添加数据`$push`，如果数据不能重复需要使用`$addToSet` `	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])`

### update
```javascript
 db.collection.update(
   <query>, //sql中的where条件
   <update>,//sql中update后面的set
   {
     upsert: <boolean>,//在没有找到的情况下是否需要增加数据 默认为false.
     multi: <boolean>, //是否对每一条满足条件的数据进行更改，默认为false
     writeConcern: <document>
   }
)
```
相关实例
```javascript
//只更新第一条记录：
db.col.update( { "count" : { $gt : 1 } } , { $set : { "test2" : "OK"} } );
//全部更新：
db.col.update( { "count" : { $gt : 3 } } , { $set : { "test2" : "OK"} },false,true );
//只添加第一条：
db.col.update( { "count" : { $gt : 4 } } , { $set : { "test5" : "OK"} },true,false );
//全部添加加进去:
db.col.update( { "count" : { $gt : 5 } } , { $set : { "test5" : "OK"} },true,true );
//全部更新：
db.col.update( { "count" : { $gt : 15 } } , { $inc : { "count" : 1} },false,true );
//只更新第一条记录：
db.col.update( { "count" : { $gt : 10 } } , { $inc : { "count" : 1} },false,false );
```

相关关键字使用

**条件操作符**  
类似`where`关键字`$ne,$lt,$gt,$lte,$gte` 使用`{data:{$ne:1}}` 使用{'数据':{'判断条件'}}

**聚合数据**
{'数据':{'判断条件'}} 如：{num_tutorial:{$max/$sum/$avg: ''}}
![相关数据](http://o6w21c1fl.bkt.clouddn.com/QQ20160509-4.png)
![相关用法](http://o6w21c1fl.bkt.clouddn.com/QQ20160509-3.png)

**管道概念**
类似unix中一个命令的输出作为下一个命令的参数
聚合框架中常见的操作：
`$project`：修改输入文档的结构，用来重命名，增加或者删除，或者用于创建计算结果以及嵌套文档
`$match`:过滤数据，只输出符合条件的文档
`$limit` `$skip`
`$unwind`：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
`$sort`
`$geoNear`：输出接近某一地理位置的有序文档。

**查看索引**
查看数据库中所有的索引`db.system.indexes.find()`
查看某个集合的索引`db.集合名.getIndexes()`
删除某个索引`db.集合.dropIndex(index)`
index参数可以是索引的名字，也可以是创建索引时的keys文档参数，如：
`db.std.dropIndex(‘name_age’)`//索引名
`db.std.dropIndex({name:1,age:-1})`//keys参数
`db.集合.dropIndexes()`//删除集合中所有的索引
