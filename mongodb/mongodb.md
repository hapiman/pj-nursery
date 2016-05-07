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
* MongoDB中的聚合aggregate 直接上代码 `$sum`求总和：`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])`  求平均：`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])` 求最大 `db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}])` 数据中添加数据`$push`，如果数据不能重复需要使用`$addToSet` `	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])` 
