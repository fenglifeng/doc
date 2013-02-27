1:ICMP(internet control message protocol)是网络协议族的核心协议之一。它用于TCP／IP网络中发送控制消息，提供可能发送在通信环境中的各种问题反馈，通过这些信息，令管理者可以对发生的问题作出诊断，然后采取适当的措施解决。

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

