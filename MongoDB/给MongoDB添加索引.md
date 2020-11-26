> 原文链接：<https://www.cnblogs.com/a-du/p/7805951.html>

用过数据库的都知道，数据库索引与书籍的索引类似，都是用来帮助快速查找的。

MongoDB的索引跟关系型数据库的索引几乎一致。

# 索引的创建
mongodb采用`ensureIndex`来创建索引，如：

	db.user.ensureIndex({"name":1})

表示在user集合的name键创建一个索引，这里的1表示索引创建的方向，可以取值为1和-1

在这里面，我们没有给索引取名字，mongodb会为我们取一个默认的名字，规则为

	keyname1_dir1_keyname2_dir2...keynameN_dirN

keyname表示键名，dir表示索引的方向，例如，上面的例子我们创建的索引名字就是name_1

索引还可以创建在多个键上，也就是联合索引，如：

	> db.user.ensureIndex({"name":1,"age":1})

这样就创建了name和age的联合索引

除了让mongodb默认索引的名字外，我们还可以去一个方便记的名字，方法就是为ensureIndex指定name的值，如：

	> db.user.ensureIndex({"name":1},{"name":"IX_name"})

这样，我们创建的索引的名字就叫IX_name了

# 唯一索引
与RDB类似，我们也可以定义唯一索引，方法就是指定unique键位true：

	>db.user.ensureIndex({"name":1},{"unique":true})

# 查看我们建立的索引
索引的信息存在每个数据库的system.indexes集合里面，对这个集合只能有ensureIndex和dropIndexes进行修改，不能手动插入或修改集合。

通过`> db.system.indexes.find()`可以找到数据库中多有的索引：

	> db.system.indexes.find() 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.entities", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.blog", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.authors", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.papers", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.analytics", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.user", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.food", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.user.info", "name" : "_id_" } 
	{ "v" : 1, "key" : { "_id" : 1 }, "ns" : "test.userinfo", "name" : "_id_" } 
	{ "v" : 1, "key" : { "name" : 1 }, "ns" : "test.user", "name" : "IX_name" }
 
# 删除索引
如果索引没有用了，可以使用dropIndexes将其删掉：

	> db.runCommand({"dropIndexes":"user","index":"IX_name"}) 
	{ "nIndexesWas" : 2, "ok" : 1 }

ok表示删除成功