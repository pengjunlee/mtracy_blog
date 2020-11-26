> 原文链接：<https://www.cnblogs.com/zgghb/p/6268509.html>

# 添加一个字段
`url`代表表名 , 添加字段`content`。 字符串类型。

	db.table.update({}, {$set: {content:""}}, {multi: 1})

# 删除一个字段

	db.table.update({},{$unset:{'content':''}},false, true)
 