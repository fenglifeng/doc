ICMP
-----------
ICMP(internet control message protocol)是网络协议族的核心协议之一。它用于TCP／IP网络中发送控制消息，提供可能发送在通信环境中的各种问题反馈，通过这些信息，令管理者可以对发生的问题作出诊断，然后采取适当的措施解决。

ICMP依靠IP来完成它的任务，它是IP的主要部分，它是在第四层，跟UDP一样，它是不可靠。

traceroute是通过发送含有特殊的TTL的包，然后接受ICMP超时消息和目标不可达消息来实现的。

ping则是用ICMP的“echo request”和“echo reply”消息来实现的。

ICMP报头：

	8bit：TYPE。icmp的类型
	8bit：CODE。进一步划分ICMP的类型。也就是说TYPE＋CODE可以确定ICMP的类型.
	16bit:checksum。校验码，ICMP报头加数据，计算得到的。
	16bit:ID。ID值
	16bit：sequence。序号，比如ping中就有序号。

关于TYPE、CODE可以参考以下链接[CODE、TYPE FOR ICMP](http://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%8E%A7%E5%88%B6%E6%B6%88%E6%81%AF%E5%8D%8F%E8%AE%AE)


填充数据：可根据用户的需求，填充数据大小和信息。

IGMP
----------
IGMP(internet group management protocol)管理因特网多播组成员的一种通信协议。它让一个物理网络上的所有系统知道主机所在的多播组。多播路由器需要这些信息以便知道多播数据保温应该向哪些接口转发。

如果说ICMP用于单播，那么IGMP是用于多播的。

IGMP的报文长度是定死的——8bytes。
IGMP的报文：

	1byte：TYPE。版本号。如果是membership query(成员查询)=0x11;membership report(成员报告)IGMPv1=0x12、 IGMPv2=0x16,leave group(离开组)=0x17,IGMPv3 membership report =0x22
	1byte:Max Resp Time.指定的报文报告时限。这个只在membership query(type == 0x11)的时候有效。如果设置成0则无视接受。
	2byte:Checksum。校验码。
	4byte:group address。组地址。这个就是要被查询的设备所在的多播组地址。如果是查询报文的话，设置成0。

GMRP
------------
GMRP(garp multiple registrasion protocol)组播注册协议.它是基于garp的一个组播注册协议，用于维护交换机中的注册信息。所有支持gmrp的交换机能够接收来自其他交换机的组播注册信息，并动态更新本地的组播注册信息，同时也能将本地的组播注册信息向其他交换机传播。这种信息交换机制，确保了同一交换网内所有支持gmrp的设备维护的组播信息的一致性。

当一台主机想要加入某个组播组时，它将发出gmrp加入消息。交换机将接到gmrp加入消息的端口加入到该组播组中，并在接收端口所在的vlan中广播该gmrp加入消息，vlan中的组播源就可以知晓组播成员的存在。当组播源向组播组发送组播报文时，交换机就只把组播报文转发给与该组播组成员相连的端口，从而实现了在vlan内的二层组播。

RSTP
-------------
RSTP(rapid spanning tree protocol)快速生成树协议，这种协议在网络结构发生变化时，能更快的收敛网络。原理：通过在交换机之间传递一种特殊的协议报文(在IEEE 802。1D中这种协议报文被称之为“配置消息”)来确定网络的拓扑结构。配置消息中包含了足够的信息来保证交换机完成生成树计算。

RSTP有五种端口：根端口，指定端口，替换端口，备用端口，禁用端口

802.1D中规定了端口有五种状态。阻塞blocking、监听listening、学习learning、转发forwarding、关闭(disable)。而在RSTP中将blocking、listening、diasble都规程为discarding(禁止)，learning、forwarding还是同一个状态，所以RSTP中只有三种状态。可以理解为不能地址学习，但是能收发配置(discarding)；可以进行地址学习，收发数据，但是不能数据转发(learning)；全部数据都能转发，能地址学习(forwarding)。

	*Blocking：处于这个状态的端口不能够参与转发数据报文，但是可以接收配置消息，并交给CPU进行处理。 不过不能发送配置消息，也不进行地址学习。
	*Listening：处于这个状态的端口也不参与数据转发，不进行地址学习；但是可以接收并发送配置消息。
	*Learning：处于这个状态的端口同样不能转发数据，但是开始地址学习，并可以接收、处理和发送配置消息。
	*Forwarding：一旦端口进入该状态，就可以转发任何数据了，同时也进行地址学习和配置消息的接收、处理和发送。


