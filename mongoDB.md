# MongoDB

## 数据类型
- null
- boolean
- 32、64位整数（会被自动转换其他方式存储）
- 64位浮点数（唯一被支持的类型）
- 字符串
- 符号（被转换成字符串）
- ObjectId（文档中必须有一个\_id键，默认是ObjectId对象，也可以是其他类型，不同的机器都能用全局唯一的方式生成它）
- 日期（存储从标准纪元开始的毫秒数，不存储时区）
- 正则表达式（采用javascript的正则表达式语法）
- 代码（javascript代码）
- 二进制数据（shell中无法使用）
- 最大值、最小值（BSON中的特殊类型，shell中无法使用）
- undefined
- 数组
- 内嵌文档

## CRUD
### 增加
```
db.foo.insert({"bar":"baz"})
```
数据条数较多，要采用批量插入数据，减少TCP连接次数，另外还要注意插入数据的大小
### 删除
```
db.user.remove()
db.mailing.list.remove({"optout":true})
```
删除数据是永久性的，不能撤销，也不能恢复
### 更新
```
> joe = db.people.find();
{
	"_id" : 1,
    "name" : "joe",
}
> db.people.update({"_id":1},{name:"hello"});
> db.people.find();
{
	"_id" : 1,
    "name" : "hello",
}
/*第3个参数是upsert选项，存在时更新，不存在时插入，也可以使用save函数*/
> db.people.update({"_id":2},{name:"world"},true);
> db.people.find();
{
	"_id" : 1,
    "name" : "hello",
}
{
	"_id" : 2,
    "name" : "world",
}
/*第4个参数true是更新多个文档，建议总是指定这些参数*/
> db.people.update({"_id":2},{name:"world"},true,true);
```
使用更新器
- $set
```
> db.zzz.find({a:1});
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1
}
> db.zzz.update({a:1},{$set:{b:"b"}});
> db.zzz.find({a:1});
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : "b"
}
> db.zzz.update({a:1},{$set:{b:["c","d"]}});
> db.zzz.find({a:1});
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["c", "d"]
}
```
- $inc
```
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : 1
}
> db.zzz.update({a:1},{$inc:{b:2}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
	"b" : 3
}
```
- $push（适用于数组）
```
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : []
}
> db.zzz.update({a:1},{$push:{b:1}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : [1]
}
```
- addToSet、$each
```
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["a", "b", "c"]
}
> db.zzz.update({a:1},{$addToSet:{b:"c"}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["a", "b", "c"]
}
> db.zzz.update({a:1},{$addToSet:{b:"d"}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["a", "b", "c", "d"]
}
> db.zzz.update({a:1},{$addToSet:{b:{$each:["e","f"]}}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["a", "b", "c", "d", "e", "f"]
}

> db.zzz.update({a:1},{$addToSet:{c:1}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["a", "b", "c", "d", "e", "f"],
    "c" : 1
}
```
- $pop（适用于数组）
{key:1}从数组末尾删除，{key:-1}从数组头部删除
```
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["a", "b", "c", "d", "e", "f"],
    "c" : 1
}
> db.zzz.update({a:1},{$pop:{b:{key:-1}}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : ["b", "c", "d", "e", "f"],
    "c" : 1
}
```
- $pull（适用于数组）
```
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : [1, 3, 1, 2]
}
> db.zzz.update({a:1},{$pull:{b:1}});
> db.zzz.find();
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : [3, 2]
}
```

### 查询
```
> db.zzz.find({a:1});
{
	"_id" : ObjectId(55c570812d3c0d6967714def),
	"a" : 1,
    "b" : [1, 3, 4]
}
```
#### 查询条件
- $lt、$lte、$gt、$gte、$ne
```
> db.zzz.find(age:{$gte:18,$lte:30});
> start = new Date("01/01/2007");
> db.zzz.find({reg_time:{$lt:start}});
> db.zzz.find({username:{$ne:"joe"}});
```
- $in、$nin（单键匹配）
```
db.zzz.find({name:{$in:["joe", "tom"]}});
db.zzz.find({name:{$nin:["joe", "tom"]}});
```
- $or（多键匹配）
```
db.zzz.find({$or:[{name:{$in:["joe", "tom"]}},{age:20}]});
```
- $not
```
db.zzz.find({age:{$not:{$lte:20}}});
```
- 正则表达式匹配
```
> db.users.find({name:/joe/i});
```
- $all（适用于数组）
```
> db.food.find({fruit:{$all:["apple", "banana"]}});
```
- $size（适用于数组）
```
> db.food.find({fruit:{$size:3}});
```
- $slice（适用于数组）
```
/*返回最后10条评论*/
> db.posts.find({artical_id:123},{"comments":{$slice:-10}});
/*返回第30~40条评论*/
> db.posts.find({artical_id:123},{"comments":{$slice:[30, 10]}});
```
- $where  
使用$where时，每个文档都要从BSON转换成javascript对象，然后通过$where中的表达式来运行，同时也不能利用索引，所以速度上比常规查询慢很多，不到万不得已不能使用，不过可以先用常规查询作为前置过滤，然后用$where对结果进行调优
```
db.foo.find({$where:"this.x + this.y == 10"});
```
#### 游标
数据库使用游标来返回find的结果
```
> var cursor = db.collection.find();
> while(cursor.hasNext()){
...obj = cursor.next();
...//do stuff
...}
```
```
> var cursor = db.collection.find();
> cursor.forEach(function(x){
...print(x.name);
...});
```
#### limit、skip、sort
```
> db.c.find().limit(3);
/*避免使用skip跳过大量文档*/
> db.c.find().skip(3);
> db.c.find().sort({age:-1});
```

## 索引
```
> db.c.ensureIndex({date:1,age:-1});
> db.c.ensureIndex({date:1},{name:"indexName"});
> db.c.ensureIndex({order_id:1},{unique:true});
/*地理空间索引*/
> db.c.ensureIndex({"gps":"2d"});
> db.c.find({"gps":{"$near":[40, 90]}}).limit(10);
> db.c.find({"gps":{"$within":{"$box":[[10,20],[15,30]]}}});
> db.c.find({"gps":{"$within":{"$center":[[10,20],5]}}});
> db.c.ensureIndex({"light-years":"year"},{"min":-1000,"max":1000});
```

## 聚合
- count
```
db.foo.count();
db.foo.find({a:1}).count();
```
- distinct
```
db.runCommand({"distinct":people,"key":"age"});
```
- group
```
db.runCommand({"group":{
..."ns":"posts",
..."key":{"tags":true},
..."initial":{"tags":{}},
..."$reduce":function(doc, prev){
...//do something
...},
..."finalize":function(prev){
...//do someting
...}
}});
```
- mapreduce
```
> db.runCommand(
...{
...mapreduce : 字符串，集合名,
...map : 函数
...reduce : 函数
...[, query : 文档，发往map函数前先给过渡文档]
...[, sort : 文档，发往map函数前先给文档排序]
...[, limit : 整数，发往map函数的文档数量上限]
...[, out : 字符串，统计结果保存的集合]
...[, keeptemp: 布尔值，链接关闭时临时结果集合是否保存]
...[, finalize : 函数，将reduce的结果送给这个函数，做最后的处理]
...[, scope : 文档,js代码中要用到的变量]
...[, jsMode : 布尔值，是否减少执行过程中BSON和JS的转换，默认true]
//false时 BSON-->JS-->map-->BSON-->JS-->reduce-->BSON,可处理非常大的          mapreduce,true时BSON-->js-->map-->reduce-->BSON
...[, verbose : 布尔值，是否产生更加详细的服务器日志，默认true]
...}
...);
```