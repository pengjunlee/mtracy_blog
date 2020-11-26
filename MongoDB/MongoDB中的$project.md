> 原文链接：<https://blog.csdn.net/qq_18948359/article/details/88777314>

在`MongoDB`中可以使用`$project`来控制数据列的显示规则，可以执行的规则如下：

- 普通列（{成员：1 | true}）：表示要显示的内容
- "_id" 列（{"_id"：0 | false}）：表示 "_id" 列是否显示
- 条件过滤列（{成员：表达式}）：满足表达式之后的数据可以进行显示

首先，准备一点点数据

	db.getCollection('sales').insertMany([
	{ "_id" : 1, "item" : "abc", "price" : 10, "quantity" : 2, "date" : ISODate("2014-03-01T08:00:00Z") },
	{ "_id" : 2, "item" : "jkl", "price" : 20, "quantity" : 1, "date" : ISODate("2014-03-01T09:00:00Z") },
	{ "_id" : 3, "item" : "xyz", "price" : 5, "quantity" : 10, "date" : ISODate("2014-03-15T09:00:00Z") },
	{ "_id" : 4, "item" : "xyz", "price" : 5, "quantity" : 20, "date" : ISODate("2014-04-04T11:21:39.736Z") },
	{ "_id" : 5, "item" : "abc", "price" : 10, "quantity" : 10, "date" : ISODate("2014-04-04T21:23:13.331Z") }
	]);

# 基础操作
**示例：只显示出 item、price列**

	db.sales.aggregate( [ { $project : { _id: 0, item : 1 , price : 1 } } ] )

只有被设置进去的字段才会显示出来，其他列就不会被显示。

**示例：设置别名**

	// 通过 "$quantity" 来表示操作的字段
	db.sales.aggregate( [ { $project : { _id: 0, item : 1 , price : 1, qty: "$quantity" } } ] )

# 其他操作
在投影的过程中是可以进行四则远算的：加法（$add）、减法（$subtract）、乘法（$multipy）、除法（$divide）、求模（$mod）

**示例：对 quantity 乘以 2（使用 $multiply 操作）**

	// 对返回值的结果 放入到 quantity 下
	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1 , price : 1, quantity: {"qty" : { "$multiply" : ["$quantity", 2] }} } 
	}]);

**示例：直接使用别名返回数据**

	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1 , price : 1, "qty" : { "$multiply" : ["$quantity", 2] }} 
	}]);

除了四则运算之外，也支持下面的运算符：

- 关系运算：大小比较（"$cmp"）、等于（"$eq"）、大于（"$gt"）、大于等于（"$gte"）、小于（"$le"）、小于等于（"$lte"）、不等于（"$ne"）、判断 null （"$ifNull"），这些返回值都是 boolean 值类型的。
- 逻辑运算：与（"$and"）、或（"$or"）、非 （"$not"）
- 字符串操作：连接（"$concat"）、截取（"$substr"）、转小写（"$toLower"）

**示例：查询价格大于 5 的数据**

	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1, price: "$price", priceBoolean: {"$gt" : ["$quantity", 5] } } 
	}]);

**示例：查询 item 值为 abc 的数据**

	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1, price: 1, itemBoolean: {"$eq" : ["$item", "abc"] } } 
	}]);

**示例：查询 item 值为 abc 的数据，不区分大小写（在MongoDB 中，是区分大小的）**

	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1, price: 1, itemBoolean: {"$strcasecmp" : ["$item", "abc"] } } 
	}]);

另外一种写法（使用 $toUpper 转换数据）：

	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1, price: 1, itemBoolean: {"$eq" : ["$item", {"$toUpper": "abc"} ] } } 
	}]);

**示例：使用日期格式的数据**

	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1, price: "$price", dateInfo: { day: { $dayOfYear: "$date"}, year: { $year: "$date" } } } 
	}]);
	// 返回结果
	{ "item" : "abc", "price" : 10, "dateInfo" : { "day" : 60, "year" : 2014 } }
	{ "item" : "jkl", "price" : 20, "dateInfo" : { "day" : 60, "year" : 2014 } }
	{ "item" : "xyz", "price" : 5, "dateInfo" : { "day" : 74, "year" : 2014 } }
	{ "item" : "xyz", "price" : 5, "dateInfo" : { "day" : 94, "year" : 2014 } }
	{ "item" : "abc", "price" : 10, "dateInfo" : { "day" : 94, "year" : 2014 } }

**示例：连接字符串**

	db.sales.aggregate([{ $project : 
	    { _id: 0, item : 1, price: 1, itemStr: {"$concat" : ["$item", "_title"] } } 
	}]);
 