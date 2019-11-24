# 与 HTTP 关系密切的协议

在 TCP / IP 协议族中与 HTTP 密不可分的3个协议：IP、TCP、DNS。

## 一、IP 协议

按 TCP 分层来说，IP（Internat Prototcal）网际协议位于网络层。它的作用是把各种数据包传送给对方。并且，确保传送到对方的前提条件是，IP 地址和 MAC 地址（Media Access Control Address）。
其中，IP协议指明了节点被分配的地址，MAC地址是指网卡所属的固定地址。IP 地址可以和MAC地址进行配对（需要注意的是IP地址可变换，但MAC地址基本不会变化）。

### 二、ARP 协议解析 IP

IP 间通信依赖与 MAC 地址。即在实际中，通信的双方在同于局域网内的情况是很少的。所以，通常是通过多台计算机和网络设备中转才能连接到对方。在通信进行中转时，则会利用下一站中转设备的MAC地址来搜索下一个中转目标。而中转的跳转，是根据 ARP（Address Resolution Protocol）协议对通信方的 IP 地址进行解析，从而查出对应的 MAC 进行相应的跳转。
并且，需要注意的是，在跳转的过程中，是一种模糊跳转，并不是精确的跳转，这种跳转机制被称作路由选择。

<div align=center> <img src="https://img-blog.csdnimg.cn/20191105224320366.png" width = "600" height = "500"  /> </div>

## 三、TCP 协议

TCP 协议属于传输层，用于提供可靠的字节流服务（字节流服务是指，将大块数据分割成报文端为单位的数据包进行管理）。而在 TCP 协议中，最重要的就是三次握手（three-way handshaking）策略，即指在传递数据包过程中，首先发送方会发送一个 SYN（synchronize）标志的数据包给接收方，接收方接收到后，给发送端回传一个带有 SYN/ACK 标志的数据包，通知发送端已成功接收，最后，发送端回传一个带有ACK标志的数据包，从而结束“握手”。

<div align=center> <img src="https://img-blog.csdnimg.cn/20191106230442500.png" width = "600" height = "500"  /> </div>

## 四、DNS

DNS 是位于应用层的协议，它是负责提供域名到 IP 地址之间映射的解析服务系统。DNS 的出现是为了方便人们记忆，而不是直接通过访问难记的 IP 来访问计算机。即这个过程就是，DNS 协议提供通过域名查找对应的 IP 地址，或逆向从 IP 地址反查域名。
  
<div align=center> <img src="https://img-blog.csdnimg.cn/20191109091928163.png" width = "600" height = "500"  /> </div>
