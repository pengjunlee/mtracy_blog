# 安装redis
在python中操作Redis数据库需要先安装redis模块。

	pip install redis

# 连接redis
redis提供`Redis`和`StrictRedis`两个类用于操作Redis，StrictRedis用于实现大部分官方的命令，并使用官方的语法和命令，Redis是StrictRedis的子类，用于向后兼容旧版本的redis-py。

redis连接实例是线程安全的，可以直接将redis连接实例设置为一个全局变量，直接使用。如果需要另一个Redis实例（or Redis数据库）时，就需要重新创建redis连接实例来获取一个新的连接。同理，python的redis没有实现select命令。

## 直接连接
连接redis时，加上`decode_responses=True`，使得写入的键值对中的value为str类型，不加这个参数写入的则为字节类型。

	import redis
	 
	# host redis主机名称或IP地址，port redis端口 
	r = redis.Redis(host='localhost', port=6379, decode_responses=True)

## 连接池连接
redis-py使用connection pool来管理对一个redis server的所有连接，避免每次建立、释放连接的开销。默认，每个Redis实例都会维护一个自己的连接池。

	import redis
	 
	pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)  
	r = redis.Redis(connection_pool=pool)

## 哨兵连接 
	
	# coding:utf-8
	 
	import redis
	from redis.sentinel import Sentinel
	 
	# 连接哨兵服务器(主机名也可以用域名)
	sentinel = Sentinel([('172.16.250.233', 26379),
	                     ('172.16.250.234', 26379),
	                     ('172.16.250.237', 26379)
	                     ],
	                    socket_timeout=0.5)
	 
	# 获取主服务器地址
	master = sentinel.discover_master('sentinel-monitor')
	print(master)
	# 输出：('172.16.250.233', 6379)
	 
	# 获取从服务器地址
	slave = sentinel.discover_slaves('sentinel-monitor')
	print(slave)
	# 输出：[('172.16.250.237', 6379), ('172.16.250.234', 6379)]
	 
	# 获取主服务器进行写入
	master = sentinel.master_for('sentinel-monitor', socket_timeout=0.5, password='sentinel_auth_pass', db=15)
	w_ret = master.set('foo', 'bar')
	# 输出：True
	 
	# 获取从服务器进行读取（默认是round-roubin）
	slave = sentinel.slave_for('sentinel-monitor', socket_timeout=0.5, password='sentinel_auth_pass', db=15)
	r_ret = slave.get('foo')
	print(r_ret)
	# 输出：b'bar'

# 常用操作

## String操作

	'''
	get(name)
	获取name键对应的值
	'''
	print(r.get('book'))
	 
	'''
	set(name, value, ex=None, px=None, nx=False, xx=False)
	设置name键对应的值，不存在则创建，存在则更新
	参数：
	ex，过期时间（秒）
	px，过期时间（毫秒）
	nx，如果设置为True，则只有name键不存在时，才会执行当前set操作
	xx，如果设置为True，则只有name键存在时，才会执行当前set操作
	'''
	r.set('book','A Long Story') # 为 book 设置值,若已存在则更新
	r.set('book','Three Stones',xx=True) # 当 book 存在时才为其设置值
	r.set('book','Mastering Redis',ex=5,nx=True) # 当 book 不存在时才为其设置值，并设定键的过期时间为5秒
	r.set('book','Mastering MongoDB',px=5000) # 为 book 设置值，并设定键的过期时间为5000毫秒
	 
	'''
	setnx(name, value)
	只在name键不存在时，才设置name键对应的值
	'''
	r.setnx('book','Mastering Redis') # 当 book 不存在时才为其设置值
	 
	'''
	setex(name, time, value)
	只在name键存在时，才设置name键对应的值
	参数：
	time，过期时间（数字秒 或 timedelta对象）
	'''
	r.setex('book',5,'Mastering MongoDB') # 更新 book 的值，并将其过期时间设为 5 秒
	 
	'''
	psetex(name, time_ms, value)
	只在name键存在时，才设置name键对应的值
	参数：
	time_ms，过期时间（数字毫秒 或 timedelta对象）
	'''
	r.psetex('book',5000,'Mastering MongoDB') # 更新 book 的值，并将其过期时间设为 5000 毫秒
	 
	'''
	mset(*args, **kwargs)
	批量设置指定键对应的值
	'''
	r.mset({'book1':'Mastering Redis','book2':'Mastering MongoDB'})
	 
	'''
	mget(keys, *args)
	批量获取指定键对应的值
	'''
	r.mget('book1','book2')
	r.mget(['book1','book2'])
	 
	'''
	getset(name, value)
	为name键设置新值并获取原来的旧值
	'''
	r.getset('book','Three Stones')
	 
	'''
	getrange(key, start, end)
	按字节截取字符串（一个汉字三个字节）
	参数：
	start，起始位置（字节）
	end，结束位置（字节）
	'''
	r.set('book','Mastering Redis')
	r.getrange('book',0,5)  # 结果 Master
	r.getrange('book',-5,-1)  # 结果 Redis
	r.getrange('book',0,-7)  # 结果 Mastering
	 
	'''
	setrange(name, offset, value)
	按字节替换字符串指定位置的内容（一个汉字三个字节）
	参数：
	offset，起始位置（字节）
	value，替换内容
	'''
	r.set('book','Mastering Redis')
	r.setrange('book',10,'MongoDB')  # 结果 Mastering MongoDB
	r.setrange('book',20,'2017')  # 结果 Mastering MongoDB   2017，空白处被"\x00"填充
	 
	'''
	getbit(name, offset)
	获取name键指定偏移位上的值
	setbit(name, offset, value)
	设置name键指定偏移位上的值
	参数：
	offset，偏移位置
	value，值只能是 1 或 0
	'''
	r.setbit('bits',100,1)
	r.getbit('bits',100)  # 结果 1
	 
	'''
	bitcount(name, start=None, end=None)
	统计name中值为1的位的数量
	参数：
	start，起始位置
	end，结束位置
	'''
	r.bitcount('bits',0,100)  # 结果 1
	 
	'''
	bitop(operation, dest, *keys)
	对指定的键进行位操作
	参数：
	operation，AND（并） 、 OR（或） 、 NOT（非） 、 XOR（异或）
	dest，用于存放结果
	*keys,要操作的键
	'''
	r.setbit('bits1',0,1)  # 0001
	r.setbit('bits1',3,1)  # 1001
	 
	r.setbit('bits2',0,1)  # 0001
	r.setbit('bits2',1,1)  # 0011
	 
	r.bitop('AND','bits_res1','bits1','bits2')  # 0001
	r.bitop('OR','bits_res2','bits1','bits2')  # 1011
	r.bitop('XOR','bits_res3','bits1','bits2')  # 1010
	r.bitop('NOT','bits_res4','bits1')  # 11110110
	 
	'''
	strlen(name)
	返回name键对应值的字节长度（一个汉字3个字节）
	'''
	r.set('book','思想品德')
	print(r.strlen('book')) # 结果 12
	 
	r.set('book','Halo')
	print(r.strlen('book')) # 结果 4
	 
	'''
	incr(self, name, amount=1)
	name键对应值自增，当name键不存在时，会先将其值初始化为 0，再执行操作
	decr(self, name, amount=1)
	name键对应值自减，当name键不存在时，会先将其值初始化为 0，再执行操作
	参数：
	amount,自增数/自减数（必须是整数）
	'''
	r.set('total',8)
	r.incr('total',amount=2) # 结果 10
	r.decr('total',amount=1) # 结果 9
	 
	r.set('price',8.8)
	r.incrbyfloat('price',amount=1.1) # 结果 9.9
	 
	'''
	append(name, value)
	向name键对应值中追加内容
	参数：
	value, 要追加的字符串
	'''
	r.set('book_name','Mastering')
	r.append('book_name',' Redis') # 结果 Mastering Redis

## Hash操作 

	'''
	hget(name,key)
	获取name键对应Hash表中key字段的值
	'''
	r.hget('user','weight')
	 
	'''
	hset(name,key,value)
	设置name键对应Hash表中key字段的值为value，不存在则创建，存在则更新
	'''
	r.hset('user','name',"Smith Lee") # 当 name 不存在时，会新建一个哈希表并设置域的值
	r.hset('user','height',180)
	r.hset('user','height',179) # 当 key 域已经存在于哈希表时，会用新值覆盖旧值
	 
	'''
	hsetnx(name,key,value)
	当name键对应Hash表中key字段不存在时，才将其值设置为value
	'''
	r.hsetnx('user','weight',70.2)
	 
	'''
	hmset(name, mapping)
	批量设置值
	hmget(name, keys, *args)
	批量获取值
	hgetall(name)
	获取全部键值对
	hlen(name)
	获取name键对应Hash表中的字段数量
	hkeys(name)
	获取name键对应Hash表中的所有字段名
	hvals(name)
	获取name键对应Hash表中的所有字段值
	hexists(name, key)
	判断name键对应Hash表中是否存在key字段
	hdel(name,*keys)
	'''
	r.hmset('user',{'name':'Huang','height':175,'weight':69})
	r.hmget('user',['name','height']) # 结果 ['Huang', '175']
	r.hmget('user','name','height') # 结果 ['Huang', '175']
	r.hgetall('user') # 结果 {'name': 'Huang', 'height': '175', 'weight': '69'}
	r.hlen('user') # 结果 3
	r.hkeys('user') # 结果 ['name', 'height', 'weight']
	r.hvals('user') # 结果 ['Huang', '175', '69']
	r.hexists('user','weight') # 结果 True
	r.hdel('user','weight')
	r.hexists('user','weight') # 结果 False
	 
	'''
	hincrby(name, key, amount=1)
	对name键对应Hash表中key字段进行自增或自减，amount 为正数则自增，amount为负数则自减
	参数：
	amount，自增数/自减数（必须是整数）
	'''
	r.hset('user','age',28)
	r.hincrby('user','age',amount=2) # 结果 30
	r.hincrby('user','age',amount=-1) # 结果 29
	 
	'''
	hincrbyfloat(name, key, amount=1.0)
	对name键对应Hash表中key字段进行浮点数自增或自减，amount 为正数则自增，amount为负数则自减
	参数：
	amount，自增数/自减数（必须是浮点数或整数）
	'''
	r.hset('user','weight',70.5)
	r.hincrbyfloat('user','weight',amount=-0.1) # 结果 70.400000000000006
	
## List操作 

Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
	
	'''
	lpush(name,values)
	向name键对应的列表头部（左侧）插入数据
	rpush(name,values)
	向name键对应的列表尾部（右侧）插入数据
	lpushx(name,values)
	只有当 name 键存在时，向头部（左侧）插入数据
	rpushx(name,values)
	只有当 name 键存在时，向尾部（右侧）插入数据
	'''
	r.delete('books')
	r.lpush('books','book0')
	r.lpush('books','book1')
	r.rpush('books','book2')
	r.lrange('books',0,-1) # 结果 ['book1', 'book0', 'book2']
	r.lpushx('books','book3')
	r.rpushx('books','book4')
	r.lrange('books',0,-1) # 结果 ['book3', 'book1', 'book0', 'book2', 'book4']
	 
	'''
	linsert(name, where, refvalue, value))
	在name键对应的列表中的指定值的前面（左侧）或后面（右侧）插入数据
	参数：
	where，BEFORE或AFTER
	refvalue，参考值，将在它前后插入数据
	value，要插入的数据
	'''
	r.linsert('books','BEFORE','book1','book5')
	r.linsert('books','AFTER','book2','book6')
	r.lrange('books',0,-1) # 结果 ['book3', 'book5', 'book1', 'book0', 'book2', 'book6', 'book4']
	 
	'''
	lset(name, index, value)
	修改name键对应的列表中指定索引处的值
	'''
	r.lset('books',1,'book8')
	r.lrange('books',0,-1) # 结果 ['book3', 'book8', 'book1', 'book0', 'book2', 'book6', 'book4']
	 
	'''
	lrem(name,count,value)
	删除name键对应的列表中num个值为value的元素
	参数：
	count， 要删除的元素个数，0表示删除所有
	value，要删除的值
	'''
	r.lrem('books',0,'book0')
	r.lrange('books',0,-1) # 结果 ['book3', 'book8', 'book1', 'book2', 'book6', 'book4']
	 
	'''
	rpop(name)
	从列表左侧弹出一个元素
	'''
	r.lpop('books') # 结果 'book3'
	r.lrange('books',0,-1) # 结果 ['book8', 'book1', 'book2', 'book6', 'book4']
	 
	'''
	ltrim(name, start, end)
	修剪列表，仅保留start-end索引之间的元素
	参数：
	start，索引的起始位置
	end，索引结束位置
	'''
	r.ltrim('books',1,3)
	r.lrange('books',0,-1) # 结果 ['book1', 'book2', 'book6']
	'''
	lindex(name, index)
	获取index索引处的元素
	'''
	r.lindex('books',1) # 结果 'book2'
	 
	'''
	rpoplpush(src, dst)
	从src列表右侧移出一个元素返回，并将其添加到dst列表左侧
	brpoplpush(src, dst, timeout=0)
	以阻塞模式，从src列表右侧移出一个元素返回，并将其添加到dst列表左侧
	参数：
	src，要取数据的列表的name
	dst，要添加数据的列表的name
	timeout，阻塞超时时间
	'''
	r.lpush('hot_books','book7')
	r.rpoplpush('books','hot_books')
	r.lrange('books',0,-1) # 结果 ['book1', 'book2']
	r.lrange('hot_books',0,-1) # 结果 ['book6', 'book7']
	 
	'''
	blpop(keys, timeout)
	以阻塞模式，从左侧弹出
	brpop(keys, timeout)
	以阻塞模式，从右侧弹出
	如果给定 key 内至少有一个非空列表，那么弹出遇到的第一个非空列表的头元素，并和被弹出元素所属的列表的名字一起，组成结果返回给调用者。 
	'''
	r.rpush('books','book3','book4')
	r.lrange('books',0,-1) # 结果 ['book1', 'book2', 'book3', 'book4']
	r.blpop(['books','hot_books'],timeout=2) # 结果 ('books', 'book1')

Set操作 
Redis的Set是String类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

Redis中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

	'''
	sadd(name,values)
	向name键对应的集合中添加元素
	scard(name)
	获取name键对应的集合中的元素个数
	smembers(name)
	获取name键对应的集合中所有的元素
	sscan(name, cursor=0, match=None, count=None)
	以元组形式，获取name键对应的集合中所有的元素
	'''
	r.delete('books')
	r.sadd('books','book0')
	r.sadd('books','book1')
	r.sadd('books','book2')
	r.sadd('books','book0') # 元素已存在，则不会再次添加
	r.scard('books') # 结果 3
	r.smembers('books') # 结果 {'book1', 'book2', 'book0'}
	r.sscan('books',cursor=0,match=None,count=None) # 结果 (0, ['book1', 'book0', 'book2'])
	 
	'''
	sdiff(keys, *args)
	求差集，返回在第一个集合中，却不在其他集合中的元素
	sdiffstore(dest, keys, *args)
	求差集，并将结果保存到dest中
	'''
	r.delete('hot_books')
	r.sadd('hot_books','book1')
	r.sadd('hot_books','book2')
	r.sadd('hot_books','book8')
	 
	r.sdiff('books','hot_books') # 结果 {'book0'}
	r.sdiffstore('diff_books','books','hot_books')
	r.smembers('diff_books') # 结果 {'book0'}
	 
	'''
	sinter(keys, *args)
	求交集，返回所有集合中都包含的元素
	sinterstore(dest, keys, *args)
	求交集，并将结果保存到dest中
	'''
	r.sinter('books','hot_books') # 结果 {'book1', 'book2'}
	r.sinterstore('inter_books','books','hot_books') # 结果 2
	 
	 
	'''
	sunion(keys, *args)
	求并集
	sunionstore(dest,keys, *args)
	求并集，并将结果保存到dest中
	'''
	r.sunion('books','hot_books') # 结果 {'book1', 'book0', 'book8', 'book2'}
	r.sunionstore('union_books','books','hot_books') # 结果 4
	r.smembers('union_books') # 结果 {'book1', 'book0', 'book8', 'book2'}
	 
	'''
	sismember(name, value)
	判断value是否是name集合中的成员
	smove(src, dst, value)
	将value从src移动到dst
	spop(name)
	从集合中随机弹出一个元素
	srem(name, values)
	删除指定的元素
	'''
	r.sismember('union_books','book8') # 结果 True
	r.smove('union_books','inter_books','book8')
	r.sismember('union_books','book8') # 结果 False
	r.smembers('inter_books') # 结果 {'book1', 'book8', 'book2'}
	 
	r.spop('inter_books')
	r.smembers('inter_books') # 结果 {'book1', 'book2'} 或者 {'book8', 'book2'} 或者{'book8', 'book1'}
	 
	r.srem('hot_books','book1','book2')
	r.smembers('hot_books') # 结果 {'book8'}

## Zset操作 

Redis有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

有序集合的成员是唯一的,但分数(score)却可以重复。

	'''
	zadd(name,mapping,nx=False,xx=False)
	向name键对应的有序集合中添加元素
	参数：
	mapping，要插入的元素字典
	nx，如果设置为True，则只有当name键不存在时，才会执行当前set操作
	xx，如果设置为True，则只有当name键存在时，才会执行当前set操作
	zcard(name)
	获取name键对应的有序集合中元素的个数
	'''
	r.delete('books')
	r.zadd('books',{'Mastering Redis':80,'A Long Story':90,'Three Stones':70})
	r.zcard('books') # 结果 2
	 
	'''
	zrange( name, start, end, desc=False, withscores=False, score_cast_func=float)
	获取索引在start-end之间的元素
	zrevrange(name, start, end, withscores=False, score_cast_func=float)
	对集合中的元素按照分数降序排列后，获取索引在start-end之间的元素
	zrangebyscore(name, min, max, start=None, num=None, withscores=False, score_cast_func=float)
	返回集合中分数范围在 min-max 之间的 num 个元素
	zrevrangebyscore(name, max, min, start=None, num=None, withscores=False, score_cast_func=float)
	对集合中的元素按照分数降序排列后，返回分数范围在 min-max 之间的 num 个元素
	参数：
	start，起始索引
	end，结束索引
	min，最小分数
	max，最大分数
	desc，是否降序排列
	withscores，是否返回分数
	score_cast_func，分数的类型转换函数
	'''
	r.zrange('books',0,-1,desc=True,withscores=True) # 结果 [('A Long Story', 90.0), ('Mastering Redis', 80.0), ('Three Stones', 70.0)]
	r.zrange('books',0,-1,desc=False,withscores=False) # 结果 ['Three Stones', 'Mastering Redis', 'A Long Story']
	 
	r.zrevrange('books',0,-1,withscores=True) # 结果 [('A Long Story', 90.0), ('Mastering Redis', 80.0), ('Three Stones', 70.0)]
	r.zrangebyscore('books',70,80,0,2,withscores=True) # 结果 [('Three Stones', 70.0), ('Mastering Redis', 80.0)]
	r.zrevrangebyscore('books',80,70,0,2,withscores=True) # 结果 [('Mastering Redis', 80.0), ('Three Stones', 70.0)]
	 
	'''
	zscan(name, cursor=0, match=None, count=None, score_cast_func=float)
	迭代集合中的元素
	zscan_iter(name, match=None, count=None,score_cast_func=float)
	迭代集合中的元素
	'''
	r.zscan('books') # 结果 (0, [('Three Stones', 70.0), ('Mastering Redis', 80.0), ('A Long Story', 90.0)])
	 
	for book in r.zscan_iter('books'):
	    print(book)                # 结果 ('Three Stones', 70.0) ('Mastering Redis', 80.0) ('A Long Story', 90.0)
	 
	'''
	zcount(name, min, max)
	获取分数在 min-max 之间的元素个数
	zincrby(name, amount, value)
	将name键对应的有序集合中的value元素自增或自减 amount
	zscore(name, value)
	获取name键对应的有序集合中的value元素的分数
	zrank(name, value)
	获取name键对应的有序集合中的value元素的索引，按分数从小到大排序
	zrevrank(name, value)
	获取name键对应的有序集合中的value元素的索引，按分数从大到小排序
	'''
	r.zrange('books',0,-1,desc=True,withscores=True) # 结果 [('A Long Story', 90.0), ('Mastering Redis', 80.0), ('Three Stones', 70.0)]
	r.zcount('books',70,80) # 结果 2
	r.zincrby('books',amount=1,value='Mastering Redis')
	r.zscore('books','Mastering Redis') # 结果 81.0
	r.zrank('books','A Long Story') # 结果 2
	r.zrevrank('books','A Long Story') # 结果 0
	 
	'''
	zrem(name, values)
	删除指定值的元素
	zremrangebyrank(name, min, max)
	根据索引范围删除元素，按分数从小到大排序
	zremrangebyscore(name, min, max)
	根据分数范围删除元素，按分数从小到大排序
	'''
	# r.zrem('books','Mastering Redis','Three Stones')
	# r.zremrangebyrank('books',0,1)
	# r.zremrangebyscore('books',70,80)

## Key操作

	'''
	delete(*names)
	删除指定的键
	exists(name)
	检验name键是否存在
	expire(name,time)
	设置name键的过期时间
	rename(src, dst)
	重命名
	randomkey()
	随机取一个key
	type(name)
	获取name键对应数据的类型
	keys(pattern='')
	根据模式匹配键
	dbsize()
	获取redis中的记录数
	flushdb()
	清空redis中的记录
	'''
	r.delete('books','hot_books')
	r.exists('books')
	r.randomkey()
	r.type("book1")
	# r.expire('book2',5)
	# r.rename('book2','new_book')
	r.keys('*book?') # 结果 ['diff_books', 'inter_books', 'union_books', 'book2', 'book1']
	r.dbsize()
	# r.flushdb()

## Pipeline操作

redis默认在执行每次请求都会创建（连接池申请连接）和断开（归还连接池）一次连接操作，

如果想要在一次请求中指定多个命令，则可以使用pipline实现一次请求指定多个命令，并且默认情况下一次pipline是原子性操作。

管道（pipeline）是redis在提供单个请求中缓冲多条服务器命令的基类的子类。它通过减少服务器-客户端之间反复的TCP数据库包，从而大大提高了执行批量命令的功能。

	import redis
	 
	r = redis.StrictRedis(host='localhost', port=6379, decode_responses=True)
	# r.pipeline(transaction=False)
	pipe = r.pipeline(transaction=True)  # 创建一个管道 ，transaction=True 启用事务，保证管道中执行命令的原子性
	pipe.set("stock",1000)
	pipe.incr('stock',amount=-1).incr('stock',amount=-1).incr('stock',amount=-1)
	pipe.execute()
	print(r.get('stock')) # 997

# 参考文章
<https://www.jianshu.com/p/2639549bedc8>