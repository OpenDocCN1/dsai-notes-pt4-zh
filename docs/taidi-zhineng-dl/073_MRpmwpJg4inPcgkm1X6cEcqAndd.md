# Signal Integrity & Serial Comm

> 来源：[https://learning.feishu.cn/docx/D3VWdQIbDovQcSx45ALcJHA7nff](https://learning.feishu.cn/docx/D3VWdQIbDovQcSx45ALcJHA7nff)

# signial

https://www.youtube.com/watch?v=anX8QZMhVjI

Understanding Signal Integrity

![](img/584d1859105bf757d63bd4130d0f8445.png)

![](img/dc2a9ce269894684d18fba298669d693.png)

![](img/050d11e3610f94cd77a15622e313ddde.png)

![](img/0baa5a180d3b1a7bba29fe390e17822a.png)

![](img/05fbbc35cfde355bfc11f0c4b7599a2e.png)

![](img/cdec397668e95e0264fcae60ff5c4292.png)

![](img/62bba5d3368520b7d1bf84e3698fb428.png)

![](img/45d04ce35f9875dc71cff372318c96b5.png)

![](img/b07b814c78ed4143d5de54fde17bdffd.png)

![](img/618a93601cecc77890eff2754e2847fd.png)

![](img/c102136d4e04cb19602500c749c79368.png)

![](img/18d5dd983d92059a6c4d2048c50dd3ee.png)

![](img/7a911b8b73b892fb3d1e2bcb14dd7433.png)

![](img/2fee027e0831fa0346e2896e71444a93.png)

![](img/06a60588359b2a93b07b56a365de999f.png)

![](img/d135903961c36c8d5a3ce17e532a275c.png)

![](img/bb668886b9b0e7dfeb425110728b6a4e.png)

# 高速差分信号

differential transmission

差分信号是指每次传输一个信号时同时发送它的正信号和负信号（D+ 和 D-，这两种信号振幅相同相位相反）差分信号提高了噪声容限，并降低了信号电压。

差分信号的接收端将会接收这一对相位相反的信号，用正信号的电压减去其反相信号的电压，得到它们的差值，以此来判定这个比特的逻辑电平值。

高速差分信号要求发送端串接一个 AC 耦合电容。

差分信号在布线时要求“等长”、“等宽”、“贴近”，并在同一层。外部干扰噪声将同时、同值加载到D+和D-上，减小对信号的逻辑值影响。

差分信号能有效抑制电磁干扰(EMI)，差分信号D+和D-距离相近且信号幅值相等、极性相反，与地线间耦合电磁场的幅值相等，将相互抵消，对外界的电磁干扰较小。

PCIe链路使用差分信号进行数据传送，一个差分信号由D+和D-两个信号组成，接收端通过比较这两个信号的差值，来判断发送端的发送逻辑是“1”还是“0”。

PCIe总线物理链路间的数据传送使用基于时钟的同步传送机制，物理链路上无时钟线，接收端通过时钟恢复模块CDR,从接收报文中提取接收时钟，进行同步数据传输，PCIe设置在进行链路训练时将完成时钟的提取工作。

![](img/41f9b9dfb41da3e28e1c696178e7485c.png)

相同内容通过一正一反镜像传输，干扰可以很快被发现和纠正，从而可以将传输频率大幅提升。加上PCI原来基本是半双工的（地址/数据线太多，不得不复用线路），而串行可以全双工。综合下来，如果如果我们从频率提高下来得到的收益大于一次传输多个bit的收益，这个选择就是合理的。我们做个简单的计算:

PCI传输: 33MHz x 4B = 133MB/s

PCIe 1.0 x1: 2.5GHz x 1b = 250MB/s (知道为什么不是2500M / 8=312.5MB吗？)

速度快了一倍!我们还得到了另外的好处，例如布线简单，线路可以加长（甚至变成线缆连出机箱！），多个lane还可以整合成为更高带宽的线路等等。

高速差分串行总线信号线只有2条，如何区别各种总线事务是一个难题，比如怎么区分读写事务，怎么上报中断信息等。在并行总线中，可以增加一条信号来表示，比如中断信号线，读写使能信号线。但是，在串行总线中，没有这样做的条件，只有另寻他法。可以参考网络传输协议，即以报文的形式交换信息。报文可以简单地理解为在有效数据负载的前后附加一些具有相对固定形式的控制信息，这些控制信息可以使得外设正确识别出报文，并检验报文的正确性，同时将数据负载送到正确的目的地。

直观上来说，高速差分串行总线扩展性很差，一般都是点对点传输，几乎不可能复用差分串行总线。但是为了实现扩展性的需求，可以专门设计一个组件，实现高速差分串行总线的可扩展，这也就是PCIe体系中Switch的功能。

经过上述一通合理分析，板间通信的高带宽、高可靠性的通信总线大概率会采用如下技术：

+   高速差分串行总线

+   报文通信

+   专用外设扩展组件

以上3个技术也是PCIe高速总线采用的技术

# SerDes

SERDES (Serializer/Deserializer)

![](img/2ef20aa8afde73ea01ac996a84187591.png)

![](img/19ad82b896a2049036fcca456bf1c662.png)

# Parallel & Serial Comm

LPT 和 PCI 都是 parallel。

USB / HDMI / Lightning 都是 serial。

https://www.youtube.com/watch?v=3xZsTBEUkFI

How SERDES works in an FPGA, high speed serial TX/RX for beginners

![](img/a7f7f07cff5b2450a1d43d5b5d9e1e97.png)

![](img/e63a9d8d3d76540119dd4bc47acc3704.png)

![](img/d7f15bbeb6f205dfc8e892e4a6783774.png)

![](img/9cf943182f134783938bbd8fafb08707.png)

![](img/9f78a26291a3236af74e5963e4cee118.png)

DC is direct current.

![](img/766a565346f63db1e9e26ce08a9df7eb.png)

差分信号

LVDS 是一种常用的差分信号标准。

![](img/cc001904488af7999a19d6379b93bbe3.png)

![](img/347720d74d8a4277d303ac060cf96b25.png)

![](img/610a0beab21c7dd1357e159967810a6b.png)

![](img/f033ea2db34a5d62c38ff4748fbe7e75.png)

![](img/32993bdf7c66de9f314b2d83029d6c2f.png)

并行总线结构中，桥下面挂连设备的一条总线在PCIe中变为一条链路，一条链路可包含一条或多条通路，每条通路由两对差分信号线组成传输通道，没有专用的数据、地址、控制和时钟线，总线上各种事务组成数据包来传送。点到点的互连方式使得每个设备由独立的链路连接，独享带宽，这是提高传输速率的有效解决方案。

一个数据通道 Lane,有两组差分信号，共4根信号线

为什么单端并行数据传输模式的数据速率会受到限制呢？

数据传输需要有时钟来采样，必须要有建立时间（ts）和保持时间（th），如果有64根数据线要实现高速同步传输，就必须要求时钟信号与64根信号线保持精确等长，走线路径环境（PCB板材、过孔以及连接器等）相同，还不能受到串扰等等。

为什么DDR总线却可以呢？现在DDR5的速率可以支持到6.4Gbps，它为什么能这么牛B的单端并行传输？有兴趣的胖友们，可以思考下这个问题，DDR总线是如何解决这些问题的。

PCI插槽都是等长的，防呆口位置靠上，大部分都是纯白色。PCIe插槽大大小小，最小的x1，最大的x16，防呆口靠下。

![](img/9350553fdc2a1bf8cedfa397e38d1ea3.png)

![](img/59fbe22f21e8fe4ab3f80375eaddce01.png)

![](img/4f9d1493f07e7bd26c9a22dea3a9c8ad.png)

uart 串口协议

![](img/9819b090690c306c1326c46b10704f8e.png)

# repeater

repeater 是一个 signal conditioner，分 2 类：

1.  retimer

1.  redriver

used to maintain signal quality of high speed links.

endpoint 是一个 PCIe device，比如 GPU。

https://www.youtube.com/watch?v=69v-4_RncAQ

Solving PCIe signal integrity challenges

![](img/a2af9172ff6bf6a4da24b6016644b98a.png)

![](img/722f704aa5261538b543ba4a32bb43f3.png)

![](img/bd971f4873ebb547338e75839545008a.png)

![](img/40c87bcc13132e1d2aacd1938a4727c4.png)

![](img/7797aab68cf3c8f366f68b826a817d46.png)

![](img/0f22058f8fef0272355838234a7ab60c.png)

答案

![](img/e97deeff940b86e6a90868c0487b9962.png)

https://www.youtube.com/watch?v=kX0F8K2rjR4

When to use a PCIe retimer vs. redriver

![](img/a96517c46d8f2d75f2a2f2e136bf3b4c.png)

![](img/d0a14be5c8d65468010221d9be901587.png)

![](img/094eb7f015f6ad2e05098b6a0e7042e4.png)

![](img/9baa7d627b5e4554dd21dc55d90d2df7.png)

![](img/d4f76ea42fb30d9cc63ad8a73f7b0a8b.png)

![](img/ceb6da9a2a2460dfa56db0d348a437fd.png)

https://www.youtube.com/watch?v=mselU-aKRAg

Signal Conditioning: What is a Signal Conditioner

![](img/17ba9edbc8e0d3ea9d7fdf02e2927225.png)

![](img/beb275d83ed48151300185d0f02c890e.png)

![](img/c6d02c1e1c840c20d0efe32cce6acadc.png)

![](img/4db8a047154ce3c119406742c7c74163.png)

![](img/d9d01ceae8f3a8e5a80bab61762c10e1.png)

![](img/839cd3f949a8ead02313fa4e77082058.png)

![](img/19c307efe7d72fff6fd84027aaf8dbfa.png)

![](img/494e8f8dc75ad77e79c8fc8a317b6df9.png)

![](img/44d9b0e600230ff40bfb63062521e586.png)

# eye diagram

https://www.youtube.com/watch?v=tZiKRfH2yZ4

What is an eye diagram

![](img/57e038ea602cb4037ced38f313d73245.png)

![](img/94ceb107ff6f880b1705e224c843a8cf.png)

![](img/49d62bb92ade0881b7e7c9bc6214c0d6.png)

![](img/89f45c4caadd64e2f74c7f2b0f9d3380.png)

![](img/e93bc7fd00077f2227b9a8dbe00c00b7.png)

![](img/247c81e4f16f223261f231a6b50861e4.png)

![](img/f670afe3134b8d88fea596c4ed6df761.png)

![](img/393975eb46061bfef821af5740436f69.png)

![](img/7a6fa5e004b20ece9897dc40a9267872.png)

![](img/805ea30a0ce0fdda124ba311bd4bd6fd.png)

![](img/e2d0f40765d53ad1c20ff581aabcb69a.png)

1.  B, C

1.  B, C, D

1.  False