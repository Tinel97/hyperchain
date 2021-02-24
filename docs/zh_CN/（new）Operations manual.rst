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
18. ``[p2p.ip.remote]``
19. ``# this node will connect to those peer, if here has self hostname, we will ignore it``
20. ``hosts = [``
21. ``"node1 127.0.0.1:50011",``
22. ``"node2 127.0.0.1:50012",``
23. ``"node3 127.0.0.1:50013",``
24. ``"node4 127.0.0.1:50014",``
25. ``]``

26. ``[p2p.ip.self]``
27. ``domain = "domain1"``

28. ``# addr is (domain,endpoint) pair, those items defined the ip address:port which``
29. ``# other domains' host how connect to self``
30. ``addrs = [``
31. ``"domain1 127.0.0.1:50011",``
32. ``"domain2 127.0.0.1:50011",``
33. ``"domain3 127.0.0.1:50011",``
34. ``"domain4 127.0.0.1:50011",``
35. ``]``
36. ``#这里配置时候需要注意,配置的是其他节点访问本节点时，使用的本节点的IP地址，举个例子，如果节点2属于域 `domain2` ，那么节点2访问节点1时需要用节点1声明的在 `domain2` 域中对外暴露的地址，换句话说，节点2访问本节点时用的地址是 `127.0.0.1:50012` 。``

37. ``[[namespace]]``
38. ``name = "global"``
39. ``start = true``

您可以根据实际申请开放的端口号进行port模块的配置，其中grpc端口是节点间通信的端口号，注意要与下方[p2p.ip.remote.hosts]中的端口号对应；jsonrpc端口是外部应用向Flato平台发送请求使用的端口号。

**domain的配置是比较容易出错的地方，最简单的配置方式就是**：

- 所有节点都在一个domain里：所有节点都在同一个内网环境，只要配置一个domain和该节点在这个domain里的IP地址即可。

namespace模块指定了namespace的根目录路径以及节点启动时默认参与的namespace名称， **我们建议每个节点都要默认启动global这个namespace** 。

3.1.3 ns_dynamic.toml
^^^^^^^^^^^^^^^^^^^^^

该配置文件中记录了 **namespace级别的可动态修改的配置项** ，包括当前节点的启动方式、启动身份、区块链网络节点数目以及每个节点的网络配置信息。您在使用之前必须确保所有的网络配置正确。节点启动的时候会 **检查该配置文件的可用性** ，比如 `nodes` 列表中不能有重复的hostname、 `self.n` 必须等于 `nodes` 列表项目数，平台通过检查网络配置文件的可用性，可以让应用开发者及时发现配置异常。

1. ``[consensus]``
2. ``algo = "RBFT"``
3. ``[consensus.set]``
4. ``set_size       = 25    # How many transactions should the node broadcast at once``
5. ``[consensus.pool]``
6. ``batch_size       = 500    # How many txs should the primary pack before sending pre-prepare``
7. ``pool_size        = 50000  # How many txs could the txPool stores in total``

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
33. ``hostname    = "node4"``
34. ``score       = 10``

可查询附录了解更多配置项信息。

3.1.4 ns_static.toml
^^^^^^^^^^^^^^^^^^^^

该配置文件中记录了所有namespace级别的配置项，包括**共识算法配置、加密证书的配置、数据库相关配置、日志等级配置**等等，这些配置都是**无法在运行中修改**的，各个配置项的释义在注释以及文末的附录中给出。

1. `` ###########################################################``
2. `` #  _   _                        ____ _           _        #``
3. `` # | | | |_   _ _ __   ___ _ __ / ___| |__   __ _(_)_ __   #``
4. `` # | |_| | | | | '_ \ / _ \ '__| |   | '_ \ / _\ | | '_ \  #``
5. `` # |  _  | |_| | |_) |  __/ |  | |___| | | | (_| | | | | | #``
6. `` # |_| |_|\__, | .__/ \___|_|   \____|_| |_|\__,_|_|_| |_| #``
7. `` #        |___/|_|                                         #``
8. `` ###########################################################``
9. `` title = "namespace configurations"``

10. ``[genesis]``
11. ``[genesis.alloc]``
12. ``"000f1a7a08ccc48e5d30f80850cf1cf283aa3abd" = "1000000000"``
13. ``"e93b92f1da08f925bdee44e91e7768380ae83307" = "1000000000"``
14. ``"6201cb0448964ac597faf6fdf1f472edf2a22b89" = "1000000000"``
15. ``"b18c8575e3284e79b92100025a31378feb8100d6" = "1000000000"``
16. ``"856E2B9A5FA82FD1B031D1FF6863864DBAC7995D" = "1000000000"``
17. ``"fbca6a7e9e29728773b270d3f00153c75d04e1ad" = "1000000000"``

18. ``############################################################################################################``
19. ``# consensus algorithm configuration section``
20. ``#``
21. ``# 1. choose which consensus algorithm to use``
22. ``# 2. define the algorithm related configurations``
23. ``#``
24. ``############################################################################################################``
25. ``[consensus]``
26. ``[consensus.set]``
27. ``# set_size       = 25    # How many transactions should the node broadcast at once``
28. ``set_timeout    = "0.1s"# Node broadcasts transactions if there are cached transactions, although set_size isn't reached yet``

29. ``[consensus.pool]``
30. ``# batch_size       = 500    # How many txs should the primary pack before sending pre-prepare``
31. ``# pool_size        = 50000  # How many txs could the txPool stores in total``
32. ``check_interval  = "3m"  # interval of the check loop``
33. ``tolerance_time  = "5m"  # the max tolerance time, if time.Now - timestamp > toleranceTime, send event to consensus``
34. ``batch_mem_limit  = false # If Limit the batch memory size or not``
35. ``batch_max_mem    = "10mb" # The maximum memory size of one batch``

36. ``[consensus.rbft]         # rbft configurations``
37. ``k                = 10    # After how many blocks a checkpoint will be sent``
38. ``vc_period        = 0     # After how many checkpoint periods( Blocks = 10 * vcperiod ) the primary gets cycled automatically. ( Set 0 to disable )``

39. ``[consensus.rbft.timeout]``
40. ``sync_state        = "1s"  # How long to wait quorum sync state response``
41. ``sync_interval     = "10s"  # How long to restart sync state process``
42. ``recovery          = "10s" # How long to wait before recovery finished``
43. ``first_request     = "30s" # How long to wait before first request should come``
44. ``batch             = "0.5s"# Primary send a pre-prepare if there are pending requests, although batchsize isn't reached yet,``
45. ``request           = "6s"  # How long may a request(transaction batch) take between reception and execution, must be greater than the batch timeout``
46. ``validate          = "1s"  # How long may a validate (transaction batch) process will take by local Validation``
47. ``null_request      = "9s"  # Primary send it to inform aliveness, must be greater than request timeout``
48. ``viewchange        = "8s"  # How long may a view change take``
49. ``resend_viewchange = "10s" # How long to wait for a view change quorum before resending (the same) view change``
50. ``clean_viewchange  = "60s" # How long to clean out-of-data view change message``
51. ``fetch_checkpoint  = "5s"  # How long to wait for config change checkpoint quorum before fetch checkpoint take``

52. ``[consensus.raft]        # RAFT configurations``
53. ``snap_count        = 20 # How many entries should trigger a snapshot``
54. ``catchup_count = 20 # when doing log compaction, keep some entries in memory for slow followers to catchup``

55. ``[consensus.raft.timeout]``
56. ``batch        = "0.5s"  # Node make a batch if there are pending requests, although batchsize isn't reached yet``
57. ``set          = "0.1s" # Node broadcasts transactions if there are cached transactions, although set_size isn't reached yet``
58. ``fetch  = "3s" # How long to fetch a missing batch``
59. ``negotiate = "6s" # How long to wait for quorum responses after send negotiate``

60. ``[consensus.raft.dir]``
61. ``snap = "data/raft/snap" # snapshot dir``
62. ``wal  = "data/raft/wal"  # wal dir``

63. ``[consensus.solo.timeout]
64. ``batch             = "0.5s"# Node send a batch if there are pending requests, although batchsize isn't reached yet``
65. ``set               = "0.1s"# Node broadcasts transactions if there are cached transactions, although set_size isn't reached yet``


66. ``[encryption]``
67. ``[encryption.TEE]``
68. ``TEEPath = "http://nexus.hyperchain.cn/repository/arch/sgx/enclave.sign.so"``
69. ``DataEncrypt = false``

70. ``[encryption.root]``
71. ``ca    = "certs/CA"``

72. ``[encryption.ecert]``
73. ``ecert  = "certs/certs"``

74. ``[encryption.bcert]``
75. ``# path of the back-up permission certs``
76. ``listDir = "certs/bcerts"``

77. ``[encryption.check]``
78. ``enable     = true   #enable RCert``
79. ``enableT    = false  #enable TCert``

80. ``[distributedCA]``
81. ``enable = false  #true is aco mode``

82. ``[encryption.security]``
83. ``algo   = "sm4"   # Selective symmetric encryption algorithm (pure,3des,aes or sm4)``


84. ``############################################################################################################``
85. ``#``
86. ``# system upgrade configuration``
87. ``#``
88. ``############################################################################################################``

89. ``[self-government.upgrade]``
90. ``archive_path = "upgrade_archive"``
91. ``archive_prefix = "upgrade_"``
92. ``fetch_remote = true``
93. ``fetch_path = "http://127.0.0.1:8000/hyperchain-deploy.tar.gz"``

94. ``[[self-government.nodes]]``
95. ``hostname    = "node1"``
96. ``address     = "14f14e4e316ed13c41eedd4759d79d07aac775ac"``

97. ``[[self-government.nodes]]``
98. ``hostname    = "node2"``
99. ``address     = "916a3c39f7c0f579c9df5b9931af49a899c5bb4a"``

100. ``[[self-government.nodes]]``
101. ``hostname    = "node3"``
102. ``address     = "be79466f637b4c18de25f0312c5961af66fb2d3d"``

103. ``[[self-government.nodes]]``
104. ``hostname    = "node4"``
105. ``address     = "e593460a7c31f9984deae6529a57229285bd0370"``

106. ``############################################################################################################``
107. ``#``
108. ``# radar configurations``
109. ``#``
110. ``############################################################################################################``

111. ``[radar-params]``
112. ``viewable_db_type = "mysql"``
113. ``mysql_connURL  = "root:root@tcp(127.0.0.1:3306)/%s?charset=utf8"``
114. ``############################################################################################################``
115. ``#``
116. ``# executor configuration section``
117. ``# config log's level by module``
118. ``# CRITICAL ERROR WARNING NOTICE INFO DEBUG``
119. ``# high <------------- log level -------> low``
120. ``#``
121. ``############################################################################################################``
123. ``[log]``
124. ``dump_file           = true # dump the log file or not``
125. ``dump_interval       = "24h"  # Valid time units are "ns", "us" (or "µs"), "ms", "s", "m", "h". such as "300ms", "2h45m".``
126. ``log_dir             = "data/logs"``
127. ``log_level           = "NOTICE" # default loglevel for all modules which can be override by module level log setting``
128. ``file_format         = "[%{module}][%{level:.5s}] %{time:15:04:05.000} %{shortfile} %{message}"``
129. ``console_format      = "%{color}[%{module}][%{level:.5s}] %{time:15:04:05.000} %{shortfile} %{message} %{color:reset}"``
130. ``max_log_size        = "200mb"  # "mb", "kb"``
131. ``check_size_interval = "2m"``

132. ``[log.module] #set log level by module``
133. ``p2p         = "NOTICE"``
134. ``consensus   = "INFO"``
135. ``flatodb     = "NOTICE"``
136. ``eventhub    = "NOTICE"``
137. ``executor    = "NOTICE"``
138. ``execmgr     = "NOTICE"``
139. ``syncmgr     = "NOTICE"``
140. ``filemgr     = "NOTICE"``
141. ``buckettree  = "NOTICE"``
142. ``radar       = "NOTICE"``
143. ``mq          = "INFO"``
144. ``private     = "INFO"``
145. ``midware     = "NOTICE"``
146. ``dispatch    = "NOTICE"``
147. ``node        = "NOTICE"``
148. ``api         = "NOTICE"``
149. ``nvp         = "NOTICE"``

150. ``[rpc.qps.flowCtrl]``
151. ``enable   = true``
152. ``capacity = 100``
153. ``limit    = 2000 # qps``

154. ``[duplicate]``
155. ``# tx generated in the past tx_active_time is legal``
156. ``tx_active_time    = "24h"``
157. ``# The transaction's timestamp can only be greater than the current time by up to tx_drift_time``
158. ``tx_drift_time     = "5m"``

159. ``[duplicate.bloomfilter] # bloom filter used in tx duplication checking``
160. ``# each bloomfilter's size``
161. ``bloombit          = 100000000``
162. ``# Bloom filter's total memory limit``
163. ``max_mem  = "100mb"``

164. ``#[[service]]``
165. ``#    service_name = "Radar"``
166. ``#[[service]]``
167. ``#    service_name = "MQ"``

168. ``[mq.broker]``
169. ``type = "rabbit"``
170. ``#type = "kafka"``

171. ``[mq.rabbit]``
172. ``url = "amqp://guest:guest@127.0.0.1:5672/"``

173. ``[mq.kafka]``
174. ``urls				= ["localhost:9092"]``
175. ``writerBatchSize		= 100``
176. ``writerBatchBytes	= 52428800``
177. ``writerBatchTimeout 	= "0.1s"``
178. ``writeTimeout 		= "10s"``
179. ``rebalanceInterval 	= "10s"``
180. ``idleConnTimeout 	= "10s"``

181. ``[mq.kafka.partitionNums]``
182. ``"localhost:9092" = 1``

183. ``[mq.kafka.replicaFactor]``
184. ``"localhost:9092" = 1``


185. ``############################################################################################################``
186. ``#``
187. ``# private protection configuration section``
188. ``#``
189. ``############################################################################################################``
190. ``[private]``
191. ``cache_size    = 500``

192. ``[private.timeout]``
193. ``sync_data     = "3s"``
194. ``query_data    = "3s"``
195. ``fetch_data    = "3s"``
196. ``check         = "1h"``

197. ``[private.ttl]``
198. ``name = "time"``
199. ``[private.ttl.time]``
200. ``timeout = "24h"``
201. ``[private.ttl.block]``
202. ``number = 5``

203. ``[executor]``
204. ``[executor.syncer]``
205. ``max_block_fetch         = 50``

206. ``[executor.archive]``
207. ``archive_root  = "data/archive/"``

208. ``[executor.nvp]``
209. ``exitflag = false``
210. ``sync_mode = "block" #another mode is journal #新增``

211. ``[executor.sync_chain]``
212. ``priority = "block"``
213. ``sync_journal_receipt = true``
214. ``hs_interval = "5s"``
215. ``hs_resend_times = 5``
216. ``batch_size = 100``
217. ``state_sync_interval = "2s"``
218. ``fetcher_resend = 5``
219. ``mode_negotiate_interval = "5s"``

220. ``[executor.filemgr]``
221. ``enable = false``
222. ``deadline = "600s"``
223. ``clean_interval = "60s"``
224. ``file_system_mode = "ipfs" #origin, ipfs``
225. ``data_path = "namespaces/global/data/filemgr"``
226. ``ipfs_url="localhost:5001"``
227. ``hs_interval = "30s"``
228. ``batch_size = 100``
229. ``transfer_interval = "30s"``

``[database]``
	``public_path = "data/public/"``
	``private_path = "data/private/"``
    ``[database.state]``
        ``# 1. Encryption is a resource consuming functionality and will somewhat slow the process down.``
        ``# 2. Something unexpected can happen if this field is changed more than once (e.g. switch-off -> start -> swicth-on -> restart).``
        ``# 3. Make sure the whole cluster is running with the same encryption config, or the data transfer (sync-chain)``
        ``#    between nodes will fail.``
        ``encrypt = false``
        ``type="multicache"``
        ``[database.state.multicache]``
            ``#maximum memory occupation of tables``
            ``persist_goroutine_num = 100``
            ``underlyint_num = 10``
            ``memory_limit = "500mb"``
            ``data_path = "statedb/"``
            ``[database.state.multicache.persist_db]``
                ``type="leveldb" #multidb, memdb or leveldb``
                ``[database.state.multicache.persist_db.multidb] # work iff type="multidb"``
                    ``db_amount_limit = 32``
                    ``db_paths = [``
                        ``"namespaces/global/data/public/statedb/persist/multidb-0",``
                        ``"namespaces/global/data/public/statedb/persist/multidb-8",``
                        ``"namespaces/global/data/public/statedb/persist/multidb-16",``
                        ``"namespaces/global/data/public/statedb/persist/multidb-24"``
                    ``]``
                    [database.state.multicache.persist_db.leveldb]
                    block_cache_capacity      = "8mb" # "mb", "kb"
                    block_size                = "4kb" # "mb", "kb"
                    write_buffer              = "4mb" # "mb", "kb"
                    write_l0_pause_trigger    = 12
                    write_l0_slowdown_trigger = 8
                    # the level db file size (default is 2mb, v1.2 is 8mb)
                    compaction_table_size     = "8mb"
                [database.state.multicache.persist_db.tikv]
                    pd_addrs = ["172.16.5.4:2371"]
            [database.state.multicache.temp_db]
                type="leveldb"
                [database.state.multicache.temp_db.leveldb]
                    block_cache_capacity      = "8mb" # "mb", "kb"
                    block_size                = "4kb" # "mb", "kb"
                    write_buffer              = "4mb" # "mb", "kb"
                    write_l0_pause_trigger    = 12
                    write_l0_slowdown_trigger = 8
                    # the level db file size (default is 2mb, v1.2 is 8mb)
                    compaction_table_size     = "8mb"
    [database.account]
        encrypt = false
        type="multicache"
        [database.account.multicache]
            #maximum memory occupation of tables
			persist_goroutine_num = 5
			underlyint_num = 1
            memory_limit = "50mb"
            data_path = "accountdb/"
            [database.account.multicache.persist_db]
                type="leveldb"
                [database.account.multicache.persist_db.leveldb]
                    block_cache_capacity      = "8mb" # "mb", "kb"
                    block_size                = "4kb" # "mb", "kb"
                    write_buffer              = "4mb" # "mb", "kb"
                    write_l0_pause_trigger    = 12
                    write_l0_slowdown_trigger = 8
                    # the level db file size (default is 2mb, v1.2 is 8mb)
                    compaction_table_size     = "8mb"
                [database.account.multicache.persist_db.tikv]
                    pd_addrs = ["172.16.5.4:2371"]

    [database.chain]
        encrypt = false
        type="multicache"
        [database.chain.multicache]
            #maximum memory occupation of tables
			persist_goroutine_num = 5
			underlyint_num = 1
			memory_limit = "50mb"
            data_path = "chaindb/"
            [database.chain.multicache.persist_db]
                type="leveldb"
                [database.chain.multicache.persist_db.leveldb]
                    block_cache_capacity      = "8mb" # "mb", "kb"
                    block_size                = "4kb" # "mb", "kb"
                    write_buffer              = "4mb" # "mb", "kb"
                    write_l0_pause_trigger    = 12
                    write_l0_slowdown_trigger = 8
                    # the level db file size (default is 2mb, v1.2 is 8mb)
                    compaction_table_size     = "8mb"
                [database.chain.multicache.persist_db.tikv]
                    pd_addrs = ["172.16.5.4:2371"]

    [database.block]
        encrypt = false
        type="filelog"
        [database.block.filelog]
            path = "blockdb/filelog/"
            compression = "snappy" #zlib pure snappy
            max_log_file_size = "100mb" # "mb", "kb"
            data_version = 1 # default to be 1, means disk storage struct version
            index_enable = true

            [database.block.filelog.cache]
                enable = true
                max_cache_size = 100 #mb
                cache_expired_time = 48 #hour
                cache_entry_num = 20

            [database.block.filelog.handler_cache]
                handler_cache_num = 100
                handler_cache_evict_time = 1 # *time.Second
    [database.journal]
        encrypt = false
        type="filelog"
        [database.journal.filelog]
            path = "journaldb/filelog/"
            compression = "snappy" #zlib pure snappy
            max_log_file_size = "100mb" # "mb", "kb"
            data_version = 1 # default to be 1, means disk storage struct version
            index_enable = true

            [database.journal.filelog.cache]
                enable = true
                max_cache_size = 100 #mb
                cache_expired_time = 48 #hour
                cache_entry_num = 20

            [database.journal.filelog.handler_cache]
                handler_cache_num = 100
                handler_cache_evict_time = 1 # *time.Second

    [database.receipt]
        encrypt = false
        type="filelog"
        [database.receipt.filelog]
            path = "receiptdb/filelog/"
            compression = "snappy" #zlib pure snappy
            max_log_file_size = "100mb" # "mb", "kb"
            data_version = 1 # default to be 1, means disk storage struct version
            index_enable = true

            [database.receipt.filelog.cache]
                enable = true
                max_cache_size = 100 #mb
                cache_expired_time = 48 #hour
                cache_entry_num = 20

            [database.receipt.filelog.handler_cache]
                handler_cache_num = 100
                handler_cache_evict_time = 1 # *time.Second

    [database.invalidtx]
        encrypt = false
        type="leveldb"
        [database.invalidtx.leveldb]
                path="invalidtx/leveldb/"
				logpath="invalidtx/log"
                block_cache_capacity      = "8mb" # "mb", "kb"
                block_size                = "4kb" # "mb", "kb"
                write_buffer              = "4mb" # "mb", "kb"
                write_l0_pause_trigger    = 12
                write_l0_slowdown_trigger = 8
                # the level db file size (default is 2mb, v1.2 is 8mb)
                compaction_table_size     = "8mb"

    [database.consensus]
        encrypt = false
        type="leveldb"
        [database.consensus.leveldb]
                path="consensusdb/leveldb/"
				logpath="consensusdb/log"
                block_cache_capacity      = "8mb" # "mb", "kb"
                block_size                = "4kb" # "mb", "kb"
                write_buffer              = "4mb" # "mb", "kb"
                write_l0_pause_trigger    = 12
                write_l0_slowdown_trigger = 8
                # the level db file size (default is 2mb, v1.2 is 8mb)
                compaction_table_size     = "8mb"

    [database.camanager]
        encrypt = false
        type="leveldb"
        [database.camanager.leveldb]
                path="cadb/leveldb/"
				logpath="cadb/log"
                block_cache_capacity      = "8mb" # "mb", "kb"
                block_size                = "4kb" # "mb", "kb"
                write_buffer              = "4mb" # "mb", "kb"
                write_l0_pause_trigger    = 12
                write_l0_slowdown_trigger = 8
                # the level db file size (default is 2mb, v1.2 is 8mb)
                compaction_table_size     = "8mb"

    [database.radar]
        encrypt = false
        type="leveldb"
        [database.radar.leveldb]
                path="radardb/leveldb/"
				logpath="radardb/log"
                block_cache_capacity      = "8mb" # "mb", "kb"
                block_size                = "4kb" # "mb", "kb"
                write_buffer              = "4mb" # "mb", "kb"
                write_l0_pause_trigger    = 12
                write_l0_slowdown_trigger = 8
                # the level db file size (default is 2mb, v1.2 is 8mb)
                compaction_table_size     = "8mb"

    [database.mq]
        encrypt = false
        type="leveldb"
        [database.mq.leveldb]
                path="mqdb/leveldb/"
				logpath="mqdb/log"
                block_cache_capacity      = "8mb" # "mb", "kb"
                block_size                = "4kb" # "mb", "kb"
                write_buffer              = "4mb" # "mb", "kb"
                write_l0_pause_trigger    = 12
                write_l0_slowdown_trigger = 8
                # the level db file size (default is 2mb, v1.2 is 8mb)
                compaction_table_size     = "8mb"

	[database.minifile]
		consensus = "minifile/consensus"
		sync = "minifile/sync"
		bloom = "minifile/bloom"
		nvp = "minifile/nvp"

    [database.indexdb]
        [database.indexdb.layer1]
            enable = false
            dbType = "mongodb"
       [database.indexdb.tempdb]
            path = "indexdb/tempdb/leveldb/"
        [database.indexdb.layer2]
            # Defines for which fields to create layer2 index, optional value including:
            #   1 - indicate field named block write time;
            #   2 - indicate field named transaction from;
            #   3 - indicate field named transaction to;
            #   4 - indicate field named transaction hash;
            # For example:
            #      active = [] - means dont create any layer2 index;
            #      active = [1] - means create layer2 index for block write time;
            #      active = [1, 2] - means create layer2 index for block write time and transaction from;
            # This config item works only when database.indexdb.layer1.enable is true.
            active = []
        [database.indexdb.mongodb]
            # if you should set username and password, please use
            # mongodb://username:password@127.0.0.1:27017?w=1&journal=true,
            # for example: "mongodb://flatoUser:123456@127.0.0.1:27017?w=1&journal=true"
            uri = "mongodb://127.0.0.1:27017/?w=1&journal=true"
            limit = 5000
            tlsEnable = false
            tlsCA = "certs/mongodb_ca.pem"
            tlsCertKey = "certs/mongodb_client_cert.pem"
	[cvp.backup]
    	path = "data/cvp"

	[send.args.extra.check]
		enable = false
		url    = "https://filoop.com/api/v1/safe/text"


```

