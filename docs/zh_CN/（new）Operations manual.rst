第一章 前言
==========

1.1 版本声明
-----------

本文档著作权归趣链科技单独所有，未经趣链科技事先书面许可，任何主体不得以任何形式复制、修改、抄袭、传播全部或部分本文档内容。

1.2 服务声明
-----------

本文档意在向客户介绍趣链科技区块链底层平台的运维说明。您所购买的趣链科技的产品、服务的种类、服务标准等应由您与趣链科技之间的商业合同约定，除非双方另有约定，否则，趣链科技对本文档内容不做任何明示或模式的承诺或保证。

第三章 配置管理
==============

3.1 配置文件说明
---------------

> 配置文件中各配置项的含义及默认值请参考附录。

3.1.1 global.toml
^^^^^^^^^^^^^^^^^

在该配置文件中记录了一些 **全局的、无法动态修改的** 配置项，包括输出日志等级、输出日志文件的路径、RPC配置、流量控制参数等：


1. ``title = "flato global configurations"``
2. ``commercialVersion = "2.0"``
3. ``##########################################################``
4. ``#``
5. ``# Logger section``
6. ``# config log's level by module``
7. ``# CRITICAL ERROR WARNING NOTICE INFO DEBUG``
8. ``# high <------------- log level -------> low``
9. ``#``
10. ``##########################################################``

11. ``[log]``
12. ``dump_file           = true # dump the log file or not``
13. ``dump_interval       = "24h"  # Valid time units are "ns", "us" (or "µs"), "ms", "s", "m", "h". such as "300ms", "2h45m".``
14. ``log_dir             = "./logs"``
15. ``log_level           = "NOTICE" # default loglevel for all modules which can be override by module level log setting``
16. ``file_format         = "[%{module}][%{level:.5s}] %{time:15:04:05.000} %{shortfile} %{message}"``
17. ``console_format      = "%{color}[%{module}][%{level:.5s}] %{time:15:04:05.000} %{shortfile} %{message} %{color:reset}"``
18. ``max_log_size        = "200mb"  # "mb", "kb"``
19. ``check_size_interval = "2m"``

20. ``[log.module] #set log level by module``
21. ``p2p         = "NOTICE"``
22. ``consensus   = "INFO"``
23. ``core        = "NOTICE"``
24. ``flatodb     = "NOTICE" ``
25. ``eventhub    = "NOTICE"``
26. ``jsonrpc     = "NOTICE"``
27. ``hypernet    = "NOTICE"``
28. ``audit       = "NOTICE"``


29. ``[audit]``
30. ``backend				= "file" # "file" or "filelog" or graylog" or "elk"``
31. ``level         		= "none" # none, metadata, responsebody, storageobject``
32. ``queue_size          = 4096``
33. ``rewrite_interval    = "2s"``
34. ``update_conn_interval = "2s"``
35. ``[audit.conn.pool]``
36. ``#urls             = ["172.16.3.1:12201", "172.16.3.2:12201", "172.16.3.3:12201", "172.16.3.4:12201"]  #graylog tcp conn urls``
37. ``urls             = ["172.16.5.5:12202"]    #filebeat tcp conn url``
38. ``init_cap         = 1``
39. ``max_cap          = 5``
40. ``idle_timeout     = "60s"``
41. ``dial_timeout     = "3s"``
42. ``[audit.file]               #filelog config``
43. ``encrypt = false``
44. ``type="filelog"``
45. ``[audit.file.filelog]``
46. ``path = "system/auditlog/filelog/"``
47. ``compression = "snappy" #zlib pure snappy``
48. ``max_log_file_size = "100mb" # "mb", "kb"``
49. ``data_version = 1 # default to be 1, means disk storage struct version``
50. ``index_enable = true``
51. ``[audit.file.filelog.cache]``
52. ``enable = true``
53. ``max_cache_size = 100 #mb``
54. ``cache_expired_time = 48 #hour``
55. ``cache_entry_num = 20``

56. ``[audit.file.filelog.handler_cache]``
57. ``handler_cache_num = 100``
58. ``handler_cache_evict_time = 1 # *time.Second``


59. ``##########################################################``
60. ``#``
61. ``#  JSONRPC section``
62. ``#``
63. ``##########################################################``
64. ``[http]``
65. ``# allowedOrigins should be a comma-separated list of allowed origin URLs``
66. ``# to allow connections with any origin, pass "*".``
67. ``allowedOrigins=["*"]``
68. ``# if true, it will enable secure http connection(https).``
69. ``security    = false``
70. ``# if true, use http/2, otherwise use http/1.1.``
71. ``# WARN: if version_2 is true, option sercurity must be true, otherwise use default http/1.1 without https.``
72. ``http_2   = false``
73. ``#Path to tlsca for rpc``
74. ``tlsCA                   = "./tls/tlsca.ca"``
75. ``#Path to tlscert for rpc``
76. ``tlsCert                 = "./tls/tls_peer.cert"``
77. ``#Path to private key of tlscert for rpc``
78. ``tlsCertPriv             = "./tls/tls_peer.priv"``
79. ``[http.request]``
80. ``max_content_length = "100kb" # default 100kB``
81. ``read_timeout = "1000s"      # default 1000s``

82. ``##########################################################``
83. ``#``
84. ``# P2P section``
85. ``#``
86. ``##########################################################``
87. ``[p2p]``
88. ``transport               = "grpc" # we now only support "grpc"``
89. ``compress                = false  # enable compression and decompression of network message``

90. ``# network mode, enum: direct, relay, discover. direct is default mode.``
91. ``# discover mode not require full hosts.toml.``
92. ``# relay mode not require direct node.``
93. ``mode                    = "direct"``

94. ``retrytime               = "3s"``
95. ``ipc                     = "hpc_1.ipc"``
96. ``enableTLS               = true``
97. ``tlsCA                   = "./tls/tlsca.ca"``
98. ``tlsServerHostOverride   = "hyperchain.cn"``
99. ``tlsCert                 = "./tls/tls_peer.cert"``
100. ``tlsCertPriv             = "./tls/tls_peer.priv"``

101. ``# connection control configurations``
102. ``# keepalive configuration``
103. ``# [1,5,10] means minLimit=1, threshold=5 maxLimit=20``
104. ``keepAliveDuration = [1,5,20]``

105. ``# pending configuration``
106. ``# [5,20,100] means minLimit=1, threshold=20 maxLimit=100``
107. ``pendingDuration = [5,20,100]``

108. ``# send keepalive message every keepAliveInterval``
109. ``keepAliveInterval = "5s"``

110. ``# stream will be closed if not receive keepalive message for keepAliveTimeout``
111. ``keepAliveTimeout = "3m"``

112.     ``[p2p.grpc]``
113.     ``maxRecvMessageSize = "50mb" # default 50mb``
114.     ``maxSendMessageSize = "50mb" # default 50mb``

115. ``##########################################################``
116. ``#``
117. ``# flow control configurations section``
118. ``#``
119. ``##########################################################``
120. ``[flow.control.ratelimit]``
121. ``enable            = true``
122. ``fileReceivePeak   = 100``
123. ``fileReceiveRate	  = "8ms"``
124. ``fileSendPeak	  = 100``
125. ``fileSendRate      = "8ms"``

126. ``[flow.control.bandwidth]``
127. ``enable            = false``
128. ``outgoingBandwidth = "500Mb" # NOTE: The unit is Mb/s NOT MB/s``


其中log模块表示日志相关的配置选项，您可以通过修改log.dump_file来控制是否将输出日志重定向至日志文件中， **我们推荐您开启日志重定向** 。

您可以根据实际申请开放的端口号进行port模块的配置，其中grpc端口是节点间通信的端口号，注意要与addr.toml中的端口号对应；jsonrpc端口是外部应用向hyperchain平台发送请求使用的端口号。

您可以通过修改flow.control.ratelimit.enable和flow.control.bandwidth的值来控制是否开启节点流量控制和带宽限制，建议根据测试的tps进行流控设置，详细的流控配置可参考3.2节内容。

3.1.2 dynamic.toml
^^^^^^^^^^^^^^^^^^

该配置文件包含了一些 **全局的，可运行时动态修改的配置项：**

1. ``self = "node1"``

2. ``##########################################################``
3. ``#``
4. ``# key ports section``
5. ``#``
6. ``##########################################################``
7. ``[port]``
8. ``jsonrpc     = 8081``
9. ``grpc        = 50011 # p2p``

10. ``##########################################################``
11. ``#``
12. ``# p2p system config``
13. ``# 1. define the remote peer's hostname and its IP address``
14. ``# 2. define self address list under different domain``
15. ``#``
16. ``##########################################################``
17. ``[p2p]``
18. 	``[p2p.ip.remote]``
18. 		``# this node will connect to those peer, if here has self hostname, we will ignore it``
20. 		``hosts = [``
21. 		 ``"node1 127.0.0.1:50011",``
22. 		 ``"node2 127.0.0.1:50012",``
23. 		 ``"node3 127.0.0.1:50013",``
24. 		 ``"node4 127.0.0.1:50014",``
25. 	    ``]``

26. 	``[p2p.ip.self]``
27. 	    ``domain = "domain1"``

28. 	    ``# addr is (domain,endpoint) pair, those items defined the ip address:port which``
29. 	    ``# other domains' host how connect to self``
30. 	    ``addrs = [``
31.	     ``"domain1 127.0.0.1:50011",``
32. 	     ``"domain2 127.0.0.1:50011",``
33.	     ``"domain3 127.0.0.1:50011",``
34. 	     ``"domain4 127.0.0.1:50011",``
35. 	    ``]``
36. ``#这里配置时候需要注意,配置的是其他节点访问本节点时，使用的本节点的IP地址，举个例子，如果节点2属于域 `domain2` ，那么节点2访问节点1时需要用节点1声明的在 `domain2` 域中对外暴露的地址，换句话说，节点2访问本节点时用的地址是 `127.0.0.1:50012` 。``

37. ``[[namespace]]``
38.     ``name = "global"``
39. 	``start = true``

您可以根据实际申请开放的端口号进行port模块的配置，其中grpc端口是节点间通信的端口号，注意要与下方[p2p.ip.remote.hosts]中的端口号对应；jsonrpc端口是外部应用向Flato平台发送请求使用的端口号。

**domain的配置是比较容易出错的地方，最简单的配置方式就是**：

- 所有节点都在一个domain里：所有节点都在同一个内网环境，只要配置一个domain和该节点在这个domain里的IP地址即可。

namespace模块指定了namespace的根目录路径以及节点启动时默认参与的namespace名称， **我们建议每个节点都要默认启动global这个namespace** 。

3.1.3 ns_dynamic.toml
^^^^^^^^^^^^^^^^^^^^^

该配置文件中记录了 **namespace级别的可动态修改的配置项** ，包括当前节点的启动方式、启动身份、区块链网络节点数目以及每个节点的网络配置信息。您在使用之前必须确保所有的网络配置正确。节点启动的时候会 **检查该配置文件的可用性** ，比如 `nodes` 列表中不能有重复的hostname、 `self.n` 必须等于 `nodes` 列表项目数，平台通过检查网络配置文件的可用性，可以让应用开发者及时发现配置异常。

1. ``[consensus]``
2. ``algo = "RBFT"``
3.     ``[consensus.set]``
4.     ``set_size       = 25    # How many transactions should the node broadcast at once``
5.     ``[consensus.pool]``
6.     ``batch_size       = 500    # How many txs should the primary pack before sending pre-prepare``
7.     ``pool_size        = 50000  # How many txs could the txPool stores in total``

8. ``[self]``
9. ``n         = 4           # 运行时修改。表示所连vp节点的个数，该值在节点运行过程中会实时变化。``
10. ``hostname    = "node1"   # 运行时修改，仅限于CVP节点。对于cvp来说，该值会发生变化，仅在cvp节点升级为vp的时候，这里的hostname会被替换为要升级vp的hostname。``
11. ``new         = false     # 运行时修改。新节点成功加入网络以后，该值会变为false。``
12. ``# the value can only be vp、nvp and cvp, case-insensitive``
13. ``type        = "vp"	    # 候选项为vp/nvp/cvp``
14. ``vp          = true      # （过时的无效配置）``

15. ``#[[cvps]]				# 运行时修改。cvps在节点运行过程中实时变化。``
16. ``#hostname 	= "cvp1"``

17. ``#[[cvps]]``
18. ``#hostname 	= "cvp2"``

19. ``#[[nvps]]				# 运行时修改。nvps数组在节点运行过程中实时变化。``
20. ``#hostname	= "nvp1"``

21. ``#[[nvps]]``				
22. ``#hostname	= "nvp2"``

23. ``[[nodes]]				# 运行时修改。nodes数组在节点运行过程中实时变化。``
24. ``hostname    = "node1"``
25. ``score       = 10``

26. ``[[nodes]]``
27. ``hostname    = "node2"``
28. ``score       = 10``

29. ``[[nodes]]``
30. ``hostname    = "node3"``
31. ``score       = 10``

32. ``[[nodes]]``
33.``hostname    = "node4"``
34. ``score       = 10``

可查询附录了解更多配置项信息。
