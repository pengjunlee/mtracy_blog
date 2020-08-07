# 概述
Redis-Sentinel是Redis官方推荐的高可用性(HA)解决方案，当用Redis做Master-slave的高可用方案时，假如master宕机了，Redis本身(包括它的很多客户端)都没有实现自动进行主备切换，而Redis-sentinel本身也是一个独立运行的进程，它能监控多个master-slave集群，发现master宕机后能进行自动切换。

它的主要功能有以下几点：

- 不时地监控redis是否按照预期良好地运行;
- 如果发现某个redis节点运行出现状况，能够通知另外一个进程(例如它的客户端);
- 能够进行自动切换。当一个master节点不可用时，能够选举出master的多个slave(如果有超过一个slave的话)中的一个来作为新的master,其它的slave节点会将它所追随的master的地址改为被提升为master的slave的新地址。 

# 无failover时的配置纠正
即使当前没有failover正在进行，sentinel依然会使用当前配置去设置监控的master。特别是：

根据最新配置确认为slaves的节点却声称自己是master(参考上文例子中被网络隔离后的的redis3)，这时它们会被重新配置为当前master的slave。

如果slaves连接了一个错误的master，将会被改正过来，连接到正确的master。  

# Slave选举与优先级
当一个sentinel准备好了要进行failover，并且收到了其他sentinel的授权，那么就需要选举出一个合适的slave来做为新的master。

slave的选举主要会评估slave的以下几个方面：

- 与master断开连接的次数
- Slave的优先级
- 数据复制的下标(用来评估slave当前拥有多少master的数据)
- 进程ID

如果一个slave与master失去联系超过10次，并且每次都超过了配置的最大失联时间(down-after-milliseconds option)，并且，如果sentinel在进行failover时发现slave失联，那么这个slave就会被sentinel认为不适合用来做新master的。

更严格的定义是，如果一个slave持续断开连接的时间超过
(down-after-milliseconds * 10) + milliseconds_since_master_is_in_SDOWN_state

就会被认为失去选举资格。

符合上述条件的slave才会被列入master候选人列表，并根据以下顺序来进行排序：

- sentinel首先会根据slaves的优先级来进行排序，优先级越小排名越靠前（？）。
- 如果优先级相同，则查看复制的下标，哪个从master接收的复制数据多，哪个就靠前。
- 如果优先级和下标都相同，就选择进程ID较小的那个。

一个redis无论是master还是slave，都必须在配置中指定一个slave优先级。要注意到master也是有可能通过failover变成slave的。

如果一个redis的slave优先级配置为0，那么它将永远不会被选为master。但是它依然会从master哪里复制数据。 

# Sentinel和Redis身份验证
当一个master配置为需要密码才能连接时，客户端和slave在连接时都需要提供密码。

master通过requirepass设置自身的密码，不提供密码无法连接到这个master。

slave通过masterauth来设置访问master时的密码。

但是当使用了sentinel时，由于一个master可能会变成一个slave，一个slave也可能会变成master，所以需要同时设置上述两个配置项。  

# Sentinel API
Sentinel默认运行在26379端口上，sentinel支持redis协议，所以可以使用redis-cli客户端或者其他可用的客户端来与sentinel通信。

有两种方式能够与sentinel通信：

- 一种是直接使用客户端向它发消息
- 另外一种是使用发布/订阅模式来订阅sentinel事件，比如说failover，或者某个redis实例运行出错，等等。 

# Sentinel命令
sentinel支持的合法命令如下： 

	PING sentinel回复PONG.
	SENTINEL masters 显示被监控的所有master以及它们的状态. 
	SENTINEL master <master name> 显示指定master的信息和状态；
	SENTINEL slaves <master name> 显示指定master的所有slave以及它们的状态；
	SENTINEL get-master-addr-by-name <master name> 返回指定master的ip和端口，如果正在进行failover或者failover已经完成，将会显示被提升为master的slave的ip和端口。
	SENTINEL reset <pattern> 重置名字匹配该正则表达式的所有的master的状态信息，清楚其之前的状态信息，以及slaves信息。
	SENTINEL failover <master name> 强制sentinel执行failover，并且不需要得到其他sentinel的同意。但是failover后会将最新的配置发送给其他sentinel。

# 动态修改Sentinel配置
从redis2.8.4开始，sentinel提供了一组API用来添加，删除，修改master的配置。 

需要注意的是，如果你通过API修改了一个sentinel的配置，sentinel不会把修改的配置告诉其他sentinel。你需要自己手动地对多个sentinel发送修改配置的命令。
以下是一些修改sentinel配置的命令： 

	SENTINEL MONITOR <name> <ip> <port> <quorum> 这个命令告诉sentinel去监听一个新的master
	SENTINEL REMOVE <name> 命令sentinel放弃对某个master的监听
	SENTINEL SET <name> <option> <value> 这个命令很像Redis的CONFIG SET命令，用来改变指定master的配置。支持多个<option><value>。例如以下实例：
	SENTINEL SET objects-cache-master down-after-milliseconds 1000

只要是配置文件中存在的配置项，都可以用SENTINEL SET命令来设置。这个还可以用来设置master的属性，比如说quorum(票数)，而不需要先删除master，再重新添加master。例如： 

	SENTINEL SET objects-cache-master quorum 5

# 增加或删除Sentinel
由于有sentinel自动发现机制，所以添加一个sentinel到你的集群中非常容易，你所需要做的只是监控到某个Master上，然后新添加的sentinel就能获得其他sentinel的信息以及masterd所有的slave。

如果你需要添加多个sentinel，建议你一个接着一个添加，这样可以预防网络隔离带来的问题。你可以每个30秒添加一个sentinel。最后你可以用SENTINEL MASTER mastername来检查一下是否所有的sentinel都已经监控到了master。

删除一个sentinel显得有点复杂：因为sentinel永远不会删除一个已经存在过的sentinel，即使它已经与组织失去联系很久了。

要想删除一个sentinel，应该遵循如下步骤：

- 停止所要删除的sentinel
- 发送一个SENTINEL RESET * 命令给所有其它的sentinel实例，如果你想要重置指定master上面的sentinel，只需要把*号改为特定的名字，注意，需要一个接一个发，每次发送的间隔不低于30秒。
- 检查一下所有的sentinels是否都有一致的当前sentinel数。使用SENTINEL MASTER mastername 来查询。 

# 删除旧master或者不可达slave
sentinel永远会记录好一个Master的slaves，即使slave已经与组织失联好久了。这是很有用的，因为sentinel集群必须有能力把一个恢复可用的slave进行重新配置。

并且，failover后，失效的master将会被标记为新master的一个slave，这样的话，当它变得可用时，就会从新master上复制数据。

然后，有时候你想要永久地删除掉一个slave(有可能它曾经是个master)，你只需要发送一个SENTINEL RESET master命令给所有的sentinels，它们将会更新列表里能够正确地复制master数据的slave。  

# 发布/订阅
客户端可以向一个sentinel发送订阅某个频道的事件的命令，当有特定的事件发生时，sentinel会通知所有订阅的客户端。需要注意的是客户端只能订阅，不能发布。

订阅频道的名字与事件的名字一致。例如，频道名为sdown 将会发布所有与SDOWN相关的消息给订阅者。

如果想要订阅所有消息，只需简单地使用PSUBSCRIBE *

以下是所有你可以收到的消息的消息格式，如果你订阅了所有消息的话。第一个单词是频道的名字，其它是数据的格式。

注意：以下的instance details的格式是： 

	<instance-type> <name> <ip> <port> @ <master-name> <master-ip> <master-port>

如果这个redis实例是一个master，那么@之后的消息就不会显示。  

	  +reset-master <instance details> -- 当master被重置时.
	  +slave <instance details> -- 当检测到一个slave并添加进slave列表时.
	  +failover-state-reconf-slaves <instance details> -- Failover状态变为reconf-slaves状态时
	  +failover-detected <instance details> -- 当failover发生时
	  +slave-reconf-sent <instance details> -- sentinel发送SLAVEOF命令把它重新配置时
	  +slave-reconf-inprog <instance details> -- slave被重新配置为另外一个master的slave，但数据复制还未发生时。
	  +slave-reconf-done <instance details> -- slave被重新配置为另外一个master的slave并且数据复制已经与master同步时。
	  -dup-sentinel <instance details> -- 删除指定master上的冗余sentinel时 (当一个sentinel重新启动时，可能会发生这个事件).
	  +sentinel <instance details> -- 当master增加了一个sentinel时。
	  +sdown <instance details> -- 进入SDOWN状态时;
	  -sdown <instance details> -- 离开SDOWN状态时。
	  +odown <instance details> -- 进入ODOWN状态时。
	  -odown <instance details> -- 离开ODOWN状态时。
	  +new-epoch <instance details> -- 当前配置版本被更新时。
	  +try-failover <instance details> -- 达到failover条件，正等待其他sentinel的选举。
	  +elected-leader <instance details> -- 被选举为去执行failover的时候。
	  +failover-state-select-slave <instance details> -- 开始要选择一个slave当选新master时。
	  no-good-slave <instance details> -- 没有合适的slave来担当新master
	  selected-slave <instance details> -- 找到了一个适合的slave来担当新master
	  failover-state-send-slaveof-noone <instance details> -- 当把选择为新master的slave的身份进行切换的时候。
	  failover-end-for-timeout <instance details> -- failover由于超时而失败时。
	  failover-end <instance details> -- failover成功完成时。
	  switch-master <master name> <oldip> <oldport> <newip> <newport> -- 当master的地址发生变化时。通常这是客户端最感兴趣的消息了。
	  +tilt -- 进入Tilt模式。
	  -tilt -- 退出Tilt模式。

# TILT 模式
redis sentinel非常依赖系统时间，例如它会使用系统时间来判断一个PING回复用了多久的时间。

然而，假如系统时间被修改了，或者是系统十分繁忙，或者是进程堵塞了，sentinel可能会出现运行不正常的情况。

当系统的稳定性下降时，TILT模式是sentinel可以进入的一种的保护模式。当进入TILT模式时，sentinel会继续监控工作，但是它不会有任何其他动作，它也不会去回应is-master-down-by-addr这样的命令了，因为它在TILT模式下，检测失效节点的能力已经变得让人不可信任了。

如果系统恢复正常，持续30秒钟，sentinel就会退出TITL模式。

-BUSY状态

注意：该功能还未实现。

当一个脚本的运行时间超过配置的运行时间时，sentinel会返回一个-BUSY 错误信号。如果这件事发生在触发一个failover之前，sentinel将会发送一个SCRIPT KILL命令，如果script是只读的话，就能成功执行。 