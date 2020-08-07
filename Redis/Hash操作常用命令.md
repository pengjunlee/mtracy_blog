# HGET

	# 命令语法：HGET key field
	# 命令用途：返回哈希表键 key 中给定域 field 的值。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hget book title // 当键不存在时，返回 nil
	(nil)
	127.0.0.1:6379> hset book title "Mastering Redis" 
	(integer) 1
	127.0.0.1:6379> hget book author    // 当键存在，但域不存在时，返回 nil
	(nil)
	127.0.0.1:6379> hget book title   // 当键和域都存在时，返回域的值
	"Mastering Redis"

# HSET

	# 命令语法：HSET key field value
	# 命令用途：将哈希表键 key 中的域 field 的值设为 value 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hset book title "Mastering Redis" // 当键不存在时，会新建一个哈希表并设置域的值
	(integer) 1
	127.0.0.1:6379> hget book title
	"Mastering Redis"
	127.0.0.1:6379> hset book title "Thinking In Java"  // 当域已经存在于哈希表时，会用新值覆盖旧值
	(integer) 0
	127.0.0.1:6379> hget book title
	"Thinking In Java"

# HSETNX

	# 命令语法：HSETNX key field value
	# 命令用途：当且仅当键 key 中的域 field  不存在时，将域 field  的值设为 value 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hsetnx book title "Mastering Redis"  // 当域不存在时为其设置值，设置成功
	(integer) 1
	127.0.0.1:6379> hget book title
	"Mastering Redis"
	127.0.0.1:6379> hsetnx book title "Thinking In Java" // 当域已经存在时再为其设置值，设置失败
	(integer) 0
	127.0.0.1:6379> hget book title
	"Mastering Redis"

# HLEN

	# 命令语法：HLEN key
	# 命令用途：返回哈希表键 key 中域的数量。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hlen book // 当键不存在时，返回 0
	(integer) 0
	127.0.0.1:6379> hset book title "Mastering Redis"
	(integer) 1
	127.0.0.1:6379> hset book author "Nelson"
	(integer) 1
	127.0.0.1:6379> hlen book
	(integer) 2

# HINCRBY

	# 命令语法：HINCRBY key field increment
	# 命令用途：将哈希表键 key 中的域 field中储存的数字值增加 increment 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hincrby book pagination 281 // 当域不存在时，会先将其值初始化为 0，再执行
	(integer) 281
	127.0.0.1:6379> hget book pagination
	"281"
	127.0.0.1:6379> hincrby book pagination -1 // HINCRBY操作中 increment 参数可以为负数
	(integer) 280
	127.0.0.1:6379> hget book pagination
	"280"

# HINCRBYFLOAT

	# 命令语法：HINCRBYFLOAT key field increment
	# 命令用途：为键 key 中的域 field 中储存的值加上浮点数增量 increment 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> hexists book price
	(integer) 0
	127.0.0.1:6379> hincrbyfloat book price 66.3 // 当域不存在时，会先将其值初始化为 0，再执行HINCRBYFLOAT操作 
	"66.3"
	127.0.0.1:6379> hget book price
	"66.3"
	127.0.0.1:6379> hincrbyfloat book price -15e-1 // 将 price 的值加 -15e-1即减去1.5
	"64.8"
	127.0.0.1:6379> hget book price
	"64.8"

# HMGET

	# 命令语法：HMGET key field1 [field2 ...]
	# 命令用途：返回哈希表键 key 中，一个或多个给定域的值。
	# 时间复杂度：O(N)， N 为给定域的数量。 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hset book title "Mastering Redis"
	(integer) 1
	127.0.0.1:6379> hset book author "Nelson"
	(integer) 1
	127.0.0.1:6379> hmget book title author price // 不存在域 price 返回 nil
	1) "Mastering Redis"
	2) "Nelson"
	3) (nil)

# HMSET

	# 命令语法：HMSET key field1 value1 [field2 value2 ...]
	# 命令用途：同时为哈希表键 key 设置一个或多个 key-value 键值对。
	# 时间复杂度：O(N)， N 为 field-value 对的数量。 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hmset book title "Mastering Redis" author "Nelson"
	OK
	127.0.0.1:6379> hmget book title author
	1) "Mastering Redis"
	2) "Nelson"

# HKEYS

	# 命令语法：HKEYS key
	# 命令用途：返回哈希表键 key 中的所有域。
	# 时间复杂度：O(N)， N 为哈希表中域的数量。 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hmset book title "Mastering Redis" author "Nelson"
	OK
	127.0.0.1:6379> hkeys book
	1) "title"
	2) "author"

# HVALS

	# 命令语法：HVALS key
	# 命令用途：返回哈希表键 key 中所有域的值。
	# 时间复杂度：O(N)， N 为哈希表中域的数量。 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hmset book title "Mastering Redis" author "Nelson"
	OK
	127.0.0.1:6379> hvals book
	1) "Mastering Redis"
	2) "Nelson"

# HGETALL

	# 命令语法：HGETALL key
	# 命令用途：返回哈希表键 key 中，所有的域和值。
	# 时间复杂度：O(N)， N 为哈希表中域的数量。 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hmset book title "Mastering Redis" author "Nelson" price 55.3
	OK
	127.0.0.1:6379> hgetall book
	1) "title"
	2) "Mastering Redis"
	3) "author"
	4) "Nelson"
	5) "price"
	6) "55.3"

# HEXISTS

	# 命令语法：HEXISTS key field
	# 命令用途：检验哈希表键 key 中，给定域 field 是否存在。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379>  hmset book title "Mastering Redis" author "Nelson" price 55.3
	OK
	127.0.0.1:6379>
	127.0.0.1:6379> hkeys book
	1) "title"
	2) "author"
	3) "price"
	127.0.0.1:6379> hexists book title
	(integer) 1
	127.0.0.1:6379> hexists book pagination
	(integer) 0

# HDEL

	# 命令语法：HDEL key field1 [field2 ...]
	# 命令用途：删除哈希表键 key 中的一个或多个域
	# 时间复杂度：O(N)， N 为要删除的域的数量。 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hmset book title "Mastering Redis" author "Nelson" price 55.3
	OK
	127.0.0.1:6379> hkeys book
	1) "title"
	2) "author"
	3) "price"
	127.0.0.1:6379> hdel book price
	(integer) 1
	127.0.0.1:6379> hkeys book
	1) "title"
	2) "author"
	127.0.0.1:6379> hdel book price author
	(integer) 1
	127.0.0.1:6379> hkeys book
	1) "title"

# HSCAN

	# 命令语法：HSCAN key cursor [MATCH pattern] [COUNT count]
	# 命令用途：迭代哈希表键 key 中的键值对。
	# 时间复杂度： O(N) ， N 为返回元素的数量。 
	# 命令参数： 

- MATCH pattern：只返回和给定模式 pattern 相匹配的域。 
- COUNT count：每次迭代从数据集返回 count 个元素。  

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> hset book title "Mastering Redis"
	(integer) 1
	127.0.0.1:6379> hmset book total:price 100 L1:price 90 L2:price 80 L3:price 70
	OK
	127.0.0.1:6379> hgetall book
	 1) "title"
	 2) "Mastering Redis"
	 3) "total:price"
	 4) "100"
	 5) "L1:price"
	 6) "90"
	 7) "L2:price"
	 8) "80"
	 9) "L3:price"
	10) "70"
	127.0.0.1:6379> hscan book 0 match *:price count 2 // 该命令通常会无视 COUNT 选项指定的值
	1) "0"
	2) 1) "total:price"
	   2) "100"
	   3) "L1:price"
	   4) "90"
	   5) "L2:price"
	   6) "80"
	   7) "L3:price"
	   8) "70"
	 