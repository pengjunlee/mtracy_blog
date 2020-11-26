> 原文链接：<https://www.cnblogs.com/xuliuzai/p/9965229.html>

# 为普通字段添加索引，并且为索引命名

	db.集合名.createIndex( {"字段名": 1 },{"name":'idx_字段名'})

**说明**：

- 索引命名规范：idx_<构成索引的字段名>。如果字段名字过长，可采用字段缩写。
- 字段值后面的 1 代表升序；如是 -1 代表 降序。

# 为内嵌字段添加索引

	db.集合名.createIndex({"字段名.内嵌字段名":1},{"name":'idx_字段名_内嵌字段名'})

# 通过后台创建索引

	db.集合名.createIndex({"字段名":1},{"name":'idx_字段名',background:true})

# 组合索引

	db.集合名.createIndex({"字段名1":-1,"字段名2":1},{"name":'idx_字段名1_字段名2',background:true})

# 设置TTL 索引

	db.集合名.createIndex( { "字段名": 1 },{ "name":'idx_字段名',expireAfterSeconds: 定义的时间,background:true} )

**说明**：expireAfterSeconds为过期时间（单位秒）  

# createIndex() 接收可选参数汇总
<table border="0"><tbody><tr><td><strong>Parameter</strong></td><td><strong>Typ</strong></td><td><strong>Description</strong></td></tr><tr><td>background</td><td>Boolean</td><td>建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 "background" 默认值为<strong>false</strong>。</td></tr><tr><td>unique</td><td>Boolean</td><td>建立的索引是否唯一。指定为true创建唯一索引。默认值为<strong>false</strong>.</td></tr><tr><td>name</td><td>string</td><td>索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。</td></tr><tr><td>sparse</td><td>Boolean</td><td>对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为&nbsp;<strong>false</strong>.</td></tr><tr><td>expireAfterSeconds</td><td>integer</td><td>指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。</td></tr><tr><td>weights</td><td>document</td><td>索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。</td></tr><tr><td>default_language</td><td>string</td><td>对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语</td></tr></tbody></table>