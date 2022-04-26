
□　接收和发送的数据包。 这个指标用来通知你，一个给定网络接口接收和发送数据包的数量。

□　接收和发送的字节。 这个指标描述了一个给定网络接口接收和发送的字节数。

□　每秒钟的冲突数量。 这个值给出了在网络上连接的每个接口发生冲突的相对数量。如果发生持续冲突通常要关注网络基础设施的问题，而不是服务器。在大多数正确配置的网络中，冲突一般是非常罕见的，除非网络基础设备是由集线器（HUB）组成的。

□　丢弃的数据包。 已经被内核丢弃的数据包的统计数。丢弃的原因可能是由于防火墙配置，也可能是由于缺乏网络缓冲区。

□　溢出。 该指标表示网络接口溢出缓冲区空间的次数。这个指标应该结合数据包被丢弃的值使用，用来确定是网络缓冲区还是网络队列长度出现瓶颈。

□　错误。 被标记为故障帧的数量。通常这些错误是由网络不匹配或是部分网络电缆中断导致的。部分网络电缆中断对于铜线千兆网络是一个明显的性能问题。

## netstat、ss
netstat是最流行的工具之一。如果在网络上工作，你应该熟悉这个工具。它显示许多网络相关的信息，比如socket的使用、路由、接口、协议及网络统计信息等。

替换netstat的是ss，替换netstat -r的是ip route，替换netstat -i的是ip -s link，替换netstat -g的是ip maddr。

```
root@zyg ～]# netstat -n
Active Internet connections (w/o servers)
Proto	Recv-Q	Send-Q	Local Address	Foreign Address	State
tcp	0	232	219.168.5.38:9825	123.120.27.173:53432	ESTABLISHED

Active UNIX domain sockets (w/o servers)
Proto	RefCnt	Flags	Type	State	I-Node Path
unix	2	[]	DGRAM	14021	/tmp/fcoemon.dcbd.2162
unix	3	[]	DGRAM	13943	/var/run/lldpad/clif
unix	2	[]	DGRAM	14025	/var/run/fcm/fcm_clif
unix	15	[]	DGRAM	24547910	/dev/log
unix	2	[]	DGRAM	9338	@/org/kernel/udev/udevd
unix	2	[]	DGRAM	14336	@/org/freedesktop/hal/udev_event
unix	2	[]	DGRAM	25205379	
unix	2	[]	DGRAM	24548943	
```

Active Internet connections
□　Proto，socket使用的协议（tcp、udp、udpl、raw）。

□　Recv-Q

•　Established，连接这个socket的用户程序非复制的字节数。

•　Listening，从2.6.18内核开始这列包含了当前的syn_backlog。

□　Send-Q

•　Established，远程主机没有确认的字节数。

•　Listening，从2.6.18内核开始这列包含了syn_backlog的最大值。

□　Local Address，socket本地端的地址和端口号。除非指定–numeric（-n）选项，否则socket地址将被解析为规范主机名（FQDN），端口号被翻译成规范服务名称。

□　Foreign Address，socket远程端的地址和端口号。类似“Local Address”。

□　State，socket的状态。因为raw模式没有状态，通常没有状态用于UDP和UDPLite，这列可能保留为空。一般情况下会是以下值之一：

•　ESTABLISHED，socket是一个已经建立的连接。

•　SYN_SENT，socket积极尝试建立一个连接。

•　SYN_RECV，从网络接收到一个连接请求。

•　FIN_WAIT1，socket已关闭，并且连接关闭。

•　FIN_WAIT2，连接已关闭，并且socket等待远程端关闭。

•　TIME_WAIT，连接关闭之后socket等待处理的仍在网中的数据包。

•　CLOSE，socket没有被使用。

•　CLOSE_WAIT，远程端已经关闭，等待socket关闭。

•　LAST_ACK，远程端已经关闭，并且socet已经关闭。等待确认。

•　LISTEN，socket监听进来的连接。在输出中不包含这样的socket，除非你指定--listening（-l）或–all（-a）选项。

•　CLOSING，两端scoket已经关闭，但是仍然没有发送所有的数据。

•　UNKNOWN，socket的状态是未知。