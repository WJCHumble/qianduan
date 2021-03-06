## TCP 三次握手

TCP 三次握手的目的是为了防止已经失效的连接请求报文又传回服务端，而产生错误。

过程：
- 客户端发送标有 SYN 的数据包，表示我将要发送请求。
- 服务端发送标有 SYN/ACK 的数据包，表示我已经收到通知，告知客户端发送请求。
- 客户端发送标有 ACK 的数据包，表示我要开始发送请求，准备被接受。

## TCP 四次握手

TCP 四次握手主要是在连接断开前发起。

过程：
- 客户端发送请求，申请断开连接，进入等待阶段，此时不会发送数据，但是会继续接收数据。
- 服务端接收请求后，告知客户端已明白，此时服务端进入等待状态，不会在接收数据，但是会继续发送数据。
- 客户端收到后，进入下一阶段等待。
- 服务端发送完剩余的数据后，告知客户端可以断开连接，此时服务端不会发送和接收数据。
- 客户端收到后，告知服务端我开始断开连接。
- 服务端收到后，开始断开连接。

