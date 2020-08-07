官网文档地址：<https://docs.mongodb.com/manual/crud/>

# 创建文档
使用插入操作向一个集合中添加文档时，如果目标集合当前并不存在，执行操作会自动创建该集合。

MongoDB 提供了以下三个方法来向集合中插入文档：

	db.collection.insert()
	db.collection.insertOne() // MongoDB 3.2 以上版本
	db.collection.insertMany() // MongoDB 3.2 以上版本

在 MongoDB 中，插入操作只能指定一个集合作为目标，并且所有的写操作在单个文档的级别上都是原子性的。

例如下面示例了向 customer 集合中插入一个新文档，如果未指定新文档的 _id 字段，MongoDB 会使用一个 ObjectId 值作为 _id 字段的值，并将其添加到新文档中。

	> db.customer.insertOne({name:"lisa",age:27,features:{height:178,weight:63,gender:"female"},hobby:["yoga","cartoon","movie"]})
	{
		"acknowledged" : true,
		"insertedId" : ObjectId("5bcfeb7542f5fcf5f319f0d9")
	}

使用 `db.collection.insertMany()` 方法可以一次性地向集合插入多个文档，传入一个文档数组作为方法的参数即可。

	> db.customer.insertMany([{name:"lisa",age:27,features:{height:178,weight:63},hobby:["yoga","cartoon","movie"]},{name:"tracy",age:23,features:{height:168,weight:55},hobby:["yoga","music","movie"]}])
	{
		"acknowledged" : true,
		"insertedIds" : [
			ObjectId("5bd00f96c90c1f1d66f50312"),
			ObjectId("5bd00f96c90c1f1d66f50313")
		]
	}

# 查询文档
MongoDB 提供了下面两个方法用来查询指定集合内部的文档：

	db.collection.find()
	db.collection.findOne()

## 查询所有文档
如果你想要查询获取指定集合内的所有文档，你需要传递一个空文档对象作为find() 方法的查询过滤器参数,这个查询过滤器决定了查询获取到的文档应该匹配的条件。

	> db.customer.find({}).pretty()
	{
		"_id" : ObjectId("5bd00f96c90c1f1d66f50312"),
		"name" : "lisa",
		"age" : 27,
		"features" : {
			"height" : 178,
			"weight" : 63
		},
		"hobby" : [
			"yoga",
			"cartoon",
			"movie"
		]
	}
	{
		"_id" : ObjectId("5bd00f96c90c1f1d66f50313"),
		"name" : "tracy",
		"age" : 23,
		"features" : {
			"height" : 168,
			"weight" : 55
		},
		"hobby" : [
			"yoga",
			"music",
			"movie"
		]
	}

该操作相当于关系型数据库中的 `SELECT * FROM customer` 查询SQL。

## 指定等式条件
在查询过滤器文档对象中使用 <field>:<value>指定等式条件，来限制只查询字段 <field> 的值等于<value> 的文档。

例如下面这个例子，查询 customer 集合中所有 name 字段值等于 "tracy" 的文档。

	> db.customer.find({name:"tracy"}).pretty()
	{
		"_id" : ObjectId("5bd00f96c90c1f1d66f50313"),
		"name" : "tracy",
		"age" : 23,
		"features" : {
			"height" : 168,
			"weight" : 55
		},
		"hobby" : [
			"yoga",
			"music",
			"movie"
		]
	}

该操作相当于关系型数据库中的 `SELECT * FROM customer WHERE name = "tracy"` 查询SQL。

当查询字段 <field> 的值是一个文档对象时，查询示例如下：

	> db.customer.find({features:{height:168,weight:55}}).pretty()
	{
		"_id" : ObjectId("5bd00f96c90c1f1d66f50313"),
		"name" : "tracy",
		"age" : 23,
		"features" : {
			"height" : 168,
			"weight" : 55
		},
		"hobby" : [
			"yoga",
			"music",
			"movie"
		]
	}

> 注意：在使用等式条件进行文档查询过滤时，如果匹配的字段值是一个内嵌的文档对象，需要查询结果中该字段的文档内容与过滤器中该字段值的文档内容完全匹配，当然也包括文档内各个字段的顺序。

## 使用查询操作符
使用查询操作符指定查询过滤器文档对象查询条件的基本语法结构如下：

	{ <field1>: { <operator1>: <value1> }, ... }

在 MongoDB 中，常用的查询操作符列举见下表：
<table border="1" cellpadding="0" cellspacing="0">
	<tbody>
		<tr><td>Category</td>
			<td>Name</td>
			<td>Description</td>
		</tr><tr><td rowspan="8">Comparison</td>
			<td>$eq</td>
			<td>Matches values that are equal to a specified value.</td>
		</tr><tr><td>$gt</td>
			<td>Matches values that are greater than a specified value.</td>
		</tr><tr><td>$gte</td>
			<td>Matches values that are greater than or equal to a specified value.</td>
		</tr><tr><td>$in</td>
			<td>Matches any of the values specified in an array.</td>
		</tr><tr><td>$lt</td>
			<td>Matches values that are less than a specified value.</td>
		</tr><tr><td>$lte</td>
			<td>Matches values that are less than or equal to a specified value.</td>
		</tr><tr><td>$ne</td>
			<td>Matches all values that are not equal to a specified value.</td>
		</tr><tr><td>$nin</td>
			<td>Matches none of the values specified in an array.</td>
		</tr><tr><td rowspan="4">Logical</td>
			<td>$and</td>
			<td>Joins query clauses with a logical&nbsp;AND&nbsp;returns all documents that match the conditions of both clauses.</td>
		</tr><tr><td>$not</td>
			<td>Inverts the effect of a query expression and returns documents that do&nbsp;not&nbsp;match the query expression.</td>
		</tr><tr><td>$nor</td>
			<td>Joins query clauses with a logical&nbsp;NOR&nbsp;returns all documents that fail to match both clauses.</td>
		</tr><tr><td>$or</td>
			<td>Joins query clauses with a logical&nbsp;OR&nbsp;returns all documents that match the conditions of either clause.</td>
		</tr><tr><td rowspan="2">Element</td>
			<td>$exists</td>
			<td>Matches documents that have the specified field.</td>
		</tr><tr><td>$type</td>
			<td>Selects documents if a field is of the specified type.</td>
		</tr><tr><td rowspan="6">Evaluation</td>
			<td>$expr</td>
			<td>Allows use of aggregation expressions within the query language.</td>
		</tr><tr><td>$jsonSchema</td>
			<td>Validate documents against the given JSON Schema.</td>
		</tr><tr><td>$mod</td>
			<td>Performs a modulo operation on the value of a field and selects documents with a specified result.</td>
		</tr><tr><td>$regex</td>
			<td>Selects documents where values match a specified regular expression.</td>
		</tr><tr><td>$text</td>
			<td>Performs text search.</td>
		</tr><tr><td>$where</td>
			<td>Matches documents that satisfy a JavaScript expression.</td>
		</tr><tr><td rowspan="4">Geospatial</td>
			<td>$geoIntersects</td>
			<td>Selects geometries that intersect with a&nbsp;GeoJSON&nbsp;geometry. The&nbsp;2dsphere&nbsp;index supports$geoIntersects.</td>
		</tr><tr><td>$geoWithin</td>
			<td>Selects geometries within a bounding&nbsp;GeoJSON geometry. The&nbsp;2dsphere&nbsp;and&nbsp;2d&nbsp;indexes support&nbsp;$geoWithin.</td>
		</tr><tr><td>$near</td>
			<td>Returns geospatial objects in proximity to a point. Requires a geospatial index. The&nbsp;2dsphereand&nbsp;2d&nbsp;indexes support&nbsp;$near.</td>
		</tr><tr><td>$nearSphere</td>
			<td>Returns geospatial objects in proximity to a point on a sphere. Requires a geospatial index. The&nbsp;2dsphere&nbsp;and&nbsp;2d&nbsp;indexes support&nbsp;$nearSphere.</td>
		</tr><tr><td rowspan="3">Array</td>
			<td>$all</td>
			<td>Matches arrays that contain all elements specified in the query.</td>
		</tr><tr><td>$elemMatch</td>
			<td>Selects documents if element in the array field matches all the specified&nbsp;$elemMatchconditions.</td>
		</tr><tr><td>$size</td>
			<td>Selects documents if the array field is a specified size.</td>
		</tr><tr><td colspan="1" rowspan="4">Bitwise</td>
			<td>$bitsAllClear</td>
			<td>Matches numeric or binary values in which a set of bit positions&nbsp;all&nbsp;have a value of&nbsp;0.</td>
		</tr><tr><td>$bitsAllSet</td>
			<td>Matches numeric or binary values in which a set of bit positions&nbsp;all&nbsp;have a value of&nbsp;1.</td>
		</tr><tr><td>$bitsAnyClear</td>
			<td>Matches numeric or binary values in which&nbsp;any&nbsp;bit from a set of bit positions has a value of&nbsp;0.</td>
		</tr><tr><td>$bitsAnySet</td>
			<td>Matches numeric or binary values in which&nbsp;any&nbsp;bit from a set of bit positions has a value of&nbsp;1.</td>
		</tr><tr><td>Comments</td>
			<td>$comment</td>
			<td>Adds a comment to a query predicate.</td>
		</tr><tr><td colspan="1" rowspan="4">Projection</td>
			<td>$</td>
			<td>Projects the first element in an array that matches the query condition.</td>
		</tr><tr><td>$elemMatch</td>
			<td>Projects the first element in an array that matches the specified&nbsp;$elemMatchcondition.</td>
		</tr><tr><td>$meta</td>
			<td>Projects the document’s score assigned during&nbsp;$text&nbsp;operation.</td>
		</tr><tr><td>$slice</td>
			<td>Limits the number of elements projected from an array. Supports skip and limit slices.</td>
		</tr></tbody></table>

例如下面这个例子，获取 customer 集合中所有 name 字段值等于 "tracy" 或者 "lisa" 的文档。

	> db.customer.find({name:{$in:["tracy","lisa"]}})
	> 
或者也可以使用 $or 操作符实现相同的功能，查询语句如下：

	> db.customer.find({$or:[{name:"tracy"},{name:"lisa"}]})

该操作相当于关系型数据库中的 SELECT * FROM customer WHERE name IN ( "tracy","lisa" ) 查询SQL。

再举一个更复杂的例子，获取 customer 集合中所有 age < 25 并且 height > 165 或者 hobby 字段值中包含 "yoga" 的文档。

	> db.customer.find({age:{$lte:25},$or:[{'features.height':{$gt:165}},{hobby:{$in:["yoga"]}}]})

## 查询嵌套文档
当对内嵌文档的属性进行匹配查询时，使用 “.”符号进行操作，例如以下示例：查询 features 文档中 height > 170 的文档。

	> db.customer.find({'features.height':{$gt:170}}).pretty()
	{
		"_id" : ObjectId("5bcfeb4642f5fcf5f319f0d8"),
		"name" : "lisa",
		"age" : 27,
		"features" : {
			"height" : 178,
			"weight" : 63
		},
		"hobby" : [
			"yoga",
			"cartoon",
			"movie"
		]
	}

## 查询数组
当使用数组字段对查询文档进行过滤时，需格外注意数组元素的个数和顺序。

例如：查询 hobby = ["yoga","movie"] （yoga 在前）的文档。

	> db.customer.find({hobby:["yoga","movie"]}).pretty()
	{
		"_id" : ObjectId("5bd03128c90c1f1d66f50315"),
		"name" : "blooth",
		"age" : 24,
		"features" : {
			"height" : 166,
			"weight" : 55
		},
		"hobby" : [
			"yoga",
			"movie"
		]
	}

若将 hobby 字段的值改为 ["movie","yoga"] （movie 在前），将获取到不同的查询结果。

	> db.customer.find({hobby:["movie","yoga"]}).pretty()

如果你只是希望获取 hobby 字段值仅包含 "yoga" 和 "movie"的文档，而并不在意两个元素在数组中的顺序如何，可以使用 $all 操作符，并限制数组元素的个数为 2。

	> db.customer.find({$and:[{hobby:{$all:["movie","yoga"]}},{hobby:{$size:2}}]}).pretty()
	{
		"_id" : ObjectId("5bd03128c90c1f1d66f50315"),
		"name" : "blooth",
		"age" : 24,
		"features" : {
			"height" : 166,
			"weight" : 55
		},
		"hobby" : [
			"yoga",
			"movie"
		]
	}

下面列举了一些使用数组元素对文档进行查询过滤的典型示例：

	// 查询 hobby 数组中包含 yoga 的文档
	> db.customer.find({hobby:"yoga"})
	// 查询 scores 数组中至少有一个值大于 94 的文档
	> db.customer.find({scores:{$gt:94}})
	// 查询 scores 数组中至少有一个值大于90，且有一个值小于95的文档
	> db.customer.find({scores:{$gt:90,$lt:95}})
	// 查询 scores 数组中至少有一个值大于90且小于95的文档
	> db.customer.find({scores:{$elemMatch:{$gt:90,$lt:95}}})
	// 查询 scores 数组中第三个元素的值大于80且小于90的文档
	> db.customer.find({'scores.2':{$gt:80,$lt:90}})
	// 查询 scores 数组中元素个数为 5 的文档
	> db.customer.find({scores:{$size:5}})

## 查询文档数组
当数组字段中存储的元素又是文档对象时，除了要格外注意数组中文档元素的个数和顺序，同时还要注意数组中元素文档自身内部字段的数量和顺序。

例如：

	> db.inventory.find({"instock":[{warehouse:"A",qty:5},{warehouse:"C",qty:15}]})
	// 不同于
	> db.inventory.find({"instock":[{warehouse:"C",qty:15},{warehouse:"A",qty:5}]})
	 
	> db.inventory.find({"instock":{warehouse:"A",qty:5}})
	// 不同于
	> db.inventory.find({"instock":{qty:5,warehouse:"A"}})

下面列举了一些使用文档数组内文档元素对文档进行查询过滤的典型示例：

	// 查询 instock 数组中至少包含一个 qty ≥ 20 的文档元素的文档
	db.inventory.find( { 'instock.qty': { $lte: 20 } } )
	// 查询 instock 数组中第一个文档元素的 qty ≤ 20 的文档
	db.inventory.find( { 'instock.0.qty': { $lte: 20 } } )
	// 查询 instock 数组中至少包含一个 qty=5 并且 warehouse="A" 的文档元素的文档
	> db.inventory.find({"instock":{$elemMatch:{qty:5,warehouse:"A"}}})
	// 查询 instock 数组中至少包含一个 10<qty≤ 20  的文档元素的文档
	> db.inventory.find({"instock":{$elemMatch:{qty:{$gt:10,$lte:20}}}})
	// 查询 instock 数组中至少包含一个 10<qty  的文档元素和一个 qty≤20 的文档元素的文档
	> db.inventory.find({"instock.qty":{$gt:10,$lte:20}})
	// 查询 instock 数组中至少包含一个 qty=5  的文档元素和一个 warehouse="A"  的文档元素的文档
	> db.inventory.find({"instock.qty":5,"instock.warehouse":"A"})

## 设置查询结果返回字段
在使用 `db.collection.find()` 方法进行文档查询时，如果未指定投影文档参数，则会将查询到的匹配文档的所有字段进行返回。

例如执行 `db.inventory.find({status:"A"})` 就相当于关系型数据库中的 `SELECT * from inventory WHERE status = "A"` 查询SQL。

如果你想仅返回匹配文档的指定字段的内容，可以通过 db.collection.find() 方法的投影文档参数进行设置。

下面列举了一些常见的对匹配文档返回字段进行配置的典型示例：

	// 仅返回 _id、item、status 三个字段
	> db.inventory.find({},{item:1,status:1})
	// 仅返回 item、status 两个字段
	> db.inventory.find({},{item:1,status:1,_id:0})
	// 返回除 status 和 instock 以外字段
	> db.inventory.find({},{status:0,instock:0})
	// 仅返回 _id、item、status 三个字段和 size 内嵌文档的 uom 字段
	db.inventory.find({},{item:1,status:1,"size.uom":1})
	// 返回除 size 内嵌文档的 uom 字段以外的所有字段
	db.inventory.find({},{"size.uom":0})
	// 仅返回 _id、item、status 三个字段和 instock 数组内文档元素的 qty字段
	db.inventory.find({},{item:1,status:1,"instock.qty":1})
	// 仅返回 _id、item、status 三个字段和 instock 数组内最后3个元素
	> db.inventory.find({},{item:1,status:1,instock:{$slice:-3}})

## 匹配空值或字段
下面列举了一些常见的对空值和字段进行匹配的典型示例：

	// 返回 item 字段值为 null 或者不包含 item 字段的文档
	db.inventory.find({item:null})
	// 返回 item 字段值为 BSON 类型的文档
	db.inventory.find({item:{$type:10}})
	// 返回不包含 item 字段的文档
	db.inventory.find({item:{$exists:false}})

## 使用游标进行迭代
在 MongoDB Shell 中，db.collection.find() 方法返回一个游标，我们可以使用它对匹配到的文档进行遍历（默认最多 20 次，使用 DBQuery.shellBatchSize 参数配置）。
	
	> var myCursor=db.customer.find();
	> var documentArray = myCursor.toArray();
	> var myDocument = documentArray[3];
	// var myDocument = myCursor[1];
	// myCursor.forEach(printjson);
	> while (myCursor.hasNext()) {
	     print(tojson(myCursor.next()));
	     // printjson(myCursor.next());
	    }


# 更新文档
MongoDB 提供了下面两个方法用来更新指定集合内部的文档：

	db.collection.update()
	db.collection.updateOne()
	db.collection.updateMany()
	db.collection.replaceOne()

`db.collection.updateOne()`方法用来更新匹配文档中的第一个文档，例如下面这个例子会将 name="lisa" 的第一个文档中的 age 字段值更新为 28 。

	> db.customer.updateOne({name:"lisa"},{$set:{age:28},$currentDate: { lastModified: true }})

`db.collection.updateMany()`方法用来更新所有匹配文档，例如下面这个例子会将所有 name="lisa" 的文档中的 age 字段值更新为 28 。

	> db.customer.updateMany({name:"lisa"},{$set:{age:28},$currentDate: { lastModified: true }})

`db.collection.replaceOne()`方法用来对匹配文档中的第一个文档进行替换，例如下面这个例子会将 name="lisa" 的第一个文档替换为{name:"sasa",age:26}。

	> db.customer.replaceOne({name:"lisa"},{name:"sasa",age:26})

# 删除文档
MongoDB 提供了下面两个方法用来更新指定集合内部的文档：

	db.collection.remove()
	db.collection.deleteOne()
	db.collection.deleteMany()

`db.collection.deleteOne()` 方法用来删除匹配文档中的第一个文档，例如下面这个例子将会删除 name="lisa" 的第一个文档 。

	> db.customer.deleteOne({name:"lisa"})

`db.collection.deleteMany()`方法用来删除所有匹配文档，例如下面这个例子会将所有 name="lisa" 的文档删除掉 。

	> db.customer.deleteMany({name:"lisa"})