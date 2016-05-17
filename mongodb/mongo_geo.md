##纪录MongoDB中地理位置查询

### 1.说明
mongodb支持二维空间索引，也就是"搜索离某个坐标最近的N条记录"，同时能够像使用`find`一样对数据进行过滤。首先需要确定索引的存在，索引的字段的格式可以如下：
```javascript
{ loc : [ 50 , 30 ] }  
{ loc : { x : 50 , y : 30 } }  
{ loc : { lat : 40.739037, long: 73.992964 } }  
```
### 2.索引
创建索引
```javascript
  //我的books文档结构
  books:{
    _id:'',
    position:[123,33],
    category:'product',
    ...
  }
```
```javascript
  db.books.ensureIndex({position:'2dsphere'});//官方推荐
  db.books.find({position:[100,100]});//精确查询
  db.books.find({position:{$near:[50,50]}});//查询离[50,50]最近的100个点（默认数量），可以使用limit函数设置其他的值
```
组合索引
```javascript
db.books.ensureIndex( { position : "2d" , category : 1 } );  
db.books.find( { position : { $near : [50,50] }, category : 'coffee' } );  
```

### 3.附近查询
3.1使用**near**和**$nearSphere**

使用`$near`和`$nearSphere`查询，两者使用方法相同，都存在两种使用方法，但是两种方法设置`maxDistance`和`minDistance`的方法不同，其中`radians`表示弧度，`meters`表示米。

```javascript
//nearSphere-radians
{
  $nearSphere: [ <x>, <y> ],
  $minDistance: <distance in radians>,
  $maxDistance: <distance in radians>
}

//near-radians
{
  $near: [ <x>, <y> ],
  $maxDistance: <distance in radians>
}

//nearSphere-meters
{
  $nearSphere: {
     $geometry: {
        type : "Point",
        coordinates : [ <longitude>, <latitude> ]
     },
     $minDistance: <distance in meters>,
     $maxDistance: <distance in meters>
  }
}

//near-meters
{
  $near: {
     $geometry: {
        type: "Point" ,
        coordinates: [ <longitude> , <latitude> ]
     },
     $maxDistance: <distance in meters>,
     $minDistance: <distance in meters>
  }
}

```
更多信息查看[https://docs.mongodb.com/manual/reference/operator/query/near/](https://docs.mongodb.com/manual/reference/operator/query/near/)

3.2使用**$geoNear**查询

```javascript
db.runCommand( { geoNear: "places", near: [ 121.4905, 31.2646 ],spherical: true,$maxDistance: 0.5/6371 })
```

使用$geoNear返回的数据中带有**dis**字段能够获取距离当前坐标的距离,如果指定了spherical为true， dis的值为弧度，不指定则为度。

>如果不用弧度，以水平单位（度）查询时，距离单位是以公里数除以111（推荐值），原因如下：经纬度的一度，分为经度一度和纬度一度。地球不同纬度之间的距离是一样的，地球子午线（南极到北极的连线）长度39940.67公里，纬度一度大约110.9公里，但是不同纬度的经度一度对应的长度是不一样的。在地球赤道，一圈大约为40075KM,除以360度，每一个经度大概是：40075/360=111.32KM。上海，大概在北纬31度，对应一个经度的长度是：40075*sin(90-31)/360=95.41KM。北京在北纬40度，对应的是85KM。前面提到的参数111，这个值只是估算，并不完全准确，任意两点之间的距离，平均纬度越大，这个参数则误差越大。

$geoNear返回结果集中，指定 spherical为true,结果中的dis需要乘以6371就能换算为km,不指定sphericial，结果中的dis乘以就能111换算为km:

### 区域查询

使用$within代替$near查找某个形状之内的结果，支持$box（矩形）和$center（圆形），如果是矩形，需要指定矩形的左下角和右上角的坐标
```javascript
const box = [[40, 40], [60, 60]]  
db.places.find({"loc" : {"$within" : {"$box" : box}}})  
```
