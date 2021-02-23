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

##########################################################
#
# P2P section
#
##########################################################
[p2p]
transport               = "grpc" # we now only support "grpc"
compress                = false  # enable compression and decompression of network message

# network mode, enum: direct, relay, discover. direct is default mode.
# discover mode not require full hosts.toml.
# relay mode not require direct node.
mode                    = "direct"

retrytime               = "3s"
ipc                     = "hpc_1.ipc"
enableTLS               = true
tlsCA                   = "./tls/tlsca.ca"
tlsServerHostOverride   = "hyperchain.cn"
tlsCert                 = "./tls/tls_peer.cert"
tlsCertPriv             = "./tls/tls_peer.priv"

# connection control configurations
# keepalive configuration
# [1,5,10] means minLimit=1, threshold=5 maxLimit=20
keepAliveDuration = [1,5,20]

# pending configuration
# [5,20,100] means minLimit=1, threshold=20 maxLimit=100
pendingDuration = [5,20,100]

# send keepalive message every keepAliveInterval
keepAliveInterval = "5s"

# stream will be closed if not receive keepalive message for keepAliveTimeout
keepAliveTimeout = "3m"

    [p2p.grpc]
    maxRecvMessageSize = "50mb" # default 50mb
    maxSendMessageSize = "50mb" # default 50mb

##########################################################
#
# flow control configurations section
#
##########################################################
[flow.control.ratelimit]
enable            = true
fileReceivePeak   = 100
fileReceiveRate	  = "8ms"
fileSendPeak	  = 100
fileSendRate      = "8ms"

[flow.control.bandwidth]
enable            = false
outgoingBandwidth = "500Mb" # NOTE: The unit is Mb/s NOT MB/s

</div>

其中log模块表示日志相关的配置选项，您可以通过修改log.dump_file来控制是否将输出日志重定向至日志文件中， **我们推荐您开启日志重定向** 。

您可以根据实际申请开放的端口号进行port模块的配置，其中grpc端口是节点间通信的端口号，注意要与addr.toml中的端口号对应；jsonrpc端口是外部应用向hyperchain平台发送请求使用的端口号。

您可以通过修改flow.control.ratelimit.enable和flow.control.bandwidth的值来控制是否开启节点流量控制和带宽限制，建议根据测试的tps进行流控设置，详细的流控配置可参考3.2节内容。

