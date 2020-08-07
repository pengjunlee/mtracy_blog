# LPUSH

	# 命令语法：LPUSH key value1 [value2 ...]
	# 命令用途：将一个或多个值 value 按照从左到右的顺序依次插入到列表 key 的表头。
	# 时间复杂度：O(1) 

<br>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lpush books "Mastering Redis" "Thinking In Java" // 添加多个元素
	(integer) 2
	127.0.0.1:6379> lrange books 0 -1
	1) "Thinking In Java"
	2) "Mastering Redis"
	127.0.0.1:6379> lpush books  "MongonDB Basic" "MongonDB Basic" // 允许添加重复元素
	(integer) 4
	127.0.0.1:6379> lrange books 0 -1
	1) "MongonDB Basic"
	2) "MongonDB Basic"
	3) "Thinking In Java"
	4) "Mastering Redis"

# RPUSH

	# 命令语法：RPUSH key value1 [value2 ...]
	# 命令用途：将一个或多个值 value 按照从左到右的顺序依次插入到列表 key 的表尾。
	# 时间复杂度：O(1) 

<br>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> rpush books "Mastering Redis" "Thinking In Java" // 添加多个元素
	(integer) 2
	127.0.0.1:6379> lrange books 0 -1
	1) "Mastering Redis"
	2) "Thinking In Java"
	127.0.0.1:6379> rpush books "MongoDB Basic"  "MongoDB Basic" // 允许添加重复元素
	(integer) 4
	127.0.0.1:6379> lrange books 0 -1
	1) "Mastering Redis"
	2) "Thinking In Java"
	3) "MongoDB Basic"
	4) "MongoDB Basic"

# LRANGE

	# 命令语法：LRANGE key start stop
	# 命令用途：返回列表 key 中下标在 start 和 stop 之间的元素。
	# 时间复杂度：O(S+N)， S 为偏移量 start ， N 为指定区间内元素的数量。 

<br>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lpush books "Redis" "Java" "PHP" "MongonDB"
	(integer) 4
	127.0.0.1:6379> lrange books 0 -1 // 0表示第一个元素，-1表示最后一个元素
	1) "MongonDB"
	2) "PHP"
	3) "Java"
	4) "Redis"
	127.0.0.1:6379> lrange books 1 -2
	1) "PHP"
	2) "Java"

# LINDEX

	# 命令语法：LINDEX key index
	# 命令用途：返回列表 key 中，下标为 index 的元素。
	# 时间复杂度：O(N)， N 为到达下标 index 过程中经过的元素数量。 

<br>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lpush books "Redis" "Java" "PHP" "MongonDB"
	(integer) 4
	127.0.0.1:6379> lrange books 0 -1
	1) "MongonDB"
	2) "PHP"
	3) "Java"
	4) "Redis"
	127.0.0.1:6379> lindex books 2
	"Java"

# LLEN

	# 命令语法：LLEN key
	# 命令用途：返回列表 key 的长度。
	# 时间复杂度：O(1) 

<br>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> llen books
	(integer) 0
	127.0.0.1:6379> lpush books "Redis" "Java" "PHP" "MongonDB"
	(integer) 4
	127.0.0.1:6379> llen books
	(integer) 4

# LINSERT

	# 命令语法：LINSERT key BEFORE|AFTER pivot value
	# 命令用途：将值 value 插入到列表 key 的值 pivot 之前或之后。
	# 时间复杂度：O(N)， N 为寻找 pivot 过程中经过的元素数量。 

<br>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lpush books "Redis" "Java" "PHP" "MongonDB"
	(integer) 4
	127.0.0.1:6379> lrange books 0 -1
	1) "MongonDB"
	2) "PHP"
	3) "Java"
	4) "Redis"
	127.0.0.1:6379> linsert books before "PHP" "Visual Basic"
	(integer) 5
	127.0.0.1:6379> lrange books 0 -1
	1) "MongonDB"
	2) "Visual Basic"
	3) "PHP"
	4) "Java"
	5) "Redis"
	127.0.0.1:6379> linsert books after "Java" "Python"
	(integer) 6
	127.0.0.1:6379> lrange books 0 -1
	1) "MongonDB"
	2) "Visual Basic"
	3) "PHP"
	4) "Java"
	5) "Python"
	6) "Redis"

# LSET

	# 命令语法：LSET key index value
	# 命令用途：将列表 key 下标为 index 的元素的值设置为 value 。
	# 时间复杂度：操作表头和表尾时，复杂度为O(1)，其余情况，复杂度为O(N)， N 为列表的长度。 

<br>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lset books 0 "Java" // 对空列表(key 不存在)进行 LSET，返回一个错误
	(error) ERR no such key
	127.0.0.1:6379> lpush books "Redis" "Java" "PHP" "MongonDB"
	(integer) 4
	127.0.0.1:6379> lrange books 0 -1
	1) "MongonDB"
	2) "PHP"
	3) "Java"
	4) "Redis"
	127.0.0.1:6379> lset books 1 "C++" 
	OK
	127.0.0.1:6379> lrange books 0 -1
	1) "MongonDB"
	2) "C++"
	3) "Java"
	4) "Redis"
	127.0.0.1:6379> lset books 4 "Visual Basic" // index 超出范围，返回一个错误
	(error) ERR index out of range

# LREM

	# 命令语法：LREM key count value
	# 命令用途：从列表 key 中移除 count 个与参数 value 相等的元素。
	# 时间复杂度：O(N)， N 为列表的长度。
	# 命令参数： 

- count > 0 :从表头开始向表尾搜索，移除 count 个与 value 相等的元素 。 
- count < 0 :从表尾开始向表头搜索，移除 count 个与 value 相等的元素。 
- count = 0 :移除表中所有与 value 相等的值。 

<br/>

	127.0.0.1:6379> lpush books "Java" "Java" "PHP" "Java" "Redis" "Java" "Redis"
	(integer) 7
	127.0.0.1:6379> lrange books 0 -1
	1) "Redis"
	2) "Java"
	3) "Redis"
	4) "Java"
	5) "PHP"
	6) "Java"
	7) "Java"
	127.0.0.1:6379> lrem books 2 "Java" // 移除从表头到表尾，最先发现的两个 Java
	(integer) 2
	127.0.0.1:6379> lrange books 0 -1
	1) "Redis"
	2) "Redis"
	3) "PHP"
	4) "Java"
	5) "Java"
	127.0.0.1:6379> lrem books -2 "Java" // 移除从表尾到表头，最先发现的两个 Java
	(integer) 2
	127.0.0.1:6379> lrange books 0 -1
	1) "Redis"
	2) "Redis"
	3) "PHP"
	127.0.0.1:6379> lrem books 0 "Redis" // 移除列表中所有 Redis
	(integer) 2
	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"

# LTRIM

	# 命令语法：LTRIM key start stop
	# 命令用途：修建列表 key ，只保留下标在 start 和 stop 之间的元素
	# 时间复杂度：O(N)， N 为被移除的元素的数量。 

<br/>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lpush books "Redis" "C++" "PHP" "Python"
	(integer) 4
	127.0.0.1:6379> lrange books 0 -1
	1) "Python"
	2) "PHP"
	3) "C++"
	4) "Redis"
	127.0.0.1:6379> ltrim books 1 2
	OK
	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"
	2) "C++"

# LPUSHX

	# 命令语法：LPUSHX key value
	# 命令用途：当且仅当 key 存在且为列表时，将值 value 插入到列表 key 的表头。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lpush books "Redis" "PHP"
	(integer) 2
	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"
	2) "Redis"
	127.0.0.1:6379> lpushx books "C++"
	(integer) 3
	127.0.0.1:6379> lrange books 0 -1
	1) "C++"
	2) "PHP"
	3) "Redis"

# RPUSHX

	# 命令语法：RPUSHX key value
	# 命令用途：当且仅当 key 存在且为列表时，将值 value 插入到列表 key 的表尾。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists books
	(integer) 0
	127.0.0.1:6379> lpush books "Redis" "PHP"
	(integer) 2
	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"
	2) "Redis"
	127.0.0.1:6379> rpushx books "C++"
	(integer) 3
	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"
	2) "Redis"
	3) "C++"

# LPOP

	# 命令语法：LPOP key
	# 命令用途：移除并返回列表 key 的头元素。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"
	2) "Redis"
	3) "C++"
	127.0.0.1:6379> lpop books
	"PHP"
	127.0.0.1:6379> lrange books 0 -1
	1) "Redis"
	2) "C++"

# RPOP

	# 命令语法：RPOP key
	# 命令用途：移除并返回列表 key 的尾元素。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"
	2) "Redis"
	3) "C++"
	127.0.0.1:6379> rpop books
	"C++"
	127.0.0.1:6379> lrange books 0 -1
	1) "PHP"
	2) "Redis"

# RPOPLPUSH

	# 命令语法：RPOPLPUSH source destination
	# 命令用途：移除并返回列表 source 的尾元素，同时，将其添加到 destination 列表的表头。
	# 时间复杂度：O(1) 

<br/>

	127.0.0.1:6379> exists source destination
	(integer) 0
	127.0.0.1:6379> lpush source "Hello" "2017" "Happy New Year"
	(integer) 3
	127.0.0.1:6379> lpush destination "World"
	(integer) 1
	127.0.0.1:6379> lrange source 0 -1
	1) "Happy New Year"
	2) "2017"
	3) "Hello"
	127.0.0.1:6379> lrange destination  0 -1
	1) "World"
	127.0.0.1:6379> rpoplpush source destination
	"Hello"
	127.0.0.1:6379> lrange source 0 -1
	1) "Happy New Year"
	2) "2017"
	127.0.0.1:6379> lrange destination  0 -1
	1) "Hello"
	2) "World"

# BLPOP

	# 命令语法：BLPOP key1 [key2 ...] timeout
	# 命令用途： LPOP 命令的阻塞版本，当给定列表内没有任何元素可供弹出的时候，连接将被 BLPOP 命令阻塞，直到等待超时或发现可弹出元素为止。当给定多个 key 参数时，按参数 key 的先后顺序依次检查各个列表，弹出第一个非空列表的头元素。
	# 时间复杂度：O(1)  
	# 命令场景： 


- 非阻塞行为：当 BLPOP 被调用时，如果给定 key 内至少有一个非空列表，那么弹出遇到的第一个非空列表的头元素，并和被弹出元素所属的列表的名字一起，组成结果返回给调用者。 
- 阻塞行为：如果所有给定 key 都不存在或包含空列表，那么 BLPOP 命令将阻塞连接，直到等待超时，或有另一个客户端对给定 key 的任意一个执行 LPUSH 或 RPUSH 命令为止。超时参数 timeout 接受一个以秒为单位的数字作为值。超时参数设为 0 表示阻塞时间可以无限期延长(block indefinitely) 。相同的key被多个客户端同时阻塞。不同的客户端被放进一个队列中，按『先阻塞先服务』(first-BLPOP，first-served)的顺序为 key 执行 BLPOP 命令。 

# BRPOP

	# 命令语法：BRPOP key1 [key2...] timeout
	# 命令用途：RPOP 命令的阻塞版本，当给定列表内没有任何元素可供弹出的时候，连接将被 BRPOP 命令阻塞，直到等待超时或发现可弹出元素为止。当给定多个 key 参数时，按参数 key 的先后顺序依次检查各个列表，弹出第一个非空列表的尾部元素。
	# 命令场景：参照BLPOP命令。
	# 时间复杂度：O(1)  

# BRPOPLPUSH

	# 命令语法：BRPOPLPUSH source destination timeout
	# 命令用途：RPOPLPUSH 的阻塞版本，当给定列表 source 不为空时， BRPOPLPUSH 的表现和 RPOPLPUSH 一样。当列表 source 为空时， BRPOPLPUSH 命令将阻塞连接，直到等待超时，或有另一个客户端对 source 执行 LPUSH 或 RPUSH 命令为止。超时参数 timeout 接受一个以秒为单位的数字作为值。超时参数设为 0 表示阻塞时间可以无限期延长(block indefinitely) 。
	# 时间复杂度：O(1)

**命令模式：**

## 安全的队列

Redis的列表经常被用作队列(queue)，用于在不同程序之间有序地交换消息(message)。一个客户端通过 LPUSH 命令将消息放入队列中，而另一个客户端通过 RPOP 或者 BRPOP 命令取出队列中等待时间最长的消息。

不幸的是，上面的队列方法是『不安全』的，因为在这个过程中，一个客户端可能在取出一个消息之后崩溃，而未处理完的消息也就因此丢失。

使用 RPOPLPUSH 命令(或者它的阻塞版本 BRPOPLPUSH )可以解决这个问题：因为它不仅返回一个消息，同时还将这个消息添加到另一个备份列表当中，如果一切正常的话，当一个客户端完成某个消息的处理之后，可以用 LREM 命令将这个消息从备份表删除。

最后，还可以添加一个客户端专门用于监视备份表，它自动地将超过一定处理时限的消息重新放入队列中去(负责处理该消息的客户端可能已经崩溃)，这样就不会丢失任何消息了。

## 循环列表

通过使用相同的 key 作为 RPOPLPUSH 命令的两个参数，客户端可以用一个接一个地获取列表元素的方式，取得列表的所有元素，而不必像 LRANGE 命令那样一下子将所有列表元素都从服务器传送到客户端中(两种方式的总复杂度都是 O(N))。 