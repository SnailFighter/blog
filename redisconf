# Redis configuration file example.
#
# Note that in order to read the configuration file, Redis must be
# started with the file path as first argument:
#
# ./redis-server /path/to/redis.conf

# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.

Redis配置文件样例
注意：为了能够读取配置文件，Redis必须把以下文件路径作为第一个参数
./redis-server /path/to/redis.conf

注意单位：当内存大小需要指定时，它可以被指定通用格式，1k 5GB 4M 等
1k	1000字节
1kb	1024字节（注意与上面的不同）
1m	1000000字节
1mb	1024*1024字节
1g	1000*1000*1000字节
1gb	1024*1024*1024字节

对单位的大小写不敏感，因此1GB 1Gb 1gB 都是一样的。

################################## INCLUDES ###################################

# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Notice option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include /path/to/local.conf
# include /path/to/other.conf

这这里可以包含一个或者多个其他的配置文件， 它是有用的如果你有一个标准模版它适用与所有的Redis
服务器，但是需要定制一些预先的服务器设置。包含文件可以把其他文件引进来，理智地地适用它们。

注意：选项“include”将不能够被重写通过从管理员或者Redis哨兵发出的“CONFIG REWRITE”命令。因此
Redis总是使用最后被处理的值作为配置指令的值，最好是把includes（即引进的文件）在这个文件
开始的文职，防止在运行时配置被重写。

如果你对引进重载配置很感兴趣，最好这么使用在最后一行
include /path/to/local.conf
include /path/to/other.conf

################################## NETWORK #####################################

# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all the network interfaces available on the server.
# It is possible to listen to just one or multiple selected interfaces using
# the "bind" configuration directive, followed by one or more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1

默认情况下，如果”bind“配置指令没有被指定，那么Redis监听服务器上所有的可用的网络
接口。它可以监听一个或者多个接口通过使用”bind“配置指令，排序一个或多个id地址

例子：
bind 192.168.1.100 10.0.0.1
bing 127.0.0.1::1

#
# ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
# internet, binding to all the interfaces is dangerous and will expose the
# instance to everybody on the internet. So by default we uncomment the
# following bind directive, that will force Redis to listen only into
# the IPv4 lookback interface address (this means Redis will be able to
# accept connections only from clients running into the same computer it
# is running).
#
# IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
# JUST COMMENT THE FOLLOWING LINE.
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bind 127.0.0.1

警告：
如果运行Redis的计算机是直接暴漏到网络中，那么绑定所有的接口是危险的，将暴漏给网络中的
每一个人。因此，默认情况下，我们取消绑定指令，那么将强制Redis仅仅监听IPv4回调接口地址（
这意味着Redis将能够仅仅来自Redis所在电脑的客户端的连接）

如果你确定你监听所有的接口，仅仅使用一些命令：
bing 127.0.0.1


# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.

#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.

#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
保护模式是一层安全保护，为了避免Redis实例在开发的状态下留在网路被访问或者利用
当安全模式开启并且有以下情景之一：
1 服务器乜有绑定一系列的可用地址使用”bind“命令
2 没有配置密码
此时，服务器仅接收来自IP4 IPv6的地址为127.0.0.1和::1的回送，和来自Unix域的连接。

#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode yes

默认情况下保护模式是被开启的可用的。你可以关闭它只要你确定来自其他主机的客户端，
Redis即使没有认证被配置，或者没有使用”bind“指令指定的一系列的可用的接口可以连接
到Redis。

rotected-mode yes

# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379

接收指定端口的连接，默认端口为6379（IANA #815344）
如果端口0 被指定，Redis将不监听任何TCP连接。

# TCP listen() backlog.
#
# In high requests-per-second environments you need an high backlog in order
# to avoid slow clients connections issues. Note that the Linux kernel
# will silently truncate it to the value of /proc/sys/net/core/somaxconn so
# make sure to raise both the value of somaxconn and tcp_max_syn_backlog
# in order to get the desired effect.
tcp-backlog 511

在每秒大量请求的环境中，你需要一个大的存储避免慢连接问题（大概意思应该是把很多请求积累在一起？？？）
Linux内核静默地截取它为/proc/sys/net/core/somaxconn 中的值以确保提高最大连接和tcp最大异步值
为了得到需要的效率
tco-backlog 511

# Unix socket.
#
# Specify the path for the Unix socket that will be used to listen for
# incoming connections. There is no default, so Redis will not listen
# on a unix socket when not specified.
#
# unixsocket /tmp/redis.sock
# unixsocketperm 700 


Unix socket

为Unix socket指明路径将能够监听所有的进来的连接。没有默认值，在不指定它时，Redis将不监听unix socket


# Close the connection after a client is idle for N seconds (0 to disable)
timeout 0

关闭闲置的连接在N秒后（0秒为不关闭）
timeout 0


# TCP keepalive.
#
# If non-zero, use SO_KEEPALIVE to send TCP ACKs to clients in absence
# of communication. This is useful for two reasons:
#
# 1) Detect dead peers.
# 2) Take the connection alive from the point of view of network
#    equipment in the middle.
#
# On Linux, the specified value (in seconds) is the period used to send ACKs.
# Note that to close the connection the double of the time is needed.
# On other kernels the period depends on the kernel configuration.
#
# A reasonable value for this option is 300 seconds, which is the new
# Redis default starting with Redis 3.2.1.
tcp-keepalive 300

TCP 连接存活
如果指定为非零，使用S0_KEEPALIVE 发送tcp确认应答到客户端无交互时。这是有用的，有两个原因：
1 发现“死亡的”客户端
2 让中间网络设备保持有效的连接

在Linux系统中，这个指定的值（单位为秒）是发送确认应答的周期时间
注意为了关闭连接，需要双倍的时间（就是连个周期喽）
在其他内核（这句话应该说的非linux 内核）这个周期取决于内核配置。

一个可选的合理的周期值是300秒，从版本3.2.1开始，300s是默认值
tcp-keepalive 300


################################# GENERAL #####################################

# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize no

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised no

默认时，Redis不作为守护进程运行，使用‘yes’如果你需要时。
注意 Redis将写一个pid文件在/var/ren/redis.pid 位置，当作为守护进程时。
daemonize no

if you run Redis from upstart or systemd,(不知道怎么翻译，如果运行Redis？？？)，Redis能够
影响监控树 ，选线：
supervised no 		不影响监控树
supervised upstar	设置upstart信号， 通过设置Redsi为SIGSTOP模式
supervised systemd	设置systemed信号，通过设置$NOTIFY_SOCKET中 READY=1
upervised auto 		通过UPSTART_JOB 或者MOTIFY_SOCKET环境变量设置upstart或者systemed模式

注意：这些监控模式方法仅仅标示出”进程是准备文毕“，它们不会连续地频繁地ping到监控者。
daemonize no
 

# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
pidfile /var/run/redis_6379.pid

如果一个pid文件被指定，Redis写这个文件当指定了在开启和退出它的时候

当服务不是作为守护进程运行时，如果没有在配置文件中指定，那么讲不会有pid文件被创建。
当服务作为守护进程运行时，这个pid文件将被使用即使没有指定，默认在/var/run/redis.pid

创建一个pid文件是最好的做法：如果Redis 不创建它，没有任何坏事发生，服务将正常开启运行。
（这个意思大概是建议大家自己建立一个pid文件吧）

pidfile /var/run/redis_6379.pid

# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice

指定服务的输出等级
可以开始以下的一种：
debug （很多信息，对开发和测试是有用的）
verbose （仅仅有很多有用的信息，但是不像debug级别）
notice （适度的输出，你可能想输出的内容）
warning（只有那些非常重要和关键的信息被打印出来）

loglevel notice




# Specify the log file name. Also the empty string can be used to force
# Redis to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile ""


指定日志文件名称，也可以使用空字符串，这能够强制Redis在标准输出设备输出。注意，如果你
使用标准输出但是不是作为守护进程运行的，那么日志将被发送到/dev/null 位置

logfile ""

# To enable logging to the system logger, just set 'syslog-enabled' to yes,
# and optionally update the other syslog parameters to suit your needs.
# syslog-enabled no

为了能够输出日志到系统日志中，仅仅设置‘syslog-enabled‘为yew就可以了，
可以选择地更新其他的系统日志参数以适应你自己的需求。
syslog-enabled no 


# Specify the syslog identity.
# syslog-ident redis

指定系统日志标志符
syslog-ident redis

# Specify the syslog facility. Must be USER or between LOCAL0-LOCAL7.
# syslog-facility local0

指定系统日志设备。必须使用USER或者LOCAL0-LOCAL7
syslog-facility local0

# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
databases 16

设置数据库号码（差点设置数据库数量了，这里可不是数量的意思啊），默认为DB0，你可以从预连接的
数据库中选择一个不同的一个，使用SELECT <数据库id> 数据库id 是一个介于0到（数据库-1的值）的数字
databases 16

################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 900 1
save 300 10
save 60 10000

##############################快照###########################################
保存数据库到硬盘
	save <seconds> <changes>
保存数据库如果给定的时间且对达到了指定对数据库的操作数量同事发生。

在下面的例子中，行为记录奖杯保存：
900秒（15分钟）后且至少一个键发生了变化
300秒（5分钟）后且至少十个键发生了变化
60秒（1分钟）后且至少10000个键发生了变化

注意：你可以不完整保存数据通过添加“save”注释到指定行
同样可以移除所有的原来的保存点配置通过一个带有一个空字符串的保存指令，
像下面的例子：
 save ""
 save 900 1
 save 3000 10
 save 60 10000

猜想：应该可以指定多个save指令，这样满足一个即可保存到数据库，有待验证。
	

# By default Redis will stop accepting writes if RDB snapshots are enabled
# (at least one save point) and the latest background save failed.
# This will make the user aware (in a hard way) that data is not persisting
# on disk properly, otherwise chances are that no one will notice and some
# disaster will happen.
#
# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes

默认地Redis将停止接收写操作如果快照被激活（至少有一个保存点）且最近后台保存失败。
这将让使用者意识到（大概是强制地意思把，不知道如何合适地翻译）数据没有被适当地持久化到硬盘，否则，没有人注意到这样的改变
一些发生一些错误。

如果后台保存进程再次开启，redis将自动允许写操作。

然而如果你建立了Redis服务和持久化的监控，你也许想不让Redis有这个特点（上面的自动写的行为）当保存到硬盘
权限等方面发生了问题。
stop-writes-on-bgsave-error yes

# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes

是否压缩字符串对象使用LZF当保存到dump.rdb数据库时？
默认地，它被设置为‘yes’因为它总是被允许？？？
如果你想节约一些cpu资源在保存数据时，设置它为‘no’，但是这时数据集可能更大一些如果你有
一些可压缩的值或者键
rdbcompression yes

# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes

从RDB版本5起CRC64校验被放置在文件末尾。
这使得文件更加固定化，但有一个性能上的影响（大约10%）当保存载入RDB文件时，因此你可以不适用它
通过指定最大执行数。

被创建的RDB文件不能指定一个0值的校验数，这将让载入代码跳过校验
rdbchecksum yes

# The filename where to dump the DB
dbfilename dump.rdb

这个存储DB的名称
dbfilename dump.rdb

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir ./

工作目录

这个数据库将被写在文件夹里面，使用‘dbfilename’配置指令指定的文件名。

################################# REPLICATION #################################

# Master-Slave replication. Use slaveof to make a Redis instance a copy of
# another Redis server. A few things to understand ASAP about Redis replication.
#
# 1) Redis replication is asynchronous, but you can configure a master to
#    stop accepting writes if it appears to be not connected with at least
#    a given number of slaves.
# 2) Redis slaves are able to perform a partial resynchronization with the
#    master if the replication link is lost for a relatively small amount of
#    time. You may want to configure the replication backlog size (see the next
#    sections of this file) with a sensible value depending on your needs.
# 3) Replication is automatic and does not need user intervention. After a
#    network partition slaves automatically try to reconnect to masters
#    and resynchronize with them.
#
# slaveof <masterip> <masterport>

################################复制##########################################
主从复制。使用 ‘slaveof’指令设置一个Redis服务实例副本。一些Redis复制的东西需要理解。
1 Redis 复制是异步的，但是你可以配置一个主服务器停止接收写操作如果没有给定任何从服务器。
2 Redis 从服务器能个执行局部异步操作当它丢失了复制连接在相对较短的时间。你也许想指定
一个合理的值配置复制块的大小（在次文件的下一个章节）根据你的需求。
3 复制是一个原子操作，不需要用户干涉。网络断开再次恢复后从服务器自动试着重连接到主服务器
再次同步。

# If the master is password protected (using the "requirepass" configuration
# directive below) it is possible to tell the slave to authenticate before
# starting the replication synchronization process, otherwise the master will
# refuse the slave request.
#
# masterauth <master-password>

如果主服务器被密码保护（使用下面的“requirepass”配置指令），它可能告诉从服务器去认证
在开启异步复制处理前，否则主服务器将拒绝从服务器的请求。
masterauth <master-passwork>


# When a slave loses its connection with the master, or when the replication
# is still in progress, the slave can act in two different ways:
#
# 1) if slave-serve-stale-data is set to 'yes' (the default) the slave will
#    still reply to client requests, possibly with out of date data, or the
#    data set may just be empty if this is the first synchronization.
#
# 2) if slave-serve-stale-data is set to 'no' the slave will reply with
#    an error "SYNC with master in progress" to all the kind of commands
#    but to INFO and SLAVEOF.
#
slave-serve-stale-data yes

当从服务器丢失了到主服务器的连接，或者当复制仍然在处理中，从服务器可以表现两种
不同的行为：
1 如果 ‘slave-serve-stale-data’ 被设置为‘yes’（默认值），这个从服务器将仍然客户端
  的请求，可能会使用过时的数据，或者被设置的数据仅仅是空的如果如果它是第一次同步。
2 如果‘slave-serve-stale-data’被设置为‘no’，从服务器将响应‘SYNC with master in progress’
  错误给除了‘INFO’和‘SLAVEOF’外的所有命令。

# You can configure a slave instance to accept writes or not. Writing against
# a slave instance may be useful to store some ephemeral data (because data
# written on a slave will be easily deleted after resync with the master) but
# may also cause problems if clients are writing to it because of a
# misconfiguration.

你可以配置或者不配置一个从服务实例来接收写操作。写入一个从服务实例也许是有用的
去存储一些短期数据（因为这些被写入从服务器的数据将很容易地被删除掉在从主服务器同步后）
但是这也会导致一些问题如果客户端正在写如它因为错误的配置。
#
# Since Redis 2.6 by default slaves are read-only.
从Redis2.6版本开始从服务器为只读模式。

# Note: read only slaves are not designed to be exposed to untrusted clients
# on the internet. It's just a protection layer against misuse of the instance.
# Still a read only slave exports by default all the administrative commands
# such as CONFIG, DEBUG, and so forth. To a limited extent you can improve
# security of read only slaves using 'rename-command' to shadow all the
# administrative / dangerous commands.
slave-read-only yes
注意：只读从服务器被设计为不暴露给网络上不信任的客户端。它仅作为保护层防止实例被滥用。
只读的从服务任然暴漏给所有的管理命令，默认地，例如CONFIG，DEBUG等等。你可以提高只读
服务的安全限制通过使用‘rename-command’来隐蔽所有的管理的/危险的命令。

# Replication SYNC strategy: disk or socket.
异步复制策略：disk 或者 socket
#
# -------------------------------------------------------
# WARNING: DISKLESS REPLICATION IS EXPERIMENTAL CURRENTLY
  警告：diskless 复制目前还在实验阶段
# -------------------------------------------------------
#
# New slaves and reconnecting slaves that are not able to continue the replication
# process just receiving differences, need to do what is called a "full
# synchronization". An RDB file is transmitted from the master to the slaves.
# The transmission can happen in two different ways:
#
# 1) Disk-backed: The Redis master creates a new process that writes the RDB
#                 file on disk. Later the file is transferred by the parent
#                 process to the slaves incrementally.
# 2) Diskless: The Redis master creates a new process that directly writes the
#              RDB file to slave sockets, without touching the disk at all.
#
# With disk-backed replication, while the RDB file is generated, more slaves
# can be queued and served with the RDB file as soon as the current child producing
# the RDB file finishes its work. With diskless replication instead once
# the transfer starts, new slaves arriving will be queued and a new transfer
# will start when the current one terminates.
#
# When diskless replication is used, the master waits a configurable amount of
# time (in seconds) before starting the transfer in the hope that multiple slaves
# will arrive and the transfer can be parallelized.
#
# With slow disks and fast (large bandwidth) networks, diskless replication
# works better.
repl-diskless-sync no

新的从服务器或者重连接的从服务器只能接收不同的数据而不能进行复制操作，需要做称为
‘full synchronization’操作才可以。一个RDB文件从主服务器传送到从服务器。可以以不通过的
两种方式发生传送。
1 Disk-backed:	Redis主服务器创建一个新的写入RDB文件到硬盘的进程。随后这个文件通过	
		通过父进程递增地传送到从服务器。
2 Diskless：	Redis主服务器创建一个新的直接写入到RDB文件到从连接的竭诚，部设计到硬盘。

使用disk-backed复制，当RDB文件被生成，更多的从服务器可以被排成队列和使用RDB文件提供服务当孩子
生成RDB文件按完成她的工作（？？？）。

当使用无磁盘复制时，主服务器在开始传输前等待一段配置指定的时间（秒为单位）以期待多个从
服务器到达，传输将并行。

# When diskless replication is enabled, it is possible to configure the delay
# the server waits in order to spawn the child that transfers the RDB via socket
# to the slaves.
#
# This is important since once the transfer starts, it is not possible to serve
# new slaves arriving, that will be queued for the next RDB transfer, so the server
# waits a delay in order to let more slaves arrive.
#
# The delay is specified in seconds, and by default is 5 seconds. To disable
# it entirely just set it to 0 seconds and the transfer will start ASAP.
repl-diskless-sync-delay 5

当无磁盘复制可使用时，可以配置一段让服务器等待的延迟时间为了以产生大量子（？？？）
通过socket传输RDB到每个从服务器。

一旦传输开始，不能给提供服务当新的从服务到达，它将在下次RDB传输中被放入队列，这一点
是很重要的。因此，服务要延迟一段时间以便让更多的从服务到达。

延迟时间以秒为单位来指定，默认是5秒。可完全地关闭延迟通过指定它为0秒，传输将尽快开始。
repl-diskless-sync-dely 5

# Slaves send PINGs to server in a predefined interval. It's possible to change
# this interval with the repl_ping_slave_period option. The default value is 10
# seconds.
#
# repl-ping-slave-period 10

从服务器以一个预先定义的时间间隔发送pings到服务器。可以改变这个间隔通过使用
repl-ping-slave-period 选项。默认值为10秒。
repl-ping-slave-period 10

# The following option sets the replication timeout for:
#
# 1) Bulk transfer I/O during SYNC, from the point of view of slave.
# 2) Master timeout from the point of view of slaves (data, pings).
# 3) Slave timeout from the point of view of masters (REPLCONF ACK pings).
#
# It is important to make sure that this value is greater than the value
# specified for repl-ping-slave-period otherwise a timeout will be detected
# every time there is low traffic between the master and the slave.
#
# repl-timeout 60

以下选项对下面的情况设置复制超市时间
1 在异步时大量I/O传输，来自从服务器视图点（？？？）
2 主服务器超时（？？？）
3 从服务器超时（？？？）

确保复超时时间比pl-ping-slave-period 时间大是重要的，否则每次有主从服务器
有低速率的发生时都会超时。

# Disable TCP_NODELAY on the slave socket after SYNC?
#
# If you select "yes" Redis will use a smaller number of TCP packets and
# less bandwidth to send data to slaves. But this can add a delay for
# the data to appear on the slave side, up to 40 milliseconds with
# Linux kernels using a default configuration.
#
# If you select "no" the delay for data to appear on the slave side will
# be reduced but more bandwidth will be used for replication.
#
# By default we optimize for low latency, but in very high traffic conditions
# or when the master and slaves are many hops away, turning this to "yes" may
# be a good idea.
repl-disable-tcp-nodelay no

在异步后在从服务器socket设置TCP_NODELAY失效？

如果你选择了‘yes’，Redis将使用更少的TCP包和更小的带宽向从服务器发送数据。但是这回
在从服务器方增加了延迟，多达了40毫秒linux内核默认配置。

如果你选择了‘no’，从服务端的延迟将减少，但是复制时需要更大的带宽。

默认地，我们为了更低延迟做优化，但是在高传输情况或者当主从服务器有很多心跳包，设置
为‘yes’也许是个好注意。
repl-disable-tcp-nodelay no

# Set the replication backlog size. The backlog is a buffer that accumulates
# slave data when slaves are disconnected for some time, so that when a slave
# wants to reconnect again, often a full resync is not needed, but a partial
# resync is enough, just passing the portion of data the slave missed while
# disconnected.
#
# The bigger the replication backlog, the longer the time the slave can be
# disconnected and later be able to perform a partial resynchronization.
#
# The backlog is only allocated once there is at least a slave connected.
#
# repl-backlog-size 1mb

设置复制块大小。这个块是一个缓存，缓存了当从服务器断开了一段时间积累的从服务器数据，
因此当一个从服务器想重新连接时，经常不需要全同步，局部同步就足够了，仅仅发送从服务器
断开连接时的那部分丢失的数据即可。

更大的复制块，从服务器可以断开更久时间，更晚地执行局部同步。

这个块只在有至少一个从服务器连接是被分配。

# After a master has no longer connected slaves for some time, the backlog
# will be freed. The following option configures the amount of seconds that
# need to elapse, starting from the time the last slave disconnected, for
# the backlog buffer to be freed.
#
# A value of 0 means to never release the backlog.
#
# repl-backlog-ttl 3600

主服务器失去较长的到从服务器的连接，这个块将被释放。下面的可选项配置了没有连接的时间的
秒数，从这个最后的从服务器断开连接开始，一直到块缓存被释放。

0值意味着永远不释放块

repl-backlog-ttl 3600



# The slave priority is an integer number published by Redis in the INFO output.
# It is used by Redis Sentinel in order to select a slave to promote into a
# master if the master is no longer working correctly.
#
# A slave with a low priority number is considered better for promotion, so
# for instance if there are three slaves with priority 10, 100, 25 Sentinel will
# pick the one with priority 10, that is the lowest.
#
# However a special priority of 0 marks the slave as not able to perform the
# role of master, so a slave with priority of 0 will never be selected by
# Redis Sentinel for promotion.
#
# By default the priority is 100.
slave-priority 100

从服务器的优先次序是一个整型值被Redsi在info输出时发布。
它被使用当Redis哨兵选择一个从服务器提升为主服务器如果主服务器不在正确地运行。

一个低优先值从服务器被优先考虑提升，一次如果有三个优先权分别为10,100,25的从服务器，
哨兵将选择优先权为10的那一个，他的值最低。

然而，指定优先权为0标识这个从服务器不能作为主服务器角色，因此优先权的值为0的从服务器不会
被哨兵选择。
优先权默认是100
slave-priority 100

# It is possible for a master to stop accepting writes if there are less than
# N slaves connected, having a lag less or equal than M seconds.
#
# The N slaves need to be in "online" state.
#
# The lag in seconds, that must be <= the specified value, is calculated from
# the last ping received from the slave, that is usually sent every second.
#
# This option does not GUARANTEE that N replicas will accept the write, but
# will limit the window of exposure for lost writes in case not enough slaves
# are available, to the specified number of seconds.
#
# For example to require at least 3 slaves with a lag <= 10 seconds use:
#
# min-slaves-to-write 3
# min-slaves-max-lag 10
#
# Setting one or the other to 0 disables the feature.
#
# By default min-slaves-to-write is set to 0 (feature disabled) and
# min-slaves-max-lag is set to 10.

主服务器可能停止接收写操作如果少于N个来自从服务器的连接，少于M秒的延迟
N个从服务器需要时在线状态。

以秒为单位的延迟，必须是小于等于指定的值，从最后一次接收来子从服务器的ping
开始计算，通常情况下，它每秒都会被发送。

这个选项不能保证N个从服务器接收写操作，但是将限制丢失写操作的暴露窗口以防
没有足够的可用的从服务器，在指定的时间内。

例如，在小于等于10秒内请求至少3台从服务器可以使用
min-slaves-to-write 3
min-slaves-max-lag 10


# A Redis master is able to list the address and port of the attached
# slaves in different ways. For example the "INFO replication" section
# offers this information, which is used, among other tools, by
# Redis Sentinel in order to discover slave instances.
# Another place where this info is available is in the output of the
# "ROLE" command of a masteer.
#
# The listed IP and address normally reported by a slave is obtained
# in the following way:
#
#   IP: The address is auto detected by checking the peer address
#   of the socket used by the slave to connect with the master.
#
#   Port: The port is communicated by the slave during the replication
#   handshake, and is normally the port that the slave is using to
#   list for connections.
#
# However when port forwarding or Network Address Translation (NAT) is
# used, the slave may be actually reachable via different IP and port
# pairs. The following two options can be used by a slave in order to
# report to its master a specific set of IP and port, so that both INFO
# and ROLE will report those values.
#
# There is no need to use both the options if you need to override just
# the port or the IP address.
#
# slave-announce-ip 5.5.5.5
# slave-announce-port 1234

一个Redis主服务器可以列出它的从服务器的地址可端口以不同的方式。例如，“INFO replication”章节
提供了这部分的信息，与其他工具相比，这被Redsi哨兵在搜寻从服务实例时被使用。
其他可用的信息在在“ROLE”命令输出。

一个从服务器所报告的ip和地址可通过一下方式获取：
IP：这个地址被自动探测通过检查从服务器链接到主服务器的socket的对等地址。
POrt：端口在从服务器复制握手被用于交流，通常是用于从服务器正在使用的连接列表
端口。

如果你仅仅想覆盖ip或则端口没有必要同时使用着两个选项，只选ip或端口就好了
slave-announce-ip 5.5.5.5
slave-abbounce-port 1234

################################## SECURITY ###################################

# Require clients to issue AUTH <PASSWORD> before processing any other
# commands.  This might be useful in environments in which you do not trust
# others with access to the host running redis-server.
#
# This should stay commented out for backward compatibility and because most
# people do not need auth (e.g. they run their own servers).
#
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# requirepass foobared

# Command renaming.
#
# It is possible to change the name of dangerous commands in a shared
# environment. For instance the CONFIG command may be renamed into something
# hard to guess so that it will still be available for internal-use tools
# but not available for general clients.
#
# Example:
#
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#
# It is also possible to completely kill a command by renaming it into
# an empty string:
#
# rename-command CONFIG ""
#
# Please note that changing the name of commands that are logged into the
# AOF file or transmitted to slaves may cause problems.

##################################安全########################################
要求客户端去认证在处理任何其他命令前。这是有用的在你不信任的客户端访问正在运行的
Redis服务器这样的环境中。

应该保持向后兼容，因为大多数人不需要认证（他们运行他们自己的服务器）

警告：因为Redis运行相当地快，外部用户可以尝试150K个密码每秒依靠一个好的机器
这意味这你应该使用一个很健壮的密码，否则它将很容易被破解。

requirepass foobared

可以在共享环境中改变那些危险的命令的名字。例如这个CONFIG命令可以被重命名为其他
难以被猜测的东西让它可以被内部工具使用，对其他的一般的客户端不可用

例如：
rename-commamd CONFIG b840fc02d524045429941cc15f59e41cb7be6c52

也可以完全结束这个命令通过以空字符串命名它：

rename-command CONFIG ""

请注意改变命令的名称会被记录到AOF文件长，或者传入到从服务器也许会导致一些问题。

################################### LIMITS ####################################

# Set the max number of connected clients at the same time. By default
# this limit is set to 10000 clients, however if the Redis server is not
# able to configure the process file limit to allow for the specified limit
# the max number of allowed clients is set to the current file limit
# minus 32 (as Redis reserves a few file descriptors for internal uses).
#
# Once the limit is reached Redis will close all the new connections sending
# an error 'max number of clients reached'.
#
# maxclients 10000

##################################限制###########################################

设置同一时刻的客户端最大连接数。默认这个限制值是10000个客户端，但是如果
????

一旦到达这个限制的值，Redis将给所有的新的连接的客户端发送‘max number of clients reached’

maxclients 10000

# Don't use more memory than the specified amount of bytes.
# When the memory limit is reached Redis will try to remove keys
# according to the eviction policy selected (see maxmemory-policy).
#
# If Redis can't remove keys according to the policy, or if the policy is
# set to 'noeviction', Redis will start to reply with errors to commands
# that would use more memory, like SET, LPUSH, and so on, and will continue
# to reply to read-only commands like GET.
#
# This option is usually useful when using Redis as an LRU cache, or to set
# a hard memory limit for an instance (using the 'noeviction' policy).
#
# WARNING: If you have slaves attached to an instance with maxmemory on,
# the size of the output buffers needed to feed the slaves are subtracted
# from the used memory count, so that network problems / resyncs will
# not trigger a loop where keys are evicted, and in turn the output
# buffer of slaves is full with DELs of keys evicted triggering the deletion
# of more keys, and so forth until the database is completely emptied.
#
# In short... if you have slaves attached it is suggested that you set a lower
# limit for maxmemory so that there is some free RAM on the system for slave
# output buffers (but this is not needed if the policy is 'noeviction').
#
# maxmemory <bytes>

当内存使用达到了指定的值，看么不会使用更多的内存。当达到内存指定的值，Redis将
根据选择的回收策略移除一些键值。（参考内粗策略）

如果Redis不能根据策略移除键，或者如果这个策略被设为‘noevication’(不回收)，Redis
将开始错误地应打那么些需要使用更多内存的命令，例如，SET,LPUST等等，可以继续
应答那些像GET这样的只读命令。

这个选择通常是有用的，当把Redis作为LRU缓存使用时，或者使用一个？？？

警告：如果有依附了限定了最大内存实例的从服务器，从服务器的输出缓存将依赖从服务器从已使用
的内存中缩减，？？？

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#
# volatile-lru -> remove the key with an expire set using an LRU algorithm
# allkeys-lru -> remove any key according to the LRU algorithm
# volatile-random -> remove a random key with an expire set
# allkeys-random -> remove a random key, any key
# volatile-ttl -> remove the key with the nearest expire time (minor TTL)
# noeviction -> don't expire at all, just return an error on write operations
#
# Note: with any of the above policies, Redis will return an error on write
#       operations, when there are no suitable keys for eviction.
#
#       At the date of writing these commands are: set setnx setex append
#       incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
#       sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
#       zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
#       getset mset msetnx exec sort
#
# The default is:
#
# maxmemory-policy noeviction

# LRU and minimal TTL algorithms are not precise algorithms but approximated
# algorithms (in order to save memory), so you can tune it for speed or
# accuracy. For default Redis will check five keys and pick the one that was
# used less recently, you can change the sample size using the following
# configuration directive.
#
# The default of 5 produces good enough results. 10 Approximates very closely
# true LRU but costs a bit more CPU. 3 is very fast but not very accurate.
#
# maxmemory-samples 5

############################## APPEND ONLY MODE ###############################

# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check http://redis.io/topics/persistence for more information.

appendonly no

#############################APPEND ONLY MODE#################################
Redis默认异步清除在磁盘的数据集。这种模式在很多应用场景中已经足够好了，但是Redis进程问题或者
电力中断也许会导致几分钟的写操作丢失（这取决于配置的保存点）。

文件追加模式是一种可选择的持久化模式，它提供了更好的持久性。例如使用默认的数据文件同步策略，
Redis可能只丢失一秒的写操作数据在突发情况下，例如服务器电力中断，或者Redis进程自己发生错误时
的一个写操作，但是正在操作的系统仍然可以正常运行。

AOF和RDB可以在同一时间被使用，这是完全没有问题的。
如果AOF被授权在开始时，Redis将载入AOF，这个文件有更好的持久化的保证。

appendonly no

# The name of the append only file (default: "appendonly.aof")

appendfilename "appendonly.aof"

追加模式文件名称（默认为“appendonly.aof”）
appendfilename "appendonly.aof"

# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

# appendfsync always
appendfsync everysec
# appendfsync no

fsync()方法调用告诉操作系统把数据写道磁盘上并清空而不是等待输出缓存有更多的数据。一些操作系统
将真正地将数据刷到硬盘，而另一些操作系统只是尽可能地写到磁盘。

Redis支持三种不同的模式：
no : 不同步清空，仅仅让系统在需要时清空数据。最快的模式。
alaways： 同步清除在每次写到追加日志操作后。速度慢，但是最安全。
ererysec：同步清空，每秒一次。这厮稳妥的方式。

默认时“everysec”模式，因为这是合适稳妥的在速度和数据安全方面两方面考虑。它取决你是否释放
它到“no”的模式，这将使操作系统清空输出缓存当系统需要时，为了更好的执行（但是你要承受丢失一些数据？？？）
或者相反，使用“always”模式，这种模式很慢但是比everysec安全一些。


# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.

no-appendfsync-on-rewrite no

当AOF同步清空策略被设置为always或者everysec模式时，一个后台保存进程（后台保存或者
AOF日志后台重写进程）执行大量到磁盘的I/O操作，在一些LInux配置中，Redis也许会阻塞很久
在fsync（）方法调用后。注意当前没有可修复的办法对于这样的情形，即使使用不同的线程执行
同步清空操作也将阻塞我们的同步write(2)调用。

# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes

################################ LUA SCRIPTING  ###############################

# Max execution time of a Lua script in milliseconds.
#
# If the maximum execution time is reached Redis will log that a script is
# still in execution after the maximum allowed time and will start to
# reply to queries with an error.
#
# When a long running script exceeds the maximum execution time only the
# SCRIPT KILL and SHUTDOWN NOSAVE commands are available. The first can be
# used to stop a script that did not yet called write commands. The second
# is the only way to shut down the server in the case a write command was
# already issued by the script but the user doesn't want to wait for the natural
# termination of the script.
#
# Set it to 0 or a negative value for unlimited execution without warnings.
lua-time-limit 5000

Lua脚本的最大执行时间以毫秒为单位。

如果到了最大执行时间，Redis将打印在最大执行时间后仍然在执行的脚本，并且带有一个错误
来应答请求。

当一个长的在运行的脚本超过了最大执行时间，只能SCRIPT KILL和SHUTDOWN NOSAVE命令是可用的。
第一个命令可以被用来停止一个至今没有被写命令调用的脚本。第二个命令是唯一的方式来关闭一个
通过脚本发出的写命令被发布但用户不想等到脚本自然终止的服务器。

设置它为0或者一个负值不限制不带警告的执行
lua-time-limit 5000



################################ REDIS CLUSTER  ###############################
#
# ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
# WARNING EXPERIMENTAL: Redis Cluster is considered to be stable code, however
# in order to mark it as "mature" we need to wait for a non trivial percentage
# of users to deploy it in production.
# ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
#
# Normal Redis instances can't be part of a Redis Cluster; only nodes that are
# started as cluster nodes can. In order to start a Redis instance as a
# cluster node enable the cluster support uncommenting the following:
#
# cluster-enabled yes

########################## redis 集群 ###################################
#+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
#  实验性警告：redis集群被当作稳定的代码，然后，为了使它成熟需要等待大多数的用户在生产环境部署它。
#++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

通常，redis实例并不是redis集群的一部分；只有那些作为集群结点的才是。为了启动一个redis实例，且作为
集群结点我们需要取消下面的注释
#cluster-enabled yes

# Every cluster node has a cluster configuration file. This file is not
# intended to be edited by hand. It is created and updated by Redis nodes.
# Every Redis Cluster node requires a different cluster configuration file.
# Make sure that instances running in the same system do not have
# overlapping cluster configuration file names.
#
# cluster-config-file nodes-6379.conf
每一个集群结点都有一个集群配置文件。这个文件并不打算被手工编写。它被redis结点建立，更新。
每一个redis集群结点都要求一个不同的集群配置文件。为了确保运行在同一个系统中的redis实例不会有相互
覆盖的集群配置文件，命名如下
#cluster-config-file node-6379.conf

# Cluster node timeout is the amount of milliseconds a node must be unreachable
# for it to be considered in failure state.
# Most other internal time limits are multiple of the node timeout.

# cluster-node-timeout 15000

#集群结点超时是一个结点不可达的毫秒总数，它被认为是失败（不可用）的状态
# 大部分的其他结点内部时间限制是多数结点超时（多个结点连接该结点都超时，即认为该结点挂掉了）
cluster-node-timeout 15000

# A slave of a failing master will avoid to start a failover if its data
# looks too old.
#一个失效的主节点的从节点将避免开启失效备援如果它的数据太久了。
#
# There is no simple way for a slave to actually have a exact measure of
# its "data age", so the following two checks are performed:
#
# 1) If there are multiple slaves able to failover, they exchange messages
#    in order to try to give an advantage to the slave with the best
#    replication offset (more data from the master processed).
#    Slaves will try to get their rank by offset, and apply to the start
#    of the failover a delay proportional to their rank.
#
# 2) Every single slave computes the time of the last interaction with
#    its master. This can be the last ping or command received (if the master
#    is still in the "connected" state), or the time that elapsed since the
#    disconnection with the master (if the replication link is currently down).
#    If the last interaction is too old, the slave will not try to failover
#    at all.

# 没有简单的方式让从节点能够经常地检测它的“数据年龄”，所以，下面两种检查将被执行
# 1） 如果有多个从节点能够失效备援，它们将相互交换信息，试着给有最好的复制补偿的从节点一个建议
#     （大部分是主节点处理的数据）从节点根据offset（补偿）排序，然后开始根据它们的顺序经行失效
		备援（不知如何精确翻译，只能意译了）

# 2) 每一个单独的从节点计算上一次与主节点交互的时间。它可以使最近一次ping或者接受命令（如果这个主
	 节点仍然是连接状态），后者自从与主节点断开连接的时间（如果当前复制连接已经关闭）。如果最近与主节点
	 交互的时间太久了，从节点就不会再失效备援（故障转移，其实就是从节点与主节点断开连接时间太长了，就不再
	 备份主节点数据了）
#
# The point "2" can be tuned by user. Specifically a slave will not perform
# the failover if, since the last interaction with the master, the time
# elapsed is greater than:
#
#   (node-timeout * slave-validity-factor) + repl-ping-slave-period

 第二点可以被用户调节。特别地，如果一个从节点不失效备援，从最近与主节点交互开始，消逝的时间比下面的时间
 更长
#  (node-timeout * slave-validity-factor) + repl-ping-slave-period（这是一个计算公式）


#
# So for example if node-timeout is 30 seconds, and the slave-validity-factor
# is 10, and assuming a default repl-ping-slave-period of 10 seconds, the
# slave will not try to failover if it was not able to talk with the master
# for longer than 310 seconds.

# 例如如果节点的超时时间是30秒，并且这个slave-validity-factor 是10秒，且假定默认repl-ping-slave-period 
  是10秒，那么从节点将不会失效备援，如果与主节点不能够交互的时间不超过310秒
  30*10+10s  根据上面的公式计算得出
#
# A large slave-validity-factor may allow slaves with too old data to failover
# a master, while a too small value may prevent the cluster from being able to
# elect a slave at all.
# 一个大的slave-validity-factor 允许那些存储太旧的数据的节点备援一个主节点，当是一个太小的值	
  那么集群就不能选择一个从节点了（这里没有说明选择这个从节点干什么，应该是当作主节点）
#
# For maximum availability, it is possible to set the slave-validity-factor
# to a value of 0, which means, that slaves will always try to failover the
# master regardless of the last time they interacted with the master.
# (However they'll always try to apply a delay proportional to their
# offset rank).


#
# Zero is the only value able to guarantee that when all the partitions heal
# the cluster will always be able to continue.
#
# cluster-slave-validity-factor 10

# Cluster slaves are able to migrate to orphaned masters, that are masters
# that are left without working slaves. This improves the cluster ability
# to resist to failures as otherwise an orphaned master can't be failed over
# in case of failure if it has no working slaves.
# 
#
# Slaves migrate to orphaned masters only if there are still at least a
# given number of other working slaves for their old master. This number
# is the "migration barrier". A migration barrier of 1 means that a slave
# will migrate only if there is at least 1 other working slave for its master
# and so forth. It usually reflects the number of slaves you want for every
# master in your cluster.
#
# Default is 1 (slaves migrate only if their masters remain with at least
# one slave). To disable migration just set it to a very large value.
# A value of 0 can be set but is useful only for debugging and dangerous
# in production.
#
# cluster-migration-barrier 1

# By default Redis Cluster nodes stop accepting queries if they detect there
# is at least an hash slot uncovered (no available node is serving it).
# This way if the cluster is partially down (for example a range of hash slots
# are no longer covered) all the cluster becomes, eventually, unavailable.
# It automatically returns available as soon as all the slots are covered again.
#
# However sometimes you want the subset of the cluster which is working,
# to continue to accept queries for the part of the key space that is still
# covered. In order to do so, just set the cluster-require-full-coverage
# option to no.
#
# cluster-require-full-coverage yes

# 默认地当redis集群节点停止接受查询当他们发现至少一个不能覆盖的哈希槽（没有可用的节点再槽位上）
# 集群中某部分挂掉了（例如一写哈希槽位没有覆盖）所有的集群最终变成不可用的。当槽位再次覆盖后，集群
# 集群自动恢复为可用的。

# 然而，某些情况下你想让集群的子集是可用的，继续接受查询对于那些部分被覆盖的key值，为了达到这样的目的，
# 只需要把cluster-require-full-coverage选项设置为no
# cluster-require-full-coverage yes（默认为yes）

# In order to setup your cluster make sure to read the documentation
# available at http://redis.io web site.

################################## SLOW LOG ###################################

# The Redis Slow Log is a system to log queries that exceeded a specified
# execution time. The execution time does not include the I/O operations
# like talking with the client, sending the reply and so forth,
# but just the time needed to actually execute the command (this is the only
# stage of command execution where the thread is blocked and can not serve
# other requests in the meantime).
#
# You can configure the slow log with two parameters: one tells Redis
# what is the execution time, in microseconds, to exceed in order for the
# command to get logged, and the other parameter is the length of the
# slow log. When a new command is logged the oldest one is removed from the
# queue of logged commands.

# The following time is expressed in microseconds, so 1000000 is equivalent
# to one second. Note that a negative number disables the slow log, while
# a value of zero forces the logging of every command.
slowlog-log-slower-than 10000

# There is no limit to this length. Just be aware that it will consume memory.
# You can reclaim memory used by the slow log with SLOWLOG RESET.
slowlog-max-len 128

################################ LATENCY MONITOR ##############################

# The Redis latency monitoring subsystem samples different operations
# at runtime in order to collect data related to possible sources of
# latency of a Redis instance.
#
# Via the LATENCY command this information is available to the user that can
# print graphs and obtain reports.
#
# The system only logs operations that were performed in a time equal or
# greater than the amount of milliseconds specified via the
# latency-monitor-threshold configuration directive. When its value is set
# to zero, the latency monitor is turned off.
#
# By default latency monitoring is disabled since it is mostly not needed
# if you don't have latency issues, and collecting data has a performance
# impact, that while very small, can be measured under big load. Latency
# monitoring can easily be enabled at runtime using the command
# "CONFIG SET latency-monitor-threshold <milliseconds>" if needed.
latency-monitor-threshold 0

############################# EVENT NOTIFICATION ##############################

# Redis can notify Pub/Sub clients about events happening in the key space.
# This feature is documented at http://redis.io/topics/notifications
#
# For instance if keyspace events notification is enabled, and a client
# performs a DEL operation on key "foo" stored in the Database 0, two
# messages will be published via Pub/Sub:
#
# PUBLISH __keyspace@0__:foo del
# PUBLISH __keyevent@0__:del foo
#
# It is possible to select the events that Redis will notify among a set
# of classes. Every class is identified by a single character:
#
#  K     Keyspace events, published with __keyspace@<db>__ prefix.
#  E     Keyevent events, published with __keyevent@<db>__ prefix.
#  g     Generic commands (non-type specific) like DEL, EXPIRE, RENAME, ...
#  $     String commands
#  l     List commands
#  s     Set commands
#  h     Hash commands
#  z     Sorted set commands
#  x     Expired events (events generated every time a key expires)
#  e     Evicted events (events generated when a key is evicted for maxmemory)
#  A     Alias for g$lshzxe, so that the "AKE" string means all the events.
#
#  The "notify-keyspace-events" takes as argument a string that is composed
#  of zero or multiple characters. The empty string means that notifications
#  are disabled.
#
#  Example: to enable list and generic events, from the point of view of the
#           event name, use:
#
#  notify-keyspace-events Elg
#
#  Example 2: to get the stream of the expired keys subscribing to channel
#             name __keyevent@0__:expired use:
#
#  notify-keyspace-events Ex
#
#  By default all notifications are disabled because most users don't need
#  this feature and the feature has some overhead. Note that if you don't
#  specify at least one of K or E, no events will be delivered.
notify-keyspace-events ""

############################### ADVANCED CONFIG ###############################

# Hashes are encoded using a memory efficient data structure when they have a
# small number of entries, and the biggest entry does not exceed a given
# threshold. These thresholds can be configured using the following directives.
hash-max-ziplist-entries 512
hash-max-ziplist-value 64

# Lists are also encoded in a special way to save a lot of space.
# The number of entries allowed per internal list node can be specified
# as a fixed maximum size or a maximum number of elements.
# For a fixed maximum size, use -5 through -1, meaning:
# -5: max size: 64 Kb  <-- not recommended for normal workloads
# -4: max size: 32 Kb  <-- not recommended
# -3: max size: 16 Kb  <-- probably not recommended
# -2: max size: 8 Kb   <-- good
# -1: max size: 4 Kb   <-- good
# Positive numbers mean store up to _exactly_ that number of elements
# per list node.
# The highest performing option is usually -2 (8 Kb size) or -1 (4 Kb size),
# but if your use case is unique, adjust the setting
