|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|auto_increment_increment|否|1|1～65,535|auto_increment_increment和auto_increment_offset 用于master-to-master的复制，并可以用来控制AUTO_INCREMENT列的操作。|
|auto_increment_offset|否|1|1～65,535|auto_increment_increment和auto_increment_offse用于master-to-master的复制，并可以用来控制AUTO_INCREMENT列的操作。|
|autocommit|否|ON ON, OFF|自动提交模式。ON：所有的更改表立即生效；OFF：必须使用COMMIT提交业务或ROLLBACK取消它。|
|automatic_sp_privileges|否|ON ON, OFF|当此变量为ON（默认值）时，如果存储例程的创建者无法执行、修改或删除该例程，服务器自动为该创建者赋予EXECUTE和ALTER ROUTINE权限。|
|back_log|是 default|1～100,000|MySQL能拥有的有效连接请求数，MySQL主线程在很短时间内收到大量连接请求时发挥生效。然后主线程花很短的一些时间检查连接并且启动一个新线程。该back_log值表示在MySQL暂时停止回答新要求之前的很短时间内，有多少请求可以堆叠。默认值大小根据系统配置决定。|
|basedir|是 /usr/|--|基础MySQL安装路径。|
|binlog_cache_size|否|32768|4,096～18,446,744,073,709,547,520|在事务中，为二进制日志存储SQL语句的缓存容量。该参数必须设置为2的幂次方。|
|binlog_checksum|否|CRC32 NONE, CRC32|启动变量时，引起主服务器在二进制日志中写入的每个事件的校验和。|
|binlog_error_action|否|ABORT_SERVER IGNORE_ERROR, ABORT_SERVER|控制服务器无法写入二进制日志时的响应情况。|
|binlog_format|否|ROWSTATEMENT, ROW, MIXED|行复制或混合复制。|
|binlog_group_commit_sync_delay|否|0|0～1,000,000|控制在将二进制日志文件同步到磁盘之前，二进制日志提交等待的微秒数。|
|binlog_group_commit_sync_no_delay_count|否|0|0～100,000|指定在中止由binlog-group-commit-sync-delay指定的当前延迟之前要等待的最大事务数。|
|binlog_gtid_simple_recovery|是 ON ON, OFF|控制在MySQL启动或者重启时，搜索GTID期间，二进制日志文件是如何迭代的。|
|binlog_order_commits|否|ON ON, OFF|如果开启此变量（默认），按照事务被写入二进制日志的顺序提交事务。变量如果没有开启，事务以并行顺序提交。|
|binlog_row_image|否|FULL FULL, MINIMAL, NOBLOB|指定服务器日志在行级复制时对所有行还是最少行进行日志记录。|
|binlog_rows_query_log_events|否|OFF ON, OFF|参数一旦启动，MySQL 5.6.2或以上版本服务器将信息写入日志事件，比如，将行日志查询写入其二进制日志。|
|binlog_stmt_cache_size|否|32768|4,096～18,446,744,073,709,547,520|此变量决定在事务中，二进制日志存储下发的非事务语句的缓存空间。该参数必须设置为2的幂次方。|
|block_encryption_mode|否|aes-128-cbc aes-128-ecb, aes-192-ecb, aes-256-ecb, aes-128-cbc, aes-192-cbc, aes-256-cbc|控制基于块算法(如AES)的块加密模式。它影响AES_ENCRYPT() and AES_DECRYPT()的加密。|
|bulk_insert_buffer_size|否|8388608|0～18,446,744,073,709,551,615|限制MyISAM缓存树每个线程的大小，单位为字节。|
|character_set_client|否|utf8 gb2312, greek, cp1250, gbk, latin5, armscii8, utf8, ......|用于来自客户端的语句的字符集。|
|character_set_connection|否|utf8  latin1, latin2, swe7, ascii, ujis, sjis, hebrew, gb2312,  utf8,.......|用于未指定introducer的文字串，或数字到字符转换的字符集。|
|character_set_database|否|utf8 gb2312, greek, cp1250, gbk, latin5, armscii8, utf8,..........|默认数据库使用的字符集。|
|character_set_filesystem|否|binary , ascii, ujis, sjis, hebrew, tis620, euckr, koi8u, gb2312, utf8, .......|文件系统字符集。|
|character_set_results|否|utf8  gb2312, greek, cp1250, gbk, latin5, armscii8, utf8, ......|用于返回查询结果到客户端的字符集。|
|character_set_server|否|utf8 utf8, latin1, gbk, utf8mb4|服务器字符集。|
|check_proxy_users|否|default ON, OFF|控制服务器是否对请求它的验证插件执行代理用户映射。|
|collation_connection|否|utf8_general_ci utf8_general_ci, utf8_bin, utf8_unicode_ci, utf8_icelandic_ci, utf8_latvian_ci, utf8_romanian_ci, .......|连接字符集的排序。|
|collation_server|否|utf8_general_ci utf8_general_ci, utf8_bin, utf8_unicode_ci, utf8_icelandic_ci, utf8_latvian_ci, utf8_romanian_ci, .......|服务器默认排序。|
|completion_type|否|NO_CHAIN NO_CHAIN, CHAIN, RELEASE|事务完成类型（0-默认，1-链型，2-释放）。|
|concurrent_insert|否|AUTO NEVER, AUTO, ALWAYS|该系统变量用于修改并发插入处理。如果设置为默认值AUTO，对于数据文件中间没有空闲空间的MyISAM表，MySQL允许INSERT和SELECT语句并发运行。如果设置为NEVER，则禁用并发插入。如果设置为ALWAYS，即使对于已删除行的表，也允许在表末尾进行并发插入。|
|connect_timeout|否|10|2～31,536,000|mysqld服务器在回Bad handshake响应之前等待连接数据包的时间（秒）。|
|core_file|是 OFF ON, OFF|mysqld崩溃后生成一个core文件。|
|datadir|是 /var/lib/mysql/data|--|MySQL数据目录。|
|default_authentication_plugin|是 mysql_native_password mysql_native_password, sha256_password|表示默认验证插件。|
|default_password_lifetime|否|0|0～65,535|定义了全局自动密码过期策略。|
|default_storage_engine|否|InnoDB InnoDB, MRG_MYISAM, MyISAM, BLACKHOLE, CSV, mem, ARCHIVE, FEDERATED|默认的存储引擎（表类型）。|
|default_tmp_storage_engine|否|InnoDB InnoDB, MRG_MYISAM, MyISAM, BLACKHOLE, CSV, mem, ARCHIVE, FEDERATED|TEMPORARY表格默认的存储引擎。|
|default_week_format|否|0|0～7|被week()函数使用的默认周格式。|
|delay_key_write|否|ON ON, OFF, ALL|该参数只对MyISAM类型数据表有效，有如下的取值种类：OFF：全部忽略DELAY_KEY_WRITE。ON：如果CREATE TABLE在建表语句中使用DELAY_KEY_WRITE，则使用该选项。此为默认值。ALL：所有打开的数据表都将按照DELAY_KEY_WRITE开启处理。|
|disabled_storage_engines|是 default|--|表示哪些存储引擎不能用于创建表或表空间。|
|disconnect_on_expired_password|是 ON ON, OFF|控制服务器如何处理具有过期密码的客户端。|
|div_precision_increment|否|4|0～30|除法结果的精度位数。|
|end_markers_in_json|否|OFF ON, OFF|指定优化程序JSON输出是否增加结束符。|
|enforce_gtid_consistency|是 ON ON, OFF|当此变量为true时，仅允许执行以事务安全的方式进行日志记录的语句。|
|eq_range_index_dive_limit|否|200|0～4,294,967,295|条件个数超过该参数值时，优化程序从使用index dive改为使用index statistics。|
|event_scheduler|否|OFF ON, OFF|Event Scheduler的状态。|
|expire_logs_days|否|1|0～99|用于设置自动删除二进制日志文件的天数。|
|explicit_defaults_for_timestamp|否|OFF ON, OFF|--|
|flush|否|OFF ON, OFF|如果该参数为ON，服务器在执行每个SQL语句后将所有变更持久化到硬盘。|
|flush_time|否|0|0～31,536,000|释放资源，将未持久化的数据同步到磁盘。仅推荐在系统资源很少时使用。|
|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|ft_boolean_syntax|否|+ -&gt;&lt;()~*:""&amp;||--|Boolean全文本搜索支持的操作符列表。|
|ft_max_word_len|是 80|10～84|FULLTEXT索引包含的最大字母长度。|
|ft_min_word_len|是 4|1～84|包含在全文索引中的词的最小长度。注意：在改变这个值后全文索引必须被重建。|
|ft_query_expansion_limit|是 20|0～1,000|用WITH QUERY EXPANSION进行全文搜索的最高匹配数。|
|ft_stopword_file|是 default|--|全文搜索时使用的停止词文件。该参数值为NULL时，使用默认停止词；该值为/dev/null时，禁用停止词。|
|general_log|否|OFF ON, OFF|通用的查询日志是否开启。|
|general_log_file|否|/var/lib/mysql/data/hostname.log|--|mysql通用日志的位置。|
|group_concat_max_len|否|1024|4～18,446,744,073,709,551,615|函数GROUP_CONCAT()结果的最大长度。|
|gtid_executed_compression_period|否|1000|0～4,294,967,295|表示每执行多少个事务，对mysql.gtid_executed表进行压缩。|
|gtid_mode|是 ON ON, OFF|是否开启GTIDs。|
|host_cache_size|否|440|0～65,536|内部主机缓存大小。|
|init_connect|否|default|--|服务器对每一个连接的客户端执行的字符串。|
|innodb_adaptive_flushing|否|ON ON, OFF|开启InnoDB Adaptive Flushing（对RDS默认值为on）。|
|innodb_adaptive_flushing_lwm|否|10|0～70|低水位标识，代表开启自适应刷新时redo log的容量。|
|innodb_adaptive_hash_index|否|ON ON, OFF|Innodb自适应哈希索引是否开启或关闭。|
|innodb_adaptive_hash_index_parts|是 8|1～512|对自适应哈希索引搜索系统进行分区。|
|innodb_adaptive_max_sleep_delay|否|150000|0～1,000,000|允许InnoDB根据当下工作量自动调整innodb_thread_sleep_delay值。|
|innodb_autoextend_increment|否|64|1～1,000|当表空间已满时，自动扩展的表空间文件的递增空间容量（MB）。|
|innodb_autoinc_lock_mode|是 1 0, 1, 2|产生自动递增值的锁定模式：0：旧的模式。1：新的模式。2：无锁定。|
|innodb_buffer_pool_chunk_size|是 134217728|1,048,576～134,217,728|定义调整InnoDB缓冲池大小操作的块的大小。|
|innodb_buffer_pool_dump_at_shutdown|否|ON ON, OFF|当MySQL服务器关闭后，是否记录InnoDB缓冲池内的缓存页。|
|innodb_buffer_pool_dump_now|否|OFF ON, OFF|即时记录InnoDB缓冲池内缓存的页。|
|innodb_buffer_pool_dump_pct|否|25|1～100|表示每个缓冲池最近使用的页数与读取和转储的百分比。|
|innodb_buffer_pool_filename|否|ib_buffer_pool|--|innodb_buffer_pool_dump_at_shutdown或innodb_buffer_pool_dump_now产生的包含页码列表的文件。|
|innodb_buffer_pool_instances|是 8|1～64|InnoDB缓冲池划分的区域总数。如果设定值为default，表示该参数随内存规格变化。|
|innodb_buffer_pool_load_abort|否|OFF ON, OFF|中断由innodb_buffer_pool_load_at_startup或innodb_buffer_pool_load_now触发的InnoDB缓冲池内容恢复进程。|
|innodb_buffer_pool_load_at_startup|是 ON ON, OFF|MySQL服务器启动时，InnoDB缓冲池通过加载前期相同的页自动预热。|
|innodb_buffer_pool_load_now|否|OFF ON, OFF|不等待服务器重启，加载一组数据页，且从而即时预热InnoDB缓冲池。|
|innodb_buffer_pool_size|否|default|536,870,912～18,446,744,073,709,551,615|对于缓存数据及其表格索引，innodb使用的内存缓存字节大小。如果设定值为default，表示该参数随内存规格变化。|
|innodb_change_buffer_max_size|否|25|0～50|InnoDB更改缓存的最大容量，占据整个缓冲池的一个百分比。|
|innodb_change_buffering|否|all inserts, deletes, changes, purges, all, none|控制InnoDB更改缓冲。|
|innodb_checksum_algorithm|否|CRC32 crc32, innodb, none|如何产生并验证存储在每个InnoDB表空间内磁盘块的校验和。|
|innodb_cmp_per_index_enabled|否|OFF ON, OFF|开启在INFORMATION_SCHEMA.INNODB_CMP_PER_INDEX表格内每个指数相关压缩的统计。|
|innodb_commit_concurrency|否|0|0～1,000|同时可以提交的线程数。|
|innodb_compression_failure_threshold_pct|否|5|0～100|设置MySQL在压缩页码内开始增加填充的截止点，避免高成本压缩失败。|
|innodb_compression_level|否|6|0～9|设置MySQL在压缩页码内开始增加填充的截止点，避免高成本压缩失败。|
|innodb_compression_pad_pct_max|否|50|0～75|每张压缩页内可预留为空闲空间的最大容量百分比。当压缩表格或索引更新且数据可能被压缩时，允许空间整理该页内数据和更改的日志。|
|innodb_concurrency_tickets|否|5000|1～4,294,967,295|决定能并发进入InnoDB的线程数。当一个线程尝试连接InnoDB，但是已经达到最大并发连接数时，该线程进入队列等待。如果请求被InnoDB接受，则会获得一个次数为innodb_concurrency_tickets的通行证，在次数用完之前，该线程重新请求时无须再进行innodb_thread_concurrency的检查。|
|innodb_data_home_dir|是 default|--|InnoDB文件的存放目录。|
|innodb_default_row_format|否|DYNAMIC DYNAMIC, COMPACT, REDUNDANT|定义InnoDB表和用户创建的临时表的默认行格式。|
|innodb_disable_sort_file_cache|否|OFF ON, OFF|对合并排序临时文件禁用操作系统文件系统缓存。|
|innodb_doublewrite|是 ON ON, OFF|如果参数设置为开启（默认为开启），InnoDB会存储两次数据，第一次存储在double write buffer缓冲池中，第二次存储在实际数据文件中。|
|innodb_fast_shutdown|否|1 0, 1, 2|InnoDB的关闭模式。|
|innodb_file_per_table|否|ON ON, OFF|指定Innodb使用表空间或文件。|
|innodb_fill_factor|否|100|10～100|定义在排序索引构建期间填充的每个B-tree页面上的空间百分比，剩余空间保留用于将来的索引增长。|
|innodb_flush_log_at_timeout|否|1|1～2,700|每N秒写入并刷新日志。当innodb_flush_log_at_trx_commit值为2时，此设置有效。|
|innodb_flush_log_at_trx_commit|否|1 0, 1, 2|当重新安排并批量处理与提交相关的I/O操作时，可以控制提交操作在严格遵守ACID合规性和高性能之间的平衡。当值设为0时，每秒把事务日志缓存区的数据写入日志文件并刷新到磁盘；当设为默认值1时，是为了保证完整的ACID，每次提交事务时，把事务日志从缓存区写到日志文件中，并刷新日志文件的数据到磁盘上；如果设为2，每次提交事务都会把事务日志从缓存区写入日志文件，大约每隔一秒会刷新到磁盘。|
|innodb_flush_method|是 O_DIRECT fsync, O_DSYNC, littlesync, nosync, O_DIRECT, O_DIRECT_NO_FSYNC|Innodb的持久化方法。|
|innodb_flush_neighbors|否|1 0, 1, 2|是否刷新InnoDB缓冲池页，同等程度，刷新其他脏页。如果设定值为default，表示该参数随磁盘IO类型变化。|
|innodb_flush_sync|否|ON ON, OFF|默认开启，表示在checkpoint突发I/O活动时忽略innodb_io_capacity的设置。|
|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|innodb_flushing_avg_loops|否|30|1～1,000|InnoDB保持先前刷新状态下统计的快照迭代总数，控制自适应刷新对更改的工作量的反应速度。|
|innodb_force_load_corrupted|是 OFF ON, OFF|InnoDB加载启动时有损坏标记的表格。|
|innodb_ft_aux_table|否|default|--|标记包含FULLTEXT索引的InnoDB表格的限定名。|
|innodb_ft_cache_size|是 8000000|1,600,000～80,000,000|创建InnoDB FULLTEXT索引时内存存储解析文件的缓存容量。|
|innodb_ft_enable_diag_print|否|OFF ON, OFF|控制是否启用其他全文搜索（FTS）诊断输出。|
|innodb_ft_enable_stopword|否|ON ON, OFF|关联InnoDB FULLTEXT索引和一组stopwords，同时创建该索引。|
|innodb_ft_max_token_size|是 84|10～84|InnoDB FULLTEXT索引存储的单词的最大长度。|
|innodb_ft_min_token_size|是 3|0～16|InnoDB FULLTEXT索引存储的单词的最小长度。|
|innodb_ft_num_word_optimize|否|2000|1,000～10,000|在InnoDB FULLTEXT索引中，每一个OPTIMIZE TABLE操作进程需要处理的单词总数。|
|innodb_ft_result_cache_limit|否|2000000000|1,000,000～4,294,967,295|每一个FTS查询或每个线程，InnoDBFULLTEXT搜索（FTS）的查询结果缓存限值（单位为字节）。|
|innodb_ft_server_stopword_table|否|default|--|创建InnoDB FULLTEXT索引进程中，包含可以忽略单词列表的表格名，格式为db_name/table_name。|
|innodb_ft_sort_pll_degree|是 2|1～32|创建大表格搜索索引进程中，用于InnoDB FULLTEXT索引和tokenize文本的平行线程的总数。|
|innodb_ft_total_cache_size|是 640000000|32,000,000～1,600,000,000|指定为所有表的InnoDB全文搜索索引缓存分配的总内存（以字节为单位）。|
|innodb_ft_user_stopword_table|否|default|--|创建InnoDB FULLTEXT索引进程中，包含的一系列单词的表格名，格式为db_name/table_name。|
|innodb_io_capacity|否|200|100～18,446,744,073,709,551,615|每秒允许InnoDB进行I/O操作的最大数。如果设定值为default，表示该参数随磁盘IO类型变化。|
|innodb_io_capacity_max|否|2000|200～18,446,744,073,709,551,615|为应对紧急情况，允许InnoDB扩展innodb_io_capacity设置的限值。如果设定值为default，表示该参数随磁盘IO类型变化。|
|innodb_lock_wait_timeout|否|50|1～1,073,741,824|放弃事务前，InnoDB事务等待行锁的时间。|
|innodb_log_buffer_size|是 default|262,144～4,294,967,295|InnoDB将日志写入日志磁盘文件前的缓冲大小。如果设定值为default，表示该参数随内存规格变化。|
|innodb_log_checksums|否|ON ON, OFF|启用或禁用redo log页的校验和。|
|innodb_log_compressed_pages|否|ON ON, OFF|是否重新压缩存储在InnoDB redo log页内的镜像 。|
|innodb_log_file_size|是 134217728|4,194,304～274,877,906,944|日志组内每个日志文件的字节大小。|
|innodb_log_files_in_group|是 default|2～100|日志组中的日志文件数目。InnoDB以环型方式（circular fashion）写入文件。默认值为2，这些文件的路径是通过innodb_log_group_home_dir来指定的，日志文件的大小（innodb_log_file_size * innodb_log_files_in_group）可达到512 GB。如果设定值为default，表示该参数随内存规格变化。|
|innodb_log_group_home_dir|是 ./|--|innodb日志文件的路径|
|innodb_log_write_ahead_size|否|8192|512～16,384|指定redo log写之前的块大小（以字节为单位）。 innodb_log_write_ahead_size的有效值是InnoDB日志文件块大小（2 ^ n）的倍数。|
|innodb_lru_scan_depth|否|1024|100～18,446,744,073,709,551,615|影响InnoDB缓冲池刷新操作的算法和启发式方法的参数。|
|innodb_max_dirty_pages_pct|否|75|0～99.99|InnoDB尝试从缓冲池刷新数据，以便脏页的百分比不超过该参数值。|
|innodb_max_dirty_pages_pct_lwm|否|0.000000|0～99.99|定义低水位标记，表示启用预冲的脏页的百分比，以控制脏页率。|
|innodb_max_purge_lag|否|0|0～4,294,967,295|清除操作延迟时，控制如何延迟插入、更新和删除操作。|
|innodb_max_purge_lag_delay|否|0|0～18,446,744,073,709,551,615|innodb_max_purge_lag配置选项造成的最大延时（毫秒）。|
|innodb_max_undo_log_size|否|1073741824|10,485,760～18,446,744,073,709,551,615|定义撤销表空间的阈值大小。|
|innodb_monitor_disable|是 default all, adaptive_hash_pages_added, adaptive_hash_pages_removed, ......|关闭information_schema.innodb_metrics表格中一个或多个计数器。|
|innodb_monitor_enable|是 default all, adaptive_hash_pages_added, adaptive_hash_pages_removed, ........|打开information_schema.innodb_metrics表格中一个或多个计数器。|
|innodb_monitor_reset|是 default all, buffer_data_reads, buffer_data_written, buffer_flush_adaptive, ..................|将information_schema.innodb_metrics表格内一个或多个计数器的计数值重置为零。|
|innodb_monitor_reset_all|是 default trx_undo_slots_cached, trx_undo_slots_used......|重置information_schema.innodb_metrics表格内一个或多个计数器的所有值（最小值、最大值和其他值）。|
|innodb_old_blocks_pct|否|37|5～95|指定InnoDB缓冲池用于旧块子列表的近似百分比。|
|innodb_old_blocks_time|否|1000|0～4,294,967,295|非零值表示在指定短暂时期内保护将被填满的引用数据。|
|innodb_online_alter_log_max_size|否|134217728|65,536～18,446,744,073,709,551,615|InnoDB表格DDL在线操作进程中，临时日志文件空间的上限值。|
|innodb_open_files|是 2000|10～500,000|InnoDB数据表驱动程序最多可以同时打开的文件数，默认值大小根据系统配置决定。|
|innodb_optimize_fulltext_only|否|OFF ON, OFF|更改InnoDB表格内操作OPTIMIZE TABLE的语句方式。|
|innodb_page_cleaners|是 4|1～64|指定从缓冲池实例刷新脏页的页面清除程序线程数。如果设定值为default，表示该参数随内存规格变化。|
|innodb_page_size|是 16384 4096, 8192, 16384|MySQL实例内InnoDB表空间的页大小。|
|innodb_print_all_deadlocks|否|OFF ON, OFF|启用此选项时，有关InnoDB用户事务中所有死锁信息都记录在mysqld错误日志中。|
|innodb_purge_batch_size|否|300|1～5,000|表示一次完成多少个undolog page，该参数和innodb_purge_threads=n组合调优，普通用户不需要修改它。|
|innodb_purge_rseg_truncate_frequency|否|128|1～128|定义purge系统释放回滚段的频率。|
|innodb_purge_threads|是 4|1～32|InnoDB预留操作的后台线程的总数。|
|innodb_random_read_ahead|否|OFF ON, OFF|启动或关闭Innodb Random Read Ahead。|
|innodb_read_ahead_threshold|否|56|0～64|线性预读取，它控制一个区中多少页被顺序访问时，InnoDB才启用预读取，预读取下一个页中所有的页。|
|innodb_read_io_threads|是 4|1～64|用于从磁盘读文件块的线程数。|
|innodb_read_only|是 OFF ON, OFF|启动服务器的只读模式。|
|innodb_replication_delay|否|0|0～4,294,967,295|在innodb_thread_concurrency达到的情况下，从服务器上复制线程的延时时间（毫秒）。|
|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|innodb_rollback_on_timeout|是 OFF ON, OFF|innodb_rollback_on_timeout确定后，事务超时后InnoDB回滚完整的事务。|
|innodb_rollback_segments|否|128|1～128|在一个事务中，InnoDB使用的系统表空间中回滚段的个数。|
|innodb_sort_buffer_size|是 1048576|65,536～67,108,864|创建InnoDB索引过程中，数据排序使用的缓冲空间。|
|innodb_spin_wait_delay|否|6|0～18,446,744,073,709,551,615|自旋锁轮询间隔的最大延时。|
|innodb_stats_auto_recalc|否|ON ON, OFF|表格中数据大规模变更后，InnoDB自动重新计算持久统计数。|
|innodb_stats_method|否|nulls_equal nulls_equal, nulls_unequal, nulls_ignored|为InnoDB表收集关于索引值分布的统计时服务器如何处理NULL值：nulls_equal，nulls_unequal和nulls_ignored。对于nulls_equal，所有NULL索引值被认为是相等的，并形成一个单一的大小等于NULL值的数目的值组。对于nulls_unequal，NULL值被认为是不相等的，每个NULL形成一个大小为1的不同值组。对于nulls_ignored，NULL值被忽略。|
|innodb_stats_on_metadata|否|OFF ON, OFF|此变量启用时，当元数据语言如SHOW TABLE STATUS或SHOW INDEX运行中或接入INFORMATION_SCHEMA. TABLES或INFORMATION_SCHEMA. STATISTICS，InnoDB更新统计。|
|innodb_stats_persistent|否|ON ON, OFF|InnoDB索引统计数是否由磁盘内存储的ANALYZE TABLE命令产生。|
|innodb_stats_persistent_sample_pages|否|20|1～18,446,744,073,709,551,615|采样的索引列的数量和其他统计数进程中的采样索引页的总数，比如ANALYZE TABLE统计的索引页。|
|innodb_stats_transient_sample_pages|否|8|1～18,446,744,073,709,551,615|采样的索引列的数量和其他统计数进程中的采样索引页的总数，比如ANALYZE TABLE统计的索引页。|
|innodb_status_output|否|OFF ON, OFF|启用或禁用标准InnoDB监视器的周期性输出。|
|innodb_status_output_locks|否|OFF ON, OFF|启用或禁用InnoDB锁定监视器。|
|innodb_strict_mode|否|ON ON, OFF|InnoDB严格检查模式，尤其采用了页数据压缩功能后，最好是开启该功能。开启此功能后，当创建表（CREATE TABLE）、更改表（ALTER TABLE）和创建索引（CREATE INDEX）语句时，如果写法有错误，不会有警告信息，而是直接抛出错误。|
|innodb_support_xa|否|ON ON, OFF|XA事务进程中启动的两阶段提交。|
|innodb_sync_array_size|是 1|1～1,024|分解用于协同线程的内部数据结构，提高大规模等待线程工作量的同步率。|
|innodb_sync_spin_loops|否|30|0～4,294,967,295|线程暂停前，等待即将释放的innoDB互斥（mutex）锁的线程总数。|
|innodb_table_locks|否|ON ON, OFF|autocommit = 0时，LOCK TABLES使InnoDB内部表锁。|
|innodb_thread_concurrency|否|0|0～1,000|InnoDB驱动程序能够同时使用的最大线程个数。|
|innodb_thread_sleep_delay|否|10000|0～1,000,000|连接InnoDB队列之前InnoDB线程的睡眠时间（微秒）。|
|innodb_undo_directory|是 ./|--|InnoDB为undo日志创建表空间的相对或绝对路径。|
|innodb_undo_log_truncate|否|OFF ON, OFF|当启用innodb_undo_log_truncate时，超过innodb_max_undo_log_size定义的阈值的undo表空间被标记为截断。|
|innodb_undo_logs|否|128|1～128|事务进程中，InnoDB使用的系统表空间的回滚段个数。|
|innodb_undo_tablespaces|是 0|0～126|使用非零innodb_undo_logs设置时，undo日志被分割的表空间文件总数。|
|innodb_use_native_aio|是 ON ON, OFF|控制MySQL是否使用Linux原生异步I/O。|
|innodb_write_io_threads|是 4|1～64|用于写脏页的线程数。|
|interactive_timeout|否|28800|1～31,536,000|服务器在关闭交互式连接之前等待活动的秒数。|
|internal_tmp_disk_storage_engine|否|InnoDB MYISAM, INNODB|指定磁盘内部临时表的存储引擎。|
|join_buffer_size|否|262144|128～18,446,744,073,709,547,520|在无法增加索引的情况下，增加join_buffer_size值实现更快的完全联接。|
|keep_files_on_create|否|OFF ON, OFF|禁止覆盖在DATA DIRECTORY或INDEX DIRECTORY中创建的MyISAM文件。|
|key_buffer_size|否|16777216|8～9,223,372,036,854,771,712|增加缓冲池空间，便于处理用于索引块的索引(针对所有读和多写)。|
|key_cache_age_threshold|否|300|100～18,446,744,073,709,551,600|该参数控制是否将缓存区从索引缓存的hot sublist中降级到warm list中。参数值越低，降级发生越快，最小可设为100。|
|key_cache_block_size|否|1024|512～16,384|指定索引缓冲区的大小（字节）。|
|key_cache_division_limit|否|100|1～100|索引缓冲区列表中hot sublist和warm sublist的分界点。该值用于warm sublist的缓冲区列表的百分比。|
|lc_time_names|否|en_US de_CH, de_DE, de_LU, el_GR, en_AU, en_CA, en_GB, en_IN, en_NZ, en_PH, en_US, en_ZA, en_ZW, es_AR, es_BO, es_CL, es_CO,zh_CN, zh_HK, zh_TW,...............|设定基于语言区域来显示日、月及其简写方式。|
|local_infile|否|OFF ON, OFF|控制LOCAL是否支持LOAD DATA INFILE。|
|lock_wait_timeout|否|31536000|1～31,536,000|试图获得元数据锁的超时时间（秒）。|
|log-bin|是 /var/lib/mysql/data/mysql-bin|--|控制二进制日志。开启binlog日志|
|log_bin_trust_function_creators|否|ON ON, OFF|强制限制存储功能/用以实现复制的触发器登录。|
|log_bin_use_v1_row_events|否|OFF ON, OFF|MySQL是否使用版本1或版本2日志记录事件写入二进制日志事件。|
|log_builtin_as_identified_by_password|否|OFF ON, OFF|影响用户管理语句的二进制日志记录。|
|log_error|是 /var/lib/mysql/data/error.log|--|错误日志位置。|
|log_error_verbosity|否|3|1～3|控制服务器把错误，警告和说明信息写入错误日志的详细程度。|
|log_output|否|FILE TABLE, FILE|控制存储查询日志的位置。|
|log_queries_not_using_indexes|否|OFF ON, OFF|是否将不适用索引的查询记录到慢查询日志中。|
|log_slave_updates|是 true|true/false|无论从服务器收到来自主服务器的更新是否记入从服务器的二进制日志，为了使改参数生效，必须启动从服务器的二进制日志。|
|log_slow_admin_statements|否|OFF ON, OFF|包含写入慢查询日志的慢执行语句。|
|log_slow_slave_statements|否|OFF ON, OFF|启动慢查询日志时，该变量需要比long_query_time设置的时长（秒）更长的时间在备机启动查询日志。|
|log_statements_unsafe_for_binlog|否|ON ON, OFF|如果遇到错误1592，控制是否将生成的警告添加到错误日志中。|
|log_syslog|否|OFF ON, OFF|控制是否将错误日志输出写入syslog（Unix和类Unix系统）或事件日志（Windows系统）。|
|log_syslog_facility|否|daemon|--|表示用于将错误日志输出写入syslog的设备（发送消息的程序类型）。|
|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|log_syslog_include_pid|否|ON ON, OFF|控制是否在写入syslog的错误日志输出的每行中都包含服务器进程ID。|
|log_syslog_tag|否|default|--|表示写入syslog的错误日志输出中的服务器标识符中待添加的标记。|
|log_throttle_queries_not_using_indexes|否|0|0～4,294,967,295|限制每分钟可以写入慢查询日志的查询总数。|
|log_timestamps|否|UTC UTC, SYSTEM|控制错误日志消息的时间戳时区，以及写入文件的一般查询日志消息和慢查询日志消息的时间戳时区。它不影响写入表（mysql.general_log和mysql.slow_log）的一般查询日志消息和慢查询日志消息的时区。|
|long_query_time|否|10|0～3,600|是否记录慢速查询。|
|low_priority_updates|否|OFF ON, OFF|设为ON时，使INSERT/DELETE/UPDATE低于SELECT和LOCK TABLE READ的优先级。|
|lower_case_table_names|是 0 0, 1|如果设为0，表格名称被存储成固定并且表名称将是大小写敏感的。如果设为1，表格名称被存储成小写并且表名称将是大小写不敏感的。|
|master_info_repository|是 TABLE FILE, TABLE|将服务器主机信息日志写入文件或表。|
|master_verify_checksum|否|OFF ON, OFF|当读取二进制信息时，主服务器通过启用该变量来校验和信息。|
|max_allowed_packet|否|104857600|1,024～1,073,741,824|包或任何生成的中间字符串的最大值。包缓冲区初始化为net_buffer_length字节，但需要时可以增长到max_allowed_packet字节。该值默认很小，以捕获大的（可能是错误的）数据包。该值必须设置为1024的倍数|
|max_binlog_cache_size|否|18446744073709547520|4,096～18,446,744,073,709,547,520|事务能够使用的最大binlog缓存空间。|
|max_binlog_size|否|67108864|4,096～1,073,741,824|当binlog达到最大空间，服务器旋转binlog。|
|max_binlog_stmt_cache_size|否|18446744073709547520|4,096～18,446,744,073,709,547,520|如果一个事务中的非事务语句需要的内存超过该参数值（单位为字节），则服务器报错。|
|max_connect_errors|否|100|1～18,446,744,073,709,551,615|如果一个主机的中断的连接数超出了这个值，这个主机再次连接时将被屏蔽。阻止过多尝试失败的客户端以防止暴力破解密码的情况。如需对该主机进行解锁，下发FLUSH HOST语句或执行mysqladmin flush-hosts命令。|
|max_connections|否|default|1～100,000|允许同时连接的客户端总数。如果设定值为default，表示该参数随内存规格变化。|
|max_delayed_threads|否|20|0～16,384|处理INSERT DELAYED语句的线程总数不得超过该参数值。|
|max_digest_length|是 1024|0～1,048,576|表示可用于计算语句摘要的最大字节数。|
|max_error_count|否|64|0～65,535|显示存储的错误、警告、和说明消息的最大总数。|
|max_execution_time|否|0|0～18,446,744,073,709,551,615|表示执行SELECT语句的超时时间（以毫秒为单位）。如果值为0，则不启用超时。|
|max_heap_table_size|否|16777216|16,384～1,844,674,407,370,954,752|允许MEMORY表格空间增加的最大空间。|
|max_join_size|否|18446744073709551615|1,073,741,824～18,446,744,073,709,551,615|抓取没有正确使用密钥、导致花费较长时长的SELECT语句。|
|max_length_for_sort_data|否|1024|4～8,388,608|ORDER BY 优化。确定使用的filesort算法的索引值大小的限值。|
|max_points_in_geometry|否|65536|3～1,048,576|指定ST_Buffer_Strategy（）函数的points_per_circle参数的最大值。|
|max_prepared_stmt_count|否|16382|0～1,048,576|如果准备大量的语句会消耗服务器的内存资源，这会带来潜在的“拒绝服务”的风险，则使用此参数。|
|max_seeks_for_key|否|18446744073709551615|1～18,446,744,073,709,551,615|如果该参数值较小，会强制MySQL优先使用索引而非表格扫描。|
|max_sort_length|否|1024|4～8,388,608|数据排序时使用的字节数。|
|max_sp_recursion_depth|否|0|0～255|限制存储过程被递归调用的最大次数，最小化对线程堆栈空间的需求。|
|max_user_connections|否|0|0～4,294,967,295|特定MySQL帐户允许的最大同时连接数。|
|max_write_lock_count|否|18446744073709551615|1～18,446,744,073,709,551,615|写锁超过该参数限定的次数后，处理部分等待中的读锁请求。|
|min_examined_row_limit|否|0|0～18,446,744,073,709,551,615|查询检查小于该参数指定值的行，且该行不会被查询或记录到日志中。|
|myisam_data_pointer_size|否|6|2～7|默认指针大小（单位为字节）。当未指定MAX_ROWS时，CREATE TABLE使用该变量创建MyISAM表。|
|myisam_max_sort_file_size|否|9223372036853727232|0～9,223,372,036,853,727,232|重新创建MyISAM索引时，可使用MySQL的最大临时文件大小。|
|myisam_mmap_size|是 18446744073709551615|7～18,446,744,073,709,551,615|用于压缩MyISAM文件内存映射的最大内存。|
|myisam_sort_buffer_size|否|8388608|4,096～18,446,744,073,709,551,615|在REPAIR时对MyISAM索引进行排序时分配的缓冲区的大小。|
|myisam_stats_method|否|nulls_unequal nulls_equal, nulls_unequal, nulls_ignored|指定服务器收集关于MyISAM表索引值分布的统计信息时如何处理NULL值。|
|myisam_use_mmap|否|OFF ON, OFF|MyISAM表读写内存映射。|
|mysql_native_password_proxy_users|否|OFF ON, OFF|控制mysql_native_password内置验证插件是否支持代理用户。|
|net_buffer_length|否|16384|1,024～1,048,576|除非当前可用内存很小，否则不建议修改该变量。 修改时，将该变量设置为服务器预计发送的语句长度。|
|net_read_timeout|否|30|1～31,536,000|中止读数据之前从一个连接等待更多数据的秒数。|
|net_retry_count|否|10|1～18,446,744,073,709,551,615|如果从一个通信端口读数据时被中断，放弃之前重试的次数。|
|net_write_timeout|否|60|1～31,536,000|中止写之前等待一个块被写入连接的秒数。|
|ngram_token_size|是 2|1～10|定义n-gram全文解析器中n-gram标记的大小。|
|offline_mode|否|OFF ON, OFF|控制服务器是否处于“离线模式”。|
|open_files_limit|是 500000|0～18,446,744,073,709,551,615|操作系统允许mysqld打开的文件数量。|
|optimizer_prune_level|否|1 0, 1|控制在优化查询中应用的启发式算法，从优化器搜索空间中排除一些可能不是最优的方案。|
|optimizer_search_depth|否|62|0～62|查询优化器搜索的最大深度。|
|optimizer_switch|否|block_nested_loop  condition_fanout_filter  derived_merge  duplicateweedout  engine_condition_pushdown  firstmatch  index_condition_pushdown  index_merge  index_merge_intersectiONindex_merge_sort_uniONindex_merge_uniONloosescan  materializatiONmrr  mrr_cost_based  semijoin  subquery_materialization_cost_based  use_index_extensions|mrr_cost_based,..............................................|控制优化器行为。|
|optimizer_trace|否|enabled=off,one_line=OFF enabled=on,one_line=on, enabled=on,one_line=off, enabled=off,one_line=on, enabled=off,one_line=off|控制如何跟踪语句。|
|optimizer_trace_features|否|greedy_search=on,range_optimizer=on,dynamic_range=on,repeated_subselect=ON greedy_search=on,range_optimizer=on,dynamic_range=on,repeated_subselect=on, greedy_search=on,range_optimizer=on,dynamic_range=on,repeated_subselect=off, ...............................................|控制语句追踪期间的优化。|
|optimizer_trace_limit|否|1|1～9,223,372,036,854,775,807|控制对保存记录的限制。|
|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|optimizer_trace_max_mem_size|否|16384|0～18,446,744,073,709,551,615|累计保存的优化器记录的最大大小。|
|optimizer_trace_offset|否|-1|-9,223,372,036,854,776,000～9,223,372,036,854,775,807|控制对显示保存记录的限制。|
|performance_schema|是 OFF ON, OFF|启用或禁用性能模式。|
|performance_schema_accounts_size|是 default|-1～1,048,576|性能模式中的accounts表行数。|
|performance_schema_digests_size|是 default|-1～1,048,576|events_statements_summary_by_digest表的最大行数。|
|performance_schema_events_stages_history_long_size|是 default|-1～1,048,576|events_stages_history_long table表的行数。|
|performance_schema_events_stages_history_size|是 default|-1～1,048,576|events_stages_history表中每个线程的行数。|
|performance_schema_events_statements_history_long_size|是 default|-1～1,048,576|events_statements_history表中每个线程的行数。|
|performance_schema_events_statements_history_size|是 default|-1～1,024|events_statements_history表中每个线程的行数。|
|performance_schema_events_transactions_history_long_size|是 default|-1～1,048,576|指定events_transactions_history_long表中的行数。|
|performance_schema_events_transactions_history_size|是 default|-1～1,024|指定在events_transactions_history表中每个线程的行数。|
|performance_schema_events_waits_history_long_size|是 default|-1～1,048,576|events_waits_history_long 表的行数。|
|performance_schema_events_waits_history_size|是 default|-1～1,024|events_waits_history表中每个线程的行数。|
|performance_schema_hosts_size|是 default|-1～1,048,576|hosts表的行数。|
|performance_schema_max_cond_classes|是 default|0～256|最大条件工具数。|
|performance_schema_max_cond_instances|是 default|-1～1,048,576|最大工具化条件对象数。|
|performance_schema_max_digest_length|是 default|0～1,048,576|指定可用于计算语句摘要的最大字节数。|
|performance_schema_max_file_classes|是 default|0～256|最大文件工具数。|
|performance_schema_max_file_handles|是 default|0～1,048,576|最大打开文件对象数。|
|performance_schema_max_file_instances|是 default|-1～1,048,576|最大工具化文件对象数。|
|performance_schema_max_index_stat|是 default|-1～1,048,576|指定性能模式维护统计信息的最大索引数。|
|performance_schema_max_memory_classes|是 default|0～1,024|指定内存工具的最大数量。|
|performance_schema_max_metadata_locks|是 default|-1～104,857,600|指定元数据锁定工具的最大数量。|
|performance_schema_max_mutex_classes|是 default|0～256|最大mutex工具数。|
|performance_schema_max_mutex_instances|是 default|-1～104,857,600|最大工具化mutex对象数。|
|performance_schema_max_prepared_statements_instances|是 default|-1～1,048,576|指定prepared_statements_instances表中的最大行数。|
|performance_schema_max_program_instances|是 default|-1～1,048,576|指定性能架构维护统计信息的存储程序的最大数量。|
|performance_schema_max_rwlock_classes|是 default|0～256|最大rwlock工具数。|
|performance_schema_max_rwlock_instances|是 default|-1～104,857,600|最大工具化rwlock对象数。|
|performance_schema_max_socket_classes|是 default|0～256|最大socket工具数。|
|performance_schema_max_socket_instances|是 default|-1～1,048,576|最大工具化socket对象数。|
|performance_schema_max_stage_classes|是 default|0～256|最大stage工具数。|
|performance_schema_max_statement_classes|是 default|0～256|最大语句工具数。|
|performance_schema_max_statement_stack|是 default|1～256|指定性能模式维护统计信息的嵌套存储的程序调用的最大深度。|
|performance_schema_max_table_handles|是 default|-1～1,048,576|最大打开表对象数。|
|performance_schema_max_table_instances|是 default|-1～1,048,576|最大工具化表对象数。|
|performance_schema_max_table_lock_stat|是 default|-1～1,048,576|指定性能模式维护锁统计信息的最大表数。|
|performance_schema_max_thread_classes|是 default|0～256|最大线程工具数。|
|performance_schema_max_thread_instances|是 default|-1～1,048,576|最大工具化线程对象数。|
|performance_schema_session_connect_attrs_size|是 default|-1～1,048,576|每个线程上，用于保存连接属性字符串的预分配内存总量。|
|performance_schema_setup_actors_size|是 default|-1～1,024|setup_actors表的行数。|
|performance_schema_setup_objects_size|是 default|-1～1,048,576|setup_objects表的行数。|
|performance_schema_users_size|是 default|-1～1,048,576|users表的行数。|
|pid_file|是 /var/lib/mysql/mysqld.pid|--|进程ID文件的路径名。MySQLd_safe等其他程序通过该文件确定进程ID。|
|plugin_dir|是 /usr/lib64/mysql/plugin/|--|指定系统动态链接在哪个目录下查找UDF对象文件。如果不指定该参数，用户定义的函数对象文件必须放在默认目录下。|
|port|是 8635|0～65,535|指定服务器在几个端口上监听TCP/IP连接。|
|preload_buffer_size|否|32768|1,024～1,073,741,824|预加载索引时分配的缓冲大小。|
|profiling_history_size|否|15|0～100|若启用profiling，设置保留profiling的语句数目。|
|query_alloc_block_size|否|8192|1,024～4,294,967,295|为查询解析与执行分配的块尺寸，请输入1024倍数，否则重启失效。|
|query_cache_limit|否|1048576|0～18,446,744,073,709,551,615|不要缓存大于该字节数的结果。|
|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|query_cache_min_res_unit|否|4096|512～18,446,744,073,709,551,608|查询缓存分配的最小块大小（单位为字节）。|
|query_cache_size|否|1048576|0～18,446,744,073,709,551,615|查询缓存区的最大长度。最小值40 KB左右，具体大小根据系统配置决定，要求输入1024倍数，否则重启失效。|
|query_cache_type|是 OFF OFF, ON, DEMAND|查询缓存区的工作模式：OFF：禁用查询缓存区。ON：启用查询缓存区。DEMAND：按需分配模式，只响应SELECT SQL_CACHE命令。|
|query_cache_wlock_invalidate|否|OFF ON, OFF|如果将该变量设为1，在对表进行写锁定的同时将使该表相关的所有查询缓存失效。|
|query_prealloc_size|否|8192|8,192～18,446,744,073,709,547,520|用于查询解析与执行的永久缓冲区。在查询之间该缓冲区不能释放，如果你执行复杂查询，分配更大的query_prealloc_size值可以帮助提高性能，因为它可以降低查询过程中服务器分配内存的需求。允许的值为1024的倍数。|
|range_alloc_block_size|否|4096|4,096～4,294,966,272|进行范围优化时分配的块大小。允许的值为1024的倍数。|
|range_optimizer_max_mem_size|是 8388608|0～18,446,744,073,709,551,615|指定范围优化程序的内存消耗限制。|
|read_buffer_size|否|262144|8,192～2,147,479,552|对每个进行顺序扫描的线程将分配一个缓冲区。如果要进行大量顺序扫描，可增大参数值以提升性能。|
|read_only|否|default ON, OFF|该参数启用时，服务器不允许任何更新，除非是来自从线程的更新。|
|read_rnd_buffer_size|否|524288|1～2,147,483,647|在key-sort操作后按排序读取行时，避免读取磁盘。增大该参数值可提升ORDER BY操作的性能。|
|relay-log|是 /var/lib/mysql/data/rds_mysql-relay-bin|--|relay log文件名。|
|relay_log_info_repository|是 FILE FILE, TABLE|制定该参数后，服务器会将relay log记录至文件或表中。|
|relay_log_recovery|是 OFF ON, OFF|启用服务器启动后的relay log自动恢复功能。|
|require_secure_transport|否|OFF ON, OFF|控制是否需要连接客户端到服务器以使用某种形式的安全传输。|
|rpl_semi_sync_master_wait_for_slave_count|否|1|1～65,535|指在继续之前每个事务master必须接收的slave确认数。|
|rpl_semi_sync_master_wait_point|否|AFTER_SYNC AFTER_SYNC, AFTER_COMMIT|控制在向提交事务的客户端返回状态之前，半同步复制master等待事务接收slave确认的点。|
|rpl_stop_slave_timeout|否|31536000|2～31,536,000|在MySQL 5.7.2及之后的版本，可以通过设置此变量来控制STOP SLAVE在超时前等待的时间长度（以秒为单位）。|
|secure_file_priv|是 /var/lib/mysql/file/|--|限制LOAD_FILE()、LOAD_DATA、SELECT...INTO OUTFILE对特定目录中生效。|
|server_id|否|1|0～4,294,967,295|用于在复制组中区分实例的整数值。|
|session_track_gtids|否|OFF OFF, OWN_GTID, ALL_GTIDS|控制跟踪器捕获GTID并将其在OK数据包中返回。|
|session_track_schema|否|ON ON, OFF|控制服务器是否跟踪当前会话中对默认模式（数据库）名称的更改，并在发生更改时使此信息可供客户端使用。|
|session_track_state_change|否|OFF ON, OFF|控制服务器是否跟踪对当前会话的状态的更改，并在发生状态更改时通知客户端。|
|session_track_system_variables|否|time_zone,autocommit,character_set_client,character_set_results,character_set_connection|--|控制服务器是否跟踪对会话系统变量的更改，并在发生更改时使此信息可供客户端使用。|
|session_track_transaction_info|否|OFF OFF, STATE, CHARACTERISTICS|跟踪对事务属性的更改。|
|sha256_password_proxy_users|否|OFF ON, OFF|控制sha256_password内置身份验证插件是否支持代理用户。|
|show_compatibility_56|否|OFF ON, OFF|为了辅助迁移，可以使用show_compatibility_56系统变量，这将影响是否启用MySQL 5.6兼容性，INFORMATION_SCHEMA和性能模式表，以及SHOW VARIABLES和SHOW STATUS语句，如何提供系统和状态变量信息。|
|skip_external_locking|是 OFF ON, OFF|使用OS锁定而非内部锁定。|
|skip_name_resolve|是 ON ON, OFF|不解析主机名。授权表中的主机列值必须为IP号或本地主机。|
|skip_show_database|是 OFF ON, OFF|SHOW DATABASES语句仅用于拥有SHOW DATABASES权限的用户。|
|slave_allow_batching|否|OFF ON, OFF|控制是否在NDB集群复制从库启用批量更新。|
|slave_checkpoint_group|否|512|32～524,280|指定在调用检查点操作更新SHOW SLAVE STATUS显示的状态前，多线程从机可处理的最大事务。|
|slave_checkpoint_period|否|300|1～4,294,967,295|指定在调用检查点操作更新SHOW SLAVE STATUS显示的多线程从机状态前，等待的最大时长（毫秒）。|
|slave_compressed_protocol|否|OFF ON, OFF|如果master和slave都支持，控制是否使用从/主协议压缩。|
|slave_parallel_type|是 LOGICAL_CLOCK|LOGICAL_CLOCK|当使用多线程slave时（slave_parallel_workers大于0），此选项指定用来决定允许哪些事务在slave上并行执行的策略。|
|slave_parallel_workers|否|default|2～1,024|设置用于并行执行复制事件（事务）的从机工作线程数。如果该变量设为0（默认值），则禁用并行执行。如果设定值为default，表示该参数随CPU规格变化。|
|slave_pending_jobs_size_max|否|16777216|1,024～18,446,744,073,709,550,592|对多线程从机，该参数指定了从机工作队列用于保持住未应用事件的最大可用内存（字节）。|
|slave_preserve_commit_order|是 OFF ON, OFF|对于多线程slaves，启用此变量可确保事务在slave上外部化的顺序与在slave的中继日志中显示的顺序相同。|
|slave_rows_search_algorithms|否|TABLE_SCAN,INDEX_SCAN TABLE_SCAN,INDEX_SCAN, INDEX_SCAN,HASH_SCAN, TABLE_SCAN,HASH_SCAN, TABLE_SCAN,INDEX_SCAN,HASH_SCAN|在为基于行的日志记录和复制准备批处理行时，此变量控制如何搜索行以查找匹配项，即是否使用散列法用于使用主键或唯一键的搜索，使用其他键，或使用no键。|
|slave_sql_verify_checksum|否|ON ON, OFF|该参数启用时，从机检查relay log中读取的校验和。如果发现不匹配，从机停止工作，上报错误。|
|slave_transaction_retries|否|10|0～18,446,744,073,709,551,615|如果复制slave SQL线程由于InnoDB死锁或由于事务的执行时间超过InnoDB的innodb_lock_wait_timeout值或NDB的TransactionDeadlockDetectionTimeout或TransactionInactiveTimeout的值而无法执行事务，它在停止并上报错误之前自动重试slave_transaction_retries设置的次数。|
|slave_type_conversions|是 default ALL_LOSSY,ALL_NON_LOSSY,ALL_SIGNED,ALL_UNSIGNED|控制进行基于行的复制时，从机使用的类型转换模式。|
|slow_launch_time|否|2|1～1,024|如果建立线程需要比该参数值更长的时间，服务器会递增slow_launch_threads的状态变量。|
|slow_query_log|否|ON ON, OFF|启用或禁用慢查询日志。|
|slow_query_log_file|否|/var/lib/mysql/data/slow.log|--|mysql慢查询日志文件存放位置。|
|socket|是 /tmp/mysql.sock|--|用于本地连接的（UNIX）socket文件和（WINDOWS）命名管道。|
|sort_buffer_size|否|262144|32,768～18,446,744,073,709,551,615|增大该参数值可提升ORDER BY或GROUP BY操作的性能。|
|sql_mode|否|NO_AUTO_CREATE_USER NO_ENGINE_SUBSTITUTIONSTRICT_ALL_TABLES ALLOW_INVALID_DATES,ANSI_QUOTES,..................|当前SQL服务器模式。|
|sql_select_limit|否|18446744073709551615|1～18,446,744,073,709,551,615|SELECT语句返回的最大行数。|
|stored_program_cache|否|256|16～524,288|设置每个连接可缓存的存储例程的软上限（soft upper limit）。|
|super_read_only|否|OFF ON, OFF|如果也启用了系统变量super_read_only，则服务器禁止客户端更新，即使是SUPER用户。|
|名称|是否需要重启|值|允许值|描述|
|---|-----------|--|-----|---|
|sync_binlog|否|1|0～4,294,967,295|允许同步binlog（MySQL持久化到硬盘，或依赖于操作系统）。|
|sync_master_info|否|1000|0～4,294,967,295|如果该变量值大于0，在每个sync_master_info事件后，复制从机通过fdatasync()将master.info文件同步到硬盘。|
|sync_relay_log|否|1000|0～4,294,967,295|如果该变量值大于0，MySQL服务器在每次sync_relay_log写入relay log后，通过fdatasync()将日志同步到硬盘。|
|sync_relay_log_info|否|1000|0～4,294,967,295|如果该变量值大于0，在每次sync_relay_log_info事务后，复制从机通过fdatasync()将relay-log.info文件同步到硬盘。|
|table_definition_cache|否|1400|400～524,288|可存入定义缓存中的表定义（来自.frm文件）。默认值大小根据系统配置决定。|
|table_open_cache|否|2000|1～524,288|缓存的打开表的个数。|
|table_open_cache_instances|是 16|1～64|打开的表缓存实例数。|
|thread_cache_size|否|11|0～16,384|要缓存的线程数，修改该参数值不会优化线程实施性能。|
|thread_stack|是 262144|131,072～18,446,744,073,709,551,615|如果线程堆栈大小过小，会限制服务器能处理的SQL语句的复杂程度、存储程序的递归深度，和其他耗费内存的操作。允许的值为1024的倍数。|
|time_zone|否|Asia/Shanghai America/Santiago, America/Tijuana, Asia/Riyadh, Asia/Seoul, Asia/Shanghai,  UTC, SYSTEM,..................|服务器时区。|
|tls_version|是 TLSv1TLSv1.1 TLSv1,TLSv1.1|指定服务器允许的用于加密连接的协议。|
|tmp_table_size|否|16777216|1,024～18,446,744,073,709,551,615|内部（内存中）临时表的最大大小，如果一个内部的临时内存表超过这个尺寸，MySQL自动的把它转换成基于磁盘的MyISAM表。|
|tmpdir|是 /var/lib/mysql/tmp|--|临时文件和临optimizer_switch时表的存放目录。|
|transaction_alloc_block_size|否|8192|1,024～131,072|为需要内存的按事务内存池增加的内存大小(字节)。允许的值为1024的倍数。|
|transaction_prealloc_size|否|4096|1,024～131,072|不同的事务相关配置会从按事务内存池中获取内存。如果由于内存池可用内存不足导致配置要求无法满足，内存池的内存会增加。允许的值为1024的倍数。|
|transaction_write_set_extraction|否|OFF OFF, MURMUR32, XXHASH6|定义用于生成标识与事务关联的写入操作的哈希的算法。|
|tx_isolation|否|REPEATABLE-READ READ-UNCOMMITTED, READ-COMMITTED, REPEATABLE-READ, SERIALIZABLE|指定默认的事务隔离等级。|
|updatable_views_with_limit|否|YES YES, NO|如果更新语句中包含LIMIT子句（通常使用GUI工具生成），当视图不包含底层表中定义的主键的所有列时，该变量控制视图是否能够更新。|
|validate_password_dictionary_file|否|default|--|validate_password插件用于校验密码的词典文件的路径名。|
|validate_password_length|否|8|0～2,147,483,647|validate_password插件校验的密码的最小字符数。|
|validate_password_mixed_case_count|否|1|0～2,147,483,647|指定当密码策略为MEDIUM（中）或更高时，为通过validate_password校验，密码至少需包含多少个大小写字符。|
|validate_password_number_count|否|1|0～2,147,483,647|指定当密码策略为MEDIUM（中）或更高时，为通过validate_password校验，密码至少需包含多少个数字。|
|validate_password_policy|否|MEDIUM LOW, MEDIUM, STRONG|validate_password插件执行的密码策略。|
|validate_password_special_char_count|否|1|0～2,147,483,647|指定当密码策略为MEDIUM（中）或更高时，为通过validate_password校验，密码至少需包含多少个非字母数字字符。|
|wait_timeout|否|28800|1～31,536,000|服务器关闭连接之前等待非交互式连接活动的秒数。|