## 引言
无论是从事 Web 前端或者后端的同学，对 HTTP 报文应该都是**最熟悉的陌生人**。为什么这么说呢？熟悉在于每一次接口对接、联调都避免不了去 Network 里面看请求的 params、response、URI 等等。其实这些已经是 HTTP 报文的一部分了，但是需要注意的是仅仅是一部分，HTTP 还有很多的请求首部字段、响应首部字段、通用首部字段、实体首部字段（PS：我之前看的一些博客称这些为请求标头、响应标头，说实话这种不专业的名词简直就是误人子弟！）等等。

HTTP 报文可以分为**请求报文**和**响应报文**：
- 请求报文由请求行（方法、URI、HTTP版本）、请求首部字段、通用首部字段、实体首部字段、其他构成
- 响应报文由状态行、响应首部字段、通用首部字段、实体首部字段、其他构成

从上面的请求报文和响应报文的构成可以得知，最重要的**四种首部字段**

### 请求首部字段


首部字段名     |  说明
-------- | -----
Accept  | 用户代理可处理的媒体类型（ text/html,application/xhtml+xml 等）
Accept-Charset  | 优先的字符集
Accept-Encoding  | 优先的内容编码
Accept-Language | 优先的语言
Authorization |  Web 认知信息
Expect |  期待服务器特定的行为
From | 用户的邮箱地址
Host | 请求资源所在的地址
If-Match | 比较实体标记（ETag，常用于 Web 应用的缓存）
If-Modified-Since | 比较实体的更新时间
If-None-Match | 比较实标记（和 If-Match 相反，即没有 Match 到）
If-Range | 资源未更新时发送实体 Byte 范围的请求
If-Unmodified-Since | 比较资源的更新时间
Max-Forwards | 最大传输逐跳数
Proxy-Authorization | 代理服务器要求的客户端信息
Range | 实体的字节范围请求
Referer | 请求 URI 的原始获取方
TE | 传输编码的优先级
User-Agent | HTTP 客户端程序的信息（简称 UA）

### 响应首部字段
首部字段名     |  说明
-------- | -----
Accept-Ranges  | 是否接收字节范围的请求
Age | 推算资源创建经过时间
ETag | 资源的匹配信息
Location | 令客户端重定向至指定 URI
Proxy-Authenticate | 代理服务器对客户端的认证信息 
Retry-After | 对再次发起请求的时机要求
Server| HTTP 服务器的安装信息
Vary | 代理服务器缓存的管理信息
WWW-Authenticate | 代理服务器对客户端的认知信息

### 实体首部字段
首部字段名     |  说明
-------- | -----
Allow  | 资源可支持的 HTTP 方法
Content-Encoding | 实体主体支持的编码方式
Content-Language | 实体主体的自然语言
Content-Length | 实体主体的大小（Byte）
Content-Location | 替代资源对应的 URI
Content-MD5 | 实体主体的报文摘要
Content-Range | 实体主体的位置范围
Content-Type | 实体主体的媒体类型
Expires | 实体主体过期的日期时间
Last-Modified | 资源的最后修改日期时间

### 通用首部字段
首部字段名     |  说明
-------- | -----
Cache-Control | 控制缓存的行为
Date |  创建报文的日期时间
Connection | 逐跳首部、连接的管理
Progma | 报文指令
Trailer | 报文末端的首部一览
Transfer-Encoding | 指定报文主体的传输编码方式
Upgrade | 升级为其他协议
Via | 代理服务器通知
Warning | 错误通知
