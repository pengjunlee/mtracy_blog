> 原文链接：<https://www.cnblogs.com/efforts-will-be-lucky/p/7324789.html>

索引通常能够极大的提高查询的效率。在系统中使用查询时，应该考虑建立相关的索引。在MongoDB中创建索引相对比较容易。

mongodb中的索引在概念上和大多数关系型数据库如MySQL是一样的。当你在某种情况下需要在MySQL中建立索引,这样的情景同样适合于MongoDB。

# 基本操作

索引是一种数据结构，他搜集一个集合中文档特定字段的值。MongoDB的查询优化器能够使用这种数据结构来快速的对集合(collection)中的文档(collection)进行寻找和排序.准确来说，这些索引是通过B-Tree索引来实现的。

在命令行中，可以通过调用ensureIndex()函数来建立索引，该函数指定一个到多个需要索引的字段。沿用在前面的随笔中的例子，我们再things集合中的j字段上建立索引：

	> db.things.ensureIndex({j:1}) 

EnsureIndex()函数自是在索引不存在的情况下才会创建。

一旦集合在某一个字段上建立索引后，对该字段的随机查询的访问速度会很快。如果没有索引,MongoDB会在遍历所有的键值对，然后去对应检查相关的字段。

	> db.things.find({j:2}); //在建立了索引的字段上查询，速度快  
	{ "_id" : ObjectId("4e24433dcac1e3490b9033be"), "x" : 4, "j" : 2 }  
	> db.things.find({x:3});//在未建立索引的字段上查询，需要逐个字段匹配，速度慢  
	{ "_id" : ObjectId("4e244315cac1e3490b9033bc"), "x" : 3 } 

通过在命令行中输入getIndexs()能够查看当前集合中的所有索引。

	> db.things.getIndexes()  
	[  
		{
			"name": "_id_",
			"ns": "things.things",
			"key": {
				"_id": 1
			},
			"v": 0
		},  
		{
		
			"_id": ObjectId("4e244382cac1e3490b9033d0"),
			"ns": "things.things",
			"key": {
				"j": 1
			},
			"name": "j_1",
			"v": 0
		}
	]

通过`db.system.indexes.find()`能够返回当前数据库中的所有索引

	> db.system.indexes.find()  
	{ "name" : "_id_", "ns" : "things.things", "key" : { "_id" : 1 }, "v" : 0 }
	{ "_id" : ObjectId("4e244382cac1e3490b9033d0"), "ns" : "things.things", "key" :{ "j" : 1 }, "name" : "j_1", "v" : 0 }

默认索引

对于每一个集合（除了capped集合），默认会在_id字段上创建索引，而且这个特别的索引不能删除。_id字段是强制唯一的，由数据库维护。

嵌套关键字

在MongoDB中，甚至能够在一个嵌入的文档上(embedded)建立索引.

> db.things.ensureIndex({"address.city":1}) 
文档作为索引

任何类型，包括文档(document)都能作为索引:

> db.factories.insert({name:"xyz",metro:{city:"New York",state:"NY"}});  
 
> db.factories.ensureIndex({metro:1});  
 
> db.factories.find({metro:{city:"New York",state:"NY"}});//能够利用索引进行查询  
 
{ "_id" : ObjectId("4e244744cac1e3490b9033d2"), "name" : "xyz", "metro" : < 
 
{ "city" : "New York", "state" : "NY" } }  
 
> db.factories.find({metro:{$gte:{city:"New York"}}});//能够利用索引进行查询  
 
{ "_id" : ObjectId("4e244744cac1e3490b9033d2"), "name" : "xyz", "metro" : { "city" : "New York", "state" : "NY" } }  
 
> db.factories.find({metro:{state:"NY",city:"New York"}})//不能够返回结果，字段的顺序不对 
创建文档索引的一个替代方法是创建复合索引,例如：

> db.factories.ensureIndex({"metro.city":1,"metro.state":1})  
 
> db.factories.find({"metro.city":"New York","metro.state":"NY"});  
 
{ "_id" : ObjectId("4e244744cac1e3490b9033d2"), "name" : "xyz", "metro" : { "city" : "New York", "state" : "NY" } }  
 
> db.factories.find({"metro.city":"New York"});  
 
{ "_id" : ObjectId("4e244744cac1e3490b9033d2"), "name" : "xyz", "metro" : { "city" : "New York", "state" : "NY" } }  
 
> db.factories.find().sort({"metro.city":1,"New York":1});  
 
{ "_id" : ObjectId("4e244744cac1e3490b9033d2"), "name" : "xyz", "metro" : { "city" : "New York", "state" : "NY" } }  
 
> db.factories.find().sort({"metro.city":1});  
 
{ "_id" : ObjectId("4e244744cac1e3490b9033d2"), "name" : "xyz", "metro" : { "city" : "New York", "state" : "NY" } } 
组合关键字索引

除了基本的以单个关键字作为索引外，MongoDB也支持多个关键字的组合索引，和基本的索引一样，也是用ensureIndex()函数，该函数可以指定多个键。

> db.things.ensureIndex({j:1,name:-1}) 
当创建索引时，键后面的数字表明了索引的方向，取值为1或者-1,1表示升序，-1表示降序。升序或者降序在随机访问的时候关系不大，当时在做排序或者范围查询的时候就很重要了。

如果在建立了a,b,c这样一个复合索引，那么你可以在a，A,b和a,b,c上使用索引查询。

 

 

 

稀疏索引

和稀疏矩阵类似，稀疏索引就是索引至包含被索引字段的文档。

任何一个稀疏的缺失某一个字段的文档将不会存储在索引中，之所以称之为稀疏索引就是说缺失字段的文档的值会丢失。

稀疏索引的创建和完全索引的创建没有什么不同。使用稀疏索引进行查询的时候，某些由于缺失了字段的文档记录可能不会被返回，这是由于稀疏索引子返回被索引了的字段。可能比较难以理解，不过看几个例子就好理解了。

> db.people.ensureIndex({title:1},{sparse:true}) //在title字段上建立稀疏索引  
 
> db.people.save({name:"Jim"})  
 
> db.people.save({name:"yang",title:"prince"})  
 
> db.people.find();  
 
{ "_id" : ObjectId("4e244dc5cac1e3490b9033d7"), "name" : "Jim" }  
 
{ "_id" : ObjectId("4e244debcac1e3490b9033d8"), "name" : "yang", "title" : "prince" }  
 
> db.people.find().sort({title:1})//自有包含有索引字段的记录才被返回  
 
{ "_id" : ObjectId("4e244debcac1e3490b9033d8"), "name" : "yang", "title" : "prince" }  
 
> db.people.dropIndex({title:1})//删除稀疏索引之后，所有的记录均显示  
 
{ "nIndexesWas" : 2, "ok" : 1 }  
 
> db.people.find().sort({title:1})  
 
{ "_id" : ObjectId("4e244dc5cac1e3490b9033d7"), "name" : "Jim" }  
 
{ "_id" : ObjectId("4e244debcac1e3490b9033d8"), "name" : "yang", "title" : "prince" } 
唯一索引

MongoDB支持唯一索引，这使得不能插入在唯一索引项上已经存在的记录。例如，要保证firstname和lastname都是唯一的，命令如下

 

> db.things.ensureIndex({firstname:1,lastname:1},{unique:true}) 
缺失的键

当一个文档以唯一索引的方式保存到集合中去的时候，任何缺失的索引字段都会一null值代替，因此，不能在唯一索引上同时插入两条缺省的记录。如下：

>db.things.ensureIndex({firstname: 1}, {unique: true});  
 
>db.things.save({lastname: "Smith"});  
 
>db.things.save({lastname: "Jones"});// 会产生错误，因为firstname会有两个null. 
重复值:

唯一索引不能够创建在具有重复值的键上，如果你一定要在这样的键上创建，那么想系统将保存第一条记录，剩下的记录会被删除，只需要在创建索引的时候加上dropDups这个可选项即可

>db.things.ensureIndex({firstname : 1}, {unique : true, dropDups : true})  
 
Dropping Indexes 
删除一个特定集合上的索引：

>db.collection.dropIndexes(); 
删除集合中的某一个索引：

db.collection.dropIndex({x: 1, y: -1}) 
也可以直接执行命令进性删除

db.runCommand({dropIndexes:'foo', index : {y:1}})//删除集合foo中{y:1}的索引  
 
// remove all indexes:  
 
db.runCommand({dropIndexes:'foo', index : '*'})//删除集合foo中所有的索引 
重建索引：

可以所用如下命令重建索引：

db.myCollection.reIndex()  
 
// same as:  
 
db.runCommand( { reIndex : 'myCollection' } ) 
通常这是不必要的，但是在集合的大小变动很大及集合在磁盘空间上占用很多空间时重建索引才有用。对于大数据量的集合来说，重建索引可能会很慢。

注：

MongoDB中索引是大小写敏感的。

当更新对象是，只有在索引上的这些key发生变化时才会更新。着极大地提高了性能。当对象增长了或者必须移动时，所有的索引必须更新，这回很慢 。

索引信息会保存在system.indexes 集合中,运行 db.system.indexes.find() 能够看到这些示例数据。

索引的字段的大小有最大限制,目前接近800 bytes. 可在大于这个值的字段上建立索引是可以的，但是该字段不会被索引，这种限制在以后的版本中可能被移除。

索引的性能

索引使得可以通过关键字段获取数据，能够使得快速查询和更新数据。

但是，必须注意的是，索引也会在插入和删除的时候增加一些系统的负担。往集合中插入数据的时候，索引的字段必须加入到B-Tree中去，因此，索引适合建立在读远多于写的数据集上，对于写入频繁的集合，在某些情况下，索引反而有副作用。不过大多数集合都是读频繁的集合，所以集合在大多数情况下是有用的。

使用sort()而不需要索引

如果数据集合比较小（通常小于4M），使用sort（）而不需要建立索引就能够返回数据。在这种情况下，做好联合使用limit()和sort()。