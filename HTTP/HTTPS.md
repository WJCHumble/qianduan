## HTTPS

认识 HTTPS 之前，先来讲讲 HTTP 存在的缺点：
- 使用明文，内容可能会被窃听
- 不验证通信方的身份，可能会遭遇伪装
- 无法证明报文的完整性

在一些场景，使用 HTTP 没有问题，但是在一些涉及到隐私、支付方面的问题的时候，我们就不能忽略 HTTP 存在的问题。而 HTTPS 的出现，就是为了解决 HTTP 存在的问题，那么什么是 HTTPS？

HTTPS = HTTP + 加密 + 认证 + 完整性保护

可以看出 HTTPS 是在 HTTP 的基础上加了**加密**、**认证**、**完整性保护**。HTTPS 并不是一种新的协议，只是相比较 HTTP，它的通信接口是由 SSL（Secure Socket Layer）和 TSL（Transport Layer Security）协议代替。这个过程会是这样，HTTP 会先和 SSL 通信，然后 SSL 和 TCP 通信，所以 HTTPS 又被称为身披 SSL 协议外壳的 HTTP。那么，接下来我们看看 HTTPS 是如何完成加密、认证以及完整性保护。

## 混合加密机制

HTTPS 采用混合加密机制，即共享密钥加密和公开密钥加密两者并用。

但是，一方面共享密钥需要将加解密用的公钥传送给对方，但是如果在传送的过程被劫持了公钥会导致内容泄漏。另一方面，由于公开密钥需要公开公钥，如果有采用私钥加密的信息，被其他人截取，则可以使用公开密钥解密。

因此，HTTPS 结合两种加密的优点，传输阶段，使用**公开密钥加密的私钥**加密交换之后需要用于解密的**共享密钥的公钥**，解密阶段，则用**公开密钥的公钥**解密出传输过来的**共享密钥的公钥**，然后用这个**共享密钥的公钥**进行进行解密。

## 数字签名

HTTPS 通过数字签名的方式来验证数据报在传输过程中是否被篡改。

数字签名使用的是公开密钥加密技术。首先，由发送方通过 Hash 算法将要发送的数据生成消息摘要，然后使用发送者的私钥加密生成数字签名，再和数据一起发送给接收方。接收方接收到后，使用公钥对数字签名进行解密，然后将解密后的内容和传送过来的数据进行对比，检测数据是否遭到修改。

## 数字证书（CA认证）

其实，虽说混合加密机制可以防止通信被窃听。但是，并不能验证公开密钥的真实性，即可能再传输过程中我们的公开密钥已经被替换掉了。所以这个时候我们就需要使用由数字证书认证机构和其相关机关颁发的公开密钥证数来确定公开密钥的真实性。与此同时，这也确保了通信双方的身份，因为数字证数是客户端和服务器双方都可以信赖的。

数字证数认证机构的业务流程：
- 首先，我们需要向数字证书认证机构提出公开密钥的申请
- 数字证书机构在认真申请者身份后，对申请的公开密钥进行数字签名，将签名后的公开密钥，放入到公开密钥证数并绑定
- 服务器将数字证书认证机构颁发的公开证书发送到客户端，从而进行公开密钥加密方式的通信（此时的公开密钥也被称为数字证书或证书）。
- 在之后的通信过程中，客户端可以用证书的公钥对服务器发送的证书进行验证，验证通过，则可以确认服务器的身份和公开密钥的真实性。


## 总结

讲了这么多 HTTPS 的优点以及弥补了 HTTP 的不足。那么，大家会像为什么我们现在大多数还是使用 HTTP 呢？因为，和 HTTP 通信的简单文本形式的传输，HTTPS 加密的通信方式会消耗更多的 CPU 和内存。如果，所有的通信都选择 HTTPS，那么消耗的资源会成倍地增加，相应地能并发地请求的数量也会减少。所以，在很多情况下，只有涉及到敏感信息的通信时才会使用 HTTPS。