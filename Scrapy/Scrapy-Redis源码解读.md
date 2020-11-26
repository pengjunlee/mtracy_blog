在上一章《Scrapy-Redis入门实战》中，我们在一个普通的Scrapy项目的settings.py文件中仅额外增加了如下几个配置就使项目实现了基于Redis的Requests请求过滤和Items持久化两大功能。

	######################################################
	##############下面是Scrapy-Redis相关配置################
	######################################################
	 
	# 指定Redis的主机名和端口
	REDIS_HOST = 'localhost'
	REDIS_PORT = 6379
	 
	# 调度器启用Redis存储Requests队列
	SCHEDULER = "scrapy_redis.scheduler.Scheduler"
	 
	# 确保所有的爬虫实例使用Redis进行重复过滤
	DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"
	 
	# 将Requests队列持久化到Redis，可支持暂停或重启爬虫
	SCHEDULER_PERSIST = True
	 
	# Requests的调度策略，默认优先级队列
	SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.PriorityQueue'
	 
	# 将爬取到的items保存到Redis 以便进行后续处理
	ITEM_PIPELINES = {
	    'scrapy_redis.pipelines.RedisPipeline': 300
	}

本文将通过解读Scrapy-Redis源码来进一步认识一下其中涉及的几个组件。

# RedisPipeline
`scrapy_redis.pipelines.RedisPipeline`用来将序列化后的Item持久化到Redis中。

主要源码如下，Scrapy-Redis正是通过其中的`_process_item()`方法来将序列化的Item保存到Redis列表的。

	from scrapy.utils.misc import load_object
	from scrapy.utils.serialize import ScrapyJSONEncoder
	from twisted.internet.threads import deferToThread
	 
	from . import connection, defaults
	 
	default_serialize = scrapy.utils.serialize.ScrapyJSONEncoder().encode
	 
	class RedisPipeline(object):
	    """将序列化后的Item保存到Redis 列表或者队列中
	    相关配置
	    --------
	    REDIS_ITEMS_KEY : str
	        保存Items 到 Redis 哪个key，默认 %(spider)s:items
	    REDIS_ITEMS_SERIALIZER : str
	        所使用的序列化方法，默认 scrapy.utils.serialize.ScrapyJSONEncoder().encode
	    """
	 
	    def __init__(self, server,
	                 key=defaults.PIPELINE_KEY,
	                 serialize_func=default_serialize):
	        """初始化 pipeline
	        参数
	        ----------
	        server : StrictRedis
	            Redis 客户端实例
	        key : str
	            保存Items 到 Redis 哪个key，默认 %(spider)s:items
	        serialize_func : callable
	            所使用的序列化方法，默认 scrapy.utils.serialize.ScrapyJSONEncoder().encode
	        """
	        self.server = server
	        self.key = key
	        self.serialize = serialize_func
	 
	    @classmethod
	    def from_settings(cls, settings):  # 从 settings.py 读取配置
	        params = {
	            'server': connection.from_settings(settings),
	        }
	        if settings.get('REDIS_ITEMS_KEY'):
	            params['key'] = settings['REDIS_ITEMS_KEY']
	        if settings.get('REDIS_ITEMS_SERIALIZER'):
	            params['serialize_func'] = load_object(
	                settings['REDIS_ITEMS_SERIALIZER']
	            )
	 
	        return cls(**params)
	 
	    @classmethod
	    def from_crawler(cls, crawler):  # 从 settings.py 读取配置
	        return cls.from_settings(crawler.settings)
	 
	    def process_item(self, item, spider):  # 另起一个线程来保存Item
	        return deferToThread(self._process_item, item, spider)
	 
	    def _process_item(self, item, spider):  # 保存Item
	        key = self.item_key(item, spider)  # 获取保存Item的Redis key
	        data = self.serialize(item)  # 将Item序列化
	        self.server.rpush(key, data)  # 将序列化后的 Item保存到Redis key
	        return item
	 
	    def item_key(self, item, spider):
	        """根据spider名称生成用于存放Items的 Redis key
	        通过重写这个方法来使用其他的key，可以使用item 和/或者 spider 来生成你想要的 key
	        默认 spider.name:items
	        """
	        return self.key % {'spider': spider.name}

# RFPDupeFilter
`scrapy_redis.dupefilter.RFPDupeFilter`是一个基于Redis的请求去重过滤器，它为Scheduler调度器提供了为Request生成指纹和判断Request是否重复等方法。

主要源码如下，重要部分已经添加上注释，其中`request_fingerprint()`用来为Request生成指纹，`request_seen()`用来判断Request是否重复。

	import logging
	import time
	 
	from scrapy.dupefilters import BaseDupeFilter
	from scrapy.utils.request import request_fingerprint
	 
	from . import defaults
	from .connection import get_redis_from_settings
	 
	 
	logger = logging.getLogger(__name__)
	 
	# TODO: Rename class to RedisDupeFilter.
	class RFPDupeFilter(BaseDupeFilter):
	    """基于Redis的请求去重过滤器.
	    Scrapy 的 scheduler调度器也可以使用该类
	    """
	 
	    logger = logger
	 
	    def __init__(self, server, key, debug=False):
	        """初始化过滤器
	        参数
	        ----------
	        server : redis.StrictRedis
	            Redis server 实例
	        key : str
	            保存Requests的指纹 到 Redis 哪个key
	        debug : bool, optional
	            是否在日志中记录被过滤的请求
	        """
	        self.server = server
	        self.key = key
	        self.debug = debug
	        self.logdupes = True
	 
	    @classmethod
	    def from_settings(cls, settings):
	        """根据给定的配置返回一个RFPDupeFilter实例
	        单独使用时，默认会使用 dupefilter:<timestamp> 作为key
	        当使用 scrapy_redis.scheduler.Scheduler 时，默认使用 %(spider)s:dupefilter 作为key
	        参数
	        ----------
	        settings : scrapy.settings.Settings
	        返回
	        -------
	        一个RFPDupeFilter实例
	        """
	        server = get_redis_from_settings(settings)
	        # 这个过滤器创建的key是一次性的，只能作为一个单独的过滤器和scrapy的默认scheduler 一起使用
	        # 如果scrapy 在open() 方法中传入了spider 作为参数，此处可不必设置key
	        # TODO: Use SCRAPY_JOB env as default and fallback to timestamp.
	        key = defaults.DUPEFILTER_KEY % {'timestamp': int(time.time())}
	        debug = settings.getbool('DUPEFILTER_DEBUG')
	        return cls(server, key=key, debug=debug)
	 
	    @classmethod
	    def from_crawler(cls, crawler):
	        """根据给定的crawler返回一个RFPDupeFilter实例
	        参数
	        ----------
	        crawler : scrapy.crawler.Crawler
	        返回
	        -------
	        一个RFPDupeFilter实例
	        """
	        return cls.from_settings(crawler.settings)
	 
	    def request_seen(self, request):
	        """判断一个Request请求之前是否见到过
	        参数
	        ----------
	        request : scrapy.http.Request
	        返回
	        -------
	        bool
	        """
	        fp = self.request_fingerprint(request) # 生成该请求的唯一指纹
	        added = self.server.sadd(self.key, fp) # 返回添加成功的值的数量，如果该值在集合中已经存在则返回 0
	        return added == 0
	 
	    def request_fingerprint(self, request):
	        """生成给定请求的唯一指纹
	        参数
	        ----------
	        request : scrapy.http.Request
	        返回
	        -------
	        str
	        """
	        return request_fingerprint(request)
	 
	    def close(self, reason=''):
	        """爬虫关闭时清除数据，供 Scrapy 的 scheduler 使用
	        Parameters
	        ----------
	        reason : str, optional
	        """
	        self.clear()
	 
	    def clear(self):
	        """清空指纹数据"""
	        self.server.delete(self.key)
	 
	    def log(self, request, spider):
	        """在日志中记录给定的请求
	        参数
	        ----------
	        request : scrapy.http.Request
	        spider : scrapy.spiders.Spider
	        """
	        if self.debug:
	            msg = "Filtered duplicate request: %(request)s"
	            self.logger.debug(msg, {'request': request}, extra={'spider': spider})
	        elif self.logdupes:
	            msg = ("Filtered duplicate request %(request)s"
	                   " - no more duplicates will be shown"
	                   " (see DUPEFILTER_DEBUG to show all duplicates)")
	            self.logger.debug(msg, {'request': request}, extra={'spider': spider})
	            self.logdupes = False

生成Request指纹的详细过程如下：

	_fingerprint_cache = weakref.WeakKeyDictionary()
	def request_fingerprint(request, include_headers=None):
	    """
	    生成 Request 的指纹
	    Request 的指纹是一个用来唯一识别这个Request所指向资源的哈希值，例如下面这两个URL：
	    http://www.example.com/query?id=111&cat=222
	    http://www.example.com/query?cat=222&id=111
	    尽管这两个URL 写法上有所区别，但它们指向的资源是相同的 (二者的响应内容是一样的),所以它们的指纹应当相同
	    对于那些只能由经过身份验证的用户访问的页面，大多数的网站都会使用cookie 来存储SESSION ID
	    这会在Request中增加一部分随机内容并且与请求的资源无关，因此cookie在计算指纹时应该被忽略
	    默认情况下，scrapy在计算指纹的时候也会忽略其他的请求头
	    如果你想要在计算指纹时包含上指定的请求头可以使用 include_headers 参数传入你想要包含的请求头列表
	    """
	    if include_headers:
	        include_headers = tuple(to_bytes(h.lower())
	                                 for h in sorted(include_headers))
	    cache = _fingerprint_cache.setdefault(request, {})
	    if include_headers not in cache:
	        fp = hashlib.sha1()
	        fp.update(to_bytes(request.method))
	        fp.update(to_bytes(canonicalize_url(request.url)))
	        fp.update(request.body or b'')
	        if include_headers:
	            for hdr in include_headers:
	                if hdr in request.headers:
	                    fp.update(hdr)
	                    for v in request.headers.getlist(hdr):
	                        fp.update(v)
	        cache[include_headers] = fp.hexdigest()
	    return cache[include_headers]

从Request指纹的生成过程不难看出，默认情况下，Request的指纹是对`request.method、request.url`和`request.body`经过SHA1加密后转换成的十六进制字符串。

# Scheduler
`scrapy_redis.scheduler.Scheduler`是一个基于Redis的scheduler调度器，借助于RedisPipeline和RFPDupeFilter可实现Request队列持久化和Request重复过滤等功能。

	import importlib
	import six
	 
	from scrapy.utils.misc import load_object
	 
	from . import connection, defaults
	 
	 
	# TODO: add SCRAPY_JOB support.
	class Scheduler(object):
	    """基于Redis的 scheduler调度器
	    相关配置
	    --------
	    SCHEDULER_PERSIST : bool (default: False)
	        是否要将Request队列持久化到 Redis
	    SCHEDULER_FLUSH_ON_START : bool (default: False)
	        在启动时是否要清空 Redis 队列
	    SCHEDULER_IDLE_BEFORE_CLOSE : int (default: 0)
	        如果没有收到消息，在关闭前等待多少秒
	    SCHEDULER_QUEUE_KEY : str
	        调度器要处理的Requests队列所对应的Redis key，默认 spider.name:requests
	    SCHEDULER_QUEUE_CLASS : str
	        调度器所使用的队列类，默认 scrapy_redis.queue.PriorityQueue
	    SCHEDULER_DUPEFILTER_KEY : str
	        调度器要已请求过的Requests集合所对应的Redis key，默认 spider.name:dupefilter
	    SCHEDULER_DUPEFILTER_CLASS : str
	        调度器所使用的过滤器类，默认 scrapy_redis.dupefilter.RFPDupeFilter
	    SCHEDULER_SERIALIZER : str
	        调度器的序列化器
	    """
	 
	    def __init__(self, server,
	                 persist=False,
	                 flush_on_start=False,
	                 queue_key=defaults.SCHEDULER_QUEUE_KEY,
	                 queue_cls=defaults.SCHEDULER_QUEUE_CLASS,
	                 dupefilter_key=defaults.SCHEDULER_DUPEFILTER_KEY,
	                 dupefilter_cls=defaults.SCHEDULER_DUPEFILTER_CLASS,
	                 idle_before_close=0,
	                 serializer=None):
	        """初始化调度器
	        参数
	        ----------
	        server : Redis
	            Redis server 实例
	        persist : bool
	            是否要将Request队列持久化到 Redis ，默认 False，即爬虫关闭时会清空Request队列
	        flush_on_start : bool
	            在启动时是否要清空 Redis 队列 ，默认 False
	        queue_key : str
	            调度器要处理的Requests队列所对应的Redis key，默认 spider.name:requests
	        queue_cls : str
	            调度器所使用的队列类，默认 scrapy_redis.queue.PriorityQueue
	        dupefilter_key : str
	            调度器要已请求过的Requests集合所对应的Redis key，默认 spider.name:dupefilter
	        dupefilter_cls : str
	            调度器所使用的过滤器类，默认 scrapy_redis.dupefilter.RFPDupeFilter
	        idle_before_close : int
	            如果没有收到消息，在关闭前等待多少秒
	        """
	        if idle_before_close < 0:
	            raise TypeError("idle_before_close cannot be negative")
	 
	        self.server = server
	        self.persist = persist
	        self.flush_on_start = flush_on_start
	        self.queue_key = queue_key
	        self.queue_cls = queue_cls
	        self.dupefilter_cls = dupefilter_cls
	        self.dupefilter_key = dupefilter_key
	        self.idle_before_close = idle_before_close
	        self.serializer = serializer
	        self.stats = None
	 
	    def __len__(self):  # 返回Requests队列的长度
	        return len(self.queue)
	 
	    @classmethod
	    def from_settings(cls, settings):  # 根据给定的配置返回一个Scheduler实例
	        kwargs = {
	            'persist': settings.getbool('SCHEDULER_PERSIST'),
	            'flush_on_start': settings.getbool('SCHEDULER_FLUSH_ON_START'),
	            'idle_before_close': settings.getint('SCHEDULER_IDLE_BEFORE_CLOSE'),
	        }
	 
	        # If these values are missing, it means we want to use the defaults.
	        optional = {
	            # TODO: Use custom prefixes for this settings to note that are
	            # specific to scrapy-redis.
	            'queue_key': 'SCHEDULER_QUEUE_KEY',
	            'queue_cls': 'SCHEDULER_QUEUE_CLASS',
	            'dupefilter_key': 'SCHEDULER_DUPEFILTER_KEY',
	            # We use the default setting name to keep compatibility.
	            'dupefilter_cls': 'DUPEFILTER_CLASS',
	            'serializer': 'SCHEDULER_SERIALIZER',
	        }
	        for name, setting_name in optional.items():
	            val = settings.get(setting_name)
	            if val:
	                kwargs[name] = val
	 
	        # Support serializer as a path to a module.
	        if isinstance(kwargs.get('serializer'), six.string_types):
	            kwargs['serializer'] = importlib.import_module(kwargs['serializer'])
	 
	        server = connection.from_settings(settings)
	        # Ensure the connection is working.
	        server.ping()
	 
	        return cls(server=server, **kwargs)
	 
	    @classmethod
	    def from_crawler(cls, crawler):  # 根据给定的crawler返回一个Scheduler实例
	        instance = cls.from_settings(crawler.settings)
	        # FIXME: for now, stats are only supported from this constructor
	        instance.stats = crawler.stats
	        return instance
	 
	    def open(self, spider):  # Scheduler 开启
	        self.spider = spider
	 
	        try:
	            self.queue = load_object(self.queue_cls)(
	                server=self.server,
	                spider=spider,
	                key=self.queue_key % {'spider': spider.name},
	                serializer=self.serializer,
	            )
	        except TypeError as e:
	            raise ValueError("Failed to instantiate queue class '%s': %s",
	                             self.queue_cls, e)
	 
	        try:
	            self.df = load_object(self.dupefilter_cls)(
	                server=self.server,
	                key=self.dupefilter_key % {'spider': spider.name},
	                debug=spider.settings.getbool('DUPEFILTER_DEBUG'),
	            )
	        except TypeError as e:
	            raise ValueError("Failed to instantiate dupefilter class '%s': %s",
	                             self.dupefilter_cls, e)
	 
	        if self.flush_on_start:
	            self.flush()
	        # notice if there are requests already in the queue to resume the crawl
	        if len(self.queue):
	            spider.log("Resuming crawl (%d requests scheduled)" % len(self.queue))
	 
	    def close(self, reason):  # Scheduler 关闭
	        """
	        若未开启持久化，则清空数据
	        """
	        if not self.persist:
	            self.flush()
	 
	    def flush(self):  # 清空数据
	        self.df.clear()  # 清空已请求过的Requests集合
	        self.queue.clear()  # 清空Requests队列
	 
	    def enqueue_request(self, request):  # 将Request添加到队列
	        """
	        若开启过滤，并且之前见过这个Request，返回False
	        否则，将Request添加到队列
	        """
	        if not request.dont_filter and self.df.request_seen(request):
	            self.df.log(request, self.spider)
	            return False
	        if self.stats:
	            self.stats.inc_value('scheduler/enqueued/redis', spider=self.spider)
	        self.queue.push(request)
	        return True
	 
	    def next_request(self):  # 获取下一个要处理的请求
	        block_pop_timeout = self.idle_before_close
	        request = self.queue.pop(block_pop_timeout)
	        if request and self.stats:
	            self.stats.inc_value('scheduler/dequeued/redis', spider=self.spider)
	        return request
	 
	    def has_pending_requests(self):  # 是否还有待处理的请求
	        return len(self) > 0

# PriorityQueue
`scrapy_redis.queue.PriorityQueue`是一个优先级队列,优先级越高越先处理，其内部是通过Redis有序集合进行实现的。

	class PriorityQueue(Base):
	    """Per-spider priority queue abstraction using redis' sorted set"""
	 
	    def __len__(self):
	        """Return the length of the queue"""
	        return self.server.zcard(self.key)
	 
	    def push(self, request):
	        """Push a request"""
	        data = self._encode_request(request)
	        score = -request.priority
	        # We don't use zadd method as the order of arguments change depending on
	        # whether the class is Redis or StrictRedis, and the option of using
	        # kwargs only accepts strings, not bytes.
	        self.server.execute_command('ZADD', self.key, score, data)
	 
	    def pop(self, timeout=0):
	        """
	        Pop a request
	        timeout not support in this queue class
	        """
	        # use atomic range/remove using multi/exec
	        pipe = self.server.pipeline()
	        pipe.multi()
	        pipe.zrange(self.key, 0, 0).zremrangebyrank(self.key, 0, 0)
	        results, count = pipe.execute()
	        if results:
	            return self._decode_request(results[0])

此外，Scrapy-Redis还提供了FifoQueue先进先出和LifoQueue后进先出两个队列。

	class FifoQueue(Base):
	    """先进先出队列"""
	 
	    def __len__(self):
	        """返回队列的长度"""
	        return self.server.llen(self.key)
	 
	    def push(self, request):
	        """压入一个请求"""
	        self.server.lpush(self.key, self._encode_request(request))
	 
	    def pop(self, timeout=0):
	        """弹出一个请求"""
	        if timeout > 0:
	            data = self.server.brpop(self.key, timeout)
	            if isinstance(data, tuple):
	                data = data[1]
	        else:
	            data = self.server.rpop(self.key)
	        if data:
	            return self._decode_request(data)
	 
	 
	 
	class LifoQueue(Base):
	    """后进先出队列"""
	 
	    def __len__(self):
	        """返回栈的长度"""
	        return self.server.llen(self.key)
	 
	    def push(self, request):
	        """压入一个请求"""
	        self.server.lpush(self.key, self._encode_request(request))
	 
	    def pop(self, timeout=0):
	        """弹出一个请求"""
	        if timeout > 0:
	            data = self.server.blpop(self.key, timeout)
	            if isinstance(data, tuple):
	                data = data[1]
	        else:
	            data = self.server.lpop(self.key)
	 
	        if data:
	            return self._decode_request(data)
	 