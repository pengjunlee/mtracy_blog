# GET

	# 命令语法：GET key
	# 命令用途：返回键 key 所关联的字符串值。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> get book // 当 book 不存在时，返回 nil
	(nil)
	127.0.0.1:6379> hset book name "Mastering Redis"
	(integer) 1
	127.0.0.1:6379> get book //当 book 不是字符串类型时，返回一个错误
	(error) WRONGTYPE Operation against a key holding the wrong kind of value
	127.0.0.1:6379> set book "Mastering Redis"
	OK
	127.0.0.1:6379> get book  // 当 book 存在且是字符串类型时，返回其所关联的字符串值
	"Mastering Redis"

# SET

	# 命令语法：SET key value [EX seconds] [PX milliseconds] [NX|XX]
	# 命令用途：设置键 key 所关联的字符串值为 value 。
	# 时间复杂度：O(1) 
	# 命令参数： 

- EX seconds：设置键的生存时间为 seconds 秒，效果等同于 SETEX 。
- PX milliseconds：设置键的生存时间为 milliseconds 毫秒，效果等同于 PSETEX 。
- NX：只在键不存在时，才对键进行设置操作，效果等同于 SETNX 。
- XX：只在键已经存在时，才对键进行设置操作。 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> set book "Mastering Redis" ex 5 nx // 当 book 不存在时才为其设置值，并设定键的生存时间为5秒
	OK
	127.0.0.1:6379> set book "Mastering MongoDB" px 5000 // 为 book 设置值，并设定键的生存时间为5000毫秒
	OK
	127.0.0.1:6379> set book "A Long Story"  // 为 book 设置值
	OK
	127.0.0.1:6379> set book "Three Stones" xx  // 当 book 存在时才为其设置值
	OK

# GETSET

	# 命令语法：GET key
	# 命令用途：将给定键 key 的值设为新值（new value）  ，并返回键 key 的旧值（old value）。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> set book "Mastering Redis" // 为 book 设置旧值
	OK
	127.0.0.1:6379> getset book "Three Stones" // 为 book 设置新值，并返回旧值
	"Mastering Redis"
	127.0.0.1:6379> get book  // 获取 book 的新值
	"Three Stones"

# SETNX

	# 命令语法：SETNX key value
	# 命令用途：当且仅当键 key 不存在时，将键 key 的值设为 value 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> setnx book "Mastering Redis" // 当 book 不存在时为其设置值，设置成功
	(integer) 1
	127.0.0.1:6379> get book
	"Mastering Redis"
	127.0.0.1:6379> setnx book "MongoDB Basic" // 当 book 存在时再为其设置值，设置失败
	(integer) 0
	127.0.0.1:6379> get book
	"Mastering Redis"

# SETEX

	# 命令语法：SETEX key seconds value
	# 命令用途：将值 value 关联到键 key ，并将键 key 的生存时间设为 seconds 秒。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> setex book 5 "Mastering Redis" // 为 book 设置值，并将其生存时间设为 5 秒
	OK
	127.0.0.1:6379> get book // 5 秒之内能够获取到 book 的值
	"Mastering Redis"
	127.0.0.1:6379> get book // 5 秒之后再获取 book 的值时，返回 nil
	(nil)

# PSETEX

	# 命令语法：PSETEX key milliseconds value
	# 命令用途：将值 value 关联到键 key ，并将键 key 的生存时间设为 milliseconds 毫秒。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> psetex book 5000 "Mastering Redis"// 为 book 设置值，并将其生存时间设为 5000 毫秒
	OK
	127.0.0.1:6379> get book // 5000 毫秒之内能够获取到 book 的值
	"Mastering Redis"
	127.0.0.1:6379> get book // 5000 毫秒之后再获取 book 的值时，返回 nil
	(nil)

# GETRANGE/SUBSTR

	# 命令语法：GETRANGE/SUBSTR key start end
	# 命令用途：截取并返回键 key 字符串值从 start 到 end 之间的自字符串(包括 start 和 end 在内)。
	# 时间复杂度：O(N)， N 为要返回的字符串的长度。 

<br/>

	127.0.0.1:6379> set book "Mastering Redis"
	OK
	127.0.0.1:6379> getrange book 0 5
	"Master"
	127.0.0.1:6379> getrange book -5 -1
	"Redis"
	127.0.0.1:6379> getrange book 0 -7
	"Mastering"

# SETRANGE

	# 命令语法：SETRANGE key offset value
	# 命令用途：从偏移量 offset 开始，用值 value 替换给定键 key 的字符串值。
	# 时间复杂度：O(M)， M 为值 value 的长度。 

<br/>

	127.0.0.1:6379> set book "Mastering Redis"
	OK
	127.0.0.1:6379> setrange book 10 MongoDB //从偏移量 10 开始替换字符串值
	(integer) 17
	127.0.0.1:6379> get book
	"Mastering MongoDB"
	127.0.0.1:6379> setrange book 20 2017 //空白处被"\x00"填充
	(integer) 24
	127.0.0.1:6379> get book
	"Mastering MongoDB\x00\x00\x002017"

# APPEND

	# 命令语法：APPEND key value
	# 命令用途：将值 value 追加到键 key 字符串值的末尾。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> append book "Mastering" // 当 book 不存在时
	(integer) 9
	127.0.0.1:6379> append book " Redis" // 当 book 存在时
	(integer) 15
	127.0.0.1:6379> get book
	"Mastering Redis"

# STRLEN

	# 命令语法：STRLEN key
	# 命令用途：返回键 key 字符串值的长度。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists book
	(integer) 0
	127.0.0.1:6379> strlen book // 当 book 不存在时，返回 0
	(integer) 0
	127.0.0.1:6379> set book "Mastering Redis"
	OK
	127.0.0.1:6379> strlen book
	(integer) 15

# INCR

	# 命令语法：INCR key
	# 命令用途：将键 key 中储存的数字值增1。
	# 时间复杂度：O(1) 

	127.0.0.1:6379> exists count
	(integer) 0
	127.0.0.1:6379> incr count // 当 count 不存在时，会先将其值初始化为 0，再执行INCR操作 
	(integer) 1
	127.0.0.1:6379> get count
	"1"
	127.0.0.1:6379> incr count // 将 count 的值加 1
	(integer) 2
	127.0.0.1:6379> get count
	"2"

# INCRBY

	# 命令语法：INCRBY key increment
	# 命令用途：将键 key 中储存的数字值增加 increment 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists count
	(integer) 0
	127.0.0.1:6379> incrby count 10  // 当 count 不存在时，会先将其值初始化为 0，再执行INCRBY操作 
	(integer) 10
	127.0.0.1:6379> get count
	"10"
	127.0.0.1:6379> incrby count 5 // 将 count 的值加 5
	(integer) 15
	127.0.0.1:6379> get count
	"15"

# INCRBYFLOAT

	# 命令语法：INCRBYFLOAT key increment
	# 命令用途：为键 key 中储存的值加上浮点数增量 increment 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists price
	(integer) 0
	127.0.0.1:6379> incrbyfloat price 0.5  // 当 price 不存在时，会先将其值初始化为 0，再执行INCRBYFLOAT操作 
	"0.5"
	127.0.0.1:6379> get price
	"0.5"
	127.0.0.1:6379> incrbyfloat price 5e-1 //  将 price 的值加 5e-1即0.5
	"1"
	127.0.0.1:6379> get price
	"1"
	127.0.0.1:6379> incrbyfloat price 1.00000000 // 小数末尾无用的 0 会自动被忽略掉
	"2"
	127.0.0.1:6379> get price
	"2"

# DECR

	# 命令语法：DECR key
	# 命令用途：将键 key 中储存的数字值减1。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists count
	(integer) 0
	127.0.0.1:6379> decr count // 当 count 不存在时，会先将其值初始化为 0，再执行DECR操作 
	(integer) -1
	127.0.0.1:6379> get count
	"-1"
	127.0.0.1:6379> decr count // 将 count 的值减 1
	(integer) -2
	127.0.0.1:6379> get count
	"-2"

# DECRBY

	# 命令语法：DECRBY key decrement
	# 命令用途：将键 key 所储存的数字值减去 decrement 。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists count
	(integer) 0
	127.0.0.1:6379> decrby count 10 // 当 count 不存在时，会先将其值初始化为 0，再执行DECRBY操作 
	(integer) -10
	127.0.0.1:6379> get count
	"-10"
	127.0.0.1:6379> decrby count 5  // 将 count 的值减 5
	(integer) -15
	127.0.0.1:6379> get count
	"-15"

# MGET

	# 命令语法：MGET key1 [key2 ...]
	# 命令用途：返回所有(一个或多个)给定键 key 的值。
	# 时间复杂度：O(N) , N 为给定 key 的数量。 

<br/>

	127.0.0.1:6379> set book:1 "Mastering Redis"
	OK
	127.0.0.1:6379> set book:2 "MongoDB Basic"
	OK
	127.0.0.1:6379> mget book:1 book:2 book:3 //不存在的 book:3 返回 nil
	1) "Mastering Redis"
	2) "MongoDB Basic"
	3) (nil)

# MSET

	# 命令语法：MSET key1 value1 [key2 value2 ...]
	# 命令用途：同时设置一个或多个 key-value 键值对。
	# 时间复杂度：O(N)， N 为要设置的键 key 的数量。 

<br/>

	127.0.0.1:6379> mset book:1 "Mastering Redis" book:2 "MongoDB Basic"
	OK
	127.0.0.1:6379>  mget book:1 book:2
	1) "Mastering Redis"
	2) "MongoDB Basic"

# MSETNX

	# 命令语法：MSETNX key1 value1[key2 value2 ...]
	# 命令用途：当且仅当所有给定 key 都不存在时，设置一个或多个 key-value 键值对。
	# 时间复杂度：O(N)， N 为要设置的键 key 的数量。 

<br/>

	127.0.0.1:6379> exists book:1 book:2 book:3
	(integer) 0
	127.0.0.1:6379> msetnx book:1 "Mastering Redis" book:2 "MongoDB Basic"
	(integer) 1
	127.0.0.1:6379> mget book:1 book:2 book:3
	1) "Mastering Redis"
	2) "MongoDB Basic"
	3) (nil)
	// MSETNX 是原子性的，由于 book:1 已经存在，从而导致 book:1 和 book:3 两个字段都没被设置
	127.0.0.1:6379> msetnx book:1 "Hello World Redis" book:3 "Thinking In Java"
	(integer) 0
	127.0.0.1:6379> mget book:1 book:2 book:3
	1) "Mastering Redis"
	2) "MongoDB Basic"
	3) (nil)

# GETBIT

	# 命令语法：GETBIT key offset
	# 命令用途：获取键 key 所储存的字符串值指定偏移量上的位(bit)。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists bits
	(integer) 0
	127.0.0.1:6379> getbit bits 100 // 对不存在的 key 或者不存在的 offset 进行 GETBIT， 返回 0
	(integer) 0
	127.0.0.1:6379> setbit bits 100 1
	(integer) 0
	127.0.0.1:6379> getbit bits 100
	(integer) 1

# SETBIT

	# 命令语法：SETBIT key offset value
	# 命令用途：设置或清除键 key 所储存的字符串值指定偏移量上的位(bit)。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists bits
	(integer) 0
	127.0.0.1:6379> setbit bits 100 1 // 设置偏移量 100 的位值为 1
	(integer) 0
	127.0.0.1:6379> getbit bits 100
	(integer) 1
	127.0.0.1:6379> setbit bits 100 0 // 清除偏移量 100 的位上的值
	(integer) 1
	127.0.0.1:6379> getbit bits 100
	(integer) 0
	127.0.0.1:6379> getbit bits 200 // 未被设置值的 bit 位默认被初始化为 0
	(integer) 0

# BITCOUNT

	# 命令语法：BITCOUNT key [start] [end]
	# 命令用途：返回键 key 所储存的字符串值中被设置为 1 的比特位的数量。
	# 时间复杂度：O(N) 

<br/>

	127.0.0.1:6379> exists bits
	(integer) 0
	127.0.0.1:6379> bitcount bits // 当 bits 不存在时，返回 0
	(integer) 0
	127.0.0.1:6379> setbit bits 0 1  // 0001
	(integer) 0
	127.0.0.1:6379> bitcount bits
	(integer) 1
	127.0.0.1:6379> setbit bits 3 1  // 1001
	(integer) 0
	127.0.0.1:6379> bitcount bits
	(integer) 2

# BITOP

	# 命令语法：BITOP operation destkey key1 [key2 ...]
	# 命令用途：对一个或多个保存二进制位的字符串 key 进行位元操作，并将结果保存到 destkey 上。
	# 时间复杂度：O(N)
	# 命令参数： 

- AND ：对一个或多个 key 求逻辑并，并将结果保存到 destkey 。
- OR ：对一个或多个 key 求逻辑或，并将结果保存到 destkey 。
- XOR ：对一个或多个 key 求逻辑异或，并将结果保存到 destkey 。 
- NOT ：对给定 key 求逻辑非，并将结果保存到 destkey 。  

<br/>

	127.0.0.1:6379> exists bits:1 bits:2
	(integer) 0
	127.0.0.1:6379> setbit bits:1 0 1 // 0001
	(integer) 0
	127.0.0.1:6379> setbit bits:1 3 1 // 1001
	(integer) 0
	127.0.0.1:6379> setbit bits:2 0 1 // 0001
	(integer) 0
	127.0.0.1:6379> setbit bits:2 1 1 // 0011
	(integer) 0
	127.0.0.1:6379> bitop and bits:result:1 bits:1 bits:2 // bits:result:1 = 0001
	(integer) 1
	127.0.0.1:6379> bitcount bits:result:1
	(integer) 1
	127.0.0.1:6379> getbit bits:result:1 0
	(integer) 1
	127.0.0.1:6379> bitop or bits:result:2 bits:1 bits:2 // bits:result:2 = 1011
	(integer) 1
	127.0.0.1:6379> bitcount bits:result:2
	(integer) 3
	127.0.0.1:6379> getbit bits:result:2 2
	(integer) 0
	127.0.0.1:6379> bitop xor bits:result:3 bits:1 bits:2 // bits:result:3 = 1010
	(integer) 1
	127.0.0.1:6379> bitcount bits:result:3
	(integer) 2
	127.0.0.1:6379> getbit bits:result:3 1
	(integer) 1
	127.0.0.1:6379> getbit bits:result:3 3
	(integer) 1
	127.0.0.1:6379> bitop not bits:result:4 bits:1 // bits:result:4 = 11110110
	(integer) 1
	127.0.0.1:6379> bitcount bits:result:4
	(integer) 6
	127.0.0.1:6379> getbit bits:result:4 1
	(integer) 1
	127.0.0.1:6379> getbit bits:result:4 0
	(integer) 0
	127.0.0.1:6379> getbit bits:result:4 3
	(integer) 0
	127.0.0.1:6379> getbit bits:result:4 7
	(integer) 1
	127.0.0.1:6379> getbit bits:result:4 8
	(integer) 0
  