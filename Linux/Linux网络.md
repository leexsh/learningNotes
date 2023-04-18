Linux网络

# 1.网络基本知识：

## 1.数据链路层：

1.数据链路层解决的三个问题： 封装成帧  透明传输  差错检验

数据在网络中传输最大不能超过1500个字节

封装成帧：加上帧头、帧尾

透明传输：若数据中存在帧尾数据，要加上填充字符来转义

差错控制：循环冗余检验

2.链路层协议：PPP协议

## 2.网络层

### 1.网络层协议：地址解析协议(ARP)        网际协议(IP) 逆地址解析协议(RARP) 网际控制报文协议(ICMP) 网际组管理协议(IGMP)

### 2.地址解析协议(ARP)：IP地址->物理地址        逆地址解析协议：物理地址->IP地址

### 3.IP数据报：

一个IP数据报由首部和数据两部分组成。首部20个字节，是固定的。数据不是固定的。 

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YWZhMDU2NDIzNTAzMzcxYjM0MjgwOGZmNTUxOWIzMDNfS2FMUXl4anRhNFhBZUhxdVVVR1o5b25HcHBjOVVQYlJfVG9rZW46Ym94Y25xc25aYjhoSGlscFVWY1hPN3BhVnk4XzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

(1)版本：用来表示TCP/IP是哪个版本，ipv4还是ipv6.

(2)首部长度：首部的长度

(3)区分服务：确定更高的传输优先级。

(4)总长度：确定数据部分长度。一共是16位，最多有2^16-1=65535字节。

->注意，网络层，数据包最大65535字节；而数据链路层数据最大是1500字节，是不一样的。所以说，一旦超过数据链路                层的最大要求时(网络层数据部分超过1480字节)，数据包会分片。最大传输单元MTU。

->数据包分片：把数据分割，分别添加IP地址，通过网络发给目标MAC地址。目标在通过网络层拼接。传送过程中可能会丢包，或者后发的先到(泪滴攻击就是利用目标机发送破坏的IP包(重叠的包货过大的包负荷)可以通过TCP/IP协议来瘫痪各种不同的操作系统)。所以需要编号。

(5)标识：如果出现数据包分片，那么标识用来确定哪些数据包是需要组合的。

(6)标志：确定该数据包是完整的还是分片中的一部分。占3位，只有前两位有用，标志字段最低位是MF(More Fragment)，MF=1表示后面还有分片，MF=0表示最后一个分片。标志字段中间一位是DF(Don’t Fragment)，只有DF=0才允许分片。

(7)片偏移：偏移等于当前字节在数据部分的第几个再除以8.(下图是一个举例)

(8)生存时间：就是TTL，time to live，每过一个路由器就减1。8位二进制。防止数据包在网络中循环。

(9)协议：用协议号标识数据部分是什么数据。ICMP协议号：1；IGMP协议号：2；TCP协议号：6；UDP协议号：17；域名解析        IPv6协议号：41；OSPF协议号：89；

(10)首部检验和：16位，只检验数据报的首部，不检验数据部分。这里不是采用CRC检验码而是采用简单的计算方法。每经过一个路由器就会检验一次。

(11)源地址和目的地址都是IP地址，32位，只符合IPv4。IPv6是128位。

(12)可变部分：一般没用。

### 4.ICMP协议(网际控制报文协议)：分为两种，分别是差错报告报文和ICMP询问报文。

差错报告报文分为：中点不可达、源点抑制、时间超过、参数问题、重定向。

ICMP：在IP之上，用来测试网络层有没有故障。使用最多的命令是ping。

为了提高IP数据报交付成功的机会，在网络层使用了ICMP(Internet Control Message Protocol)。

ICMP允许主机或路由器报告差错情况和提供有关异常情况的报告；

ICMP报文件为IP层数据报的数据加上数据报的首部，组成IP数据报发送出去。

## 3.传输层：

### 1.TCP(传输控制协议)：

#### 1.TCP特点：

(1)TCP是面向连接的传输层协议。(三次握手)

(2)每一条TCP连接智能有两个端点(endpoint)，每一条TCP连接只能是点对点的(一对一)。

(3)TCP提供可靠交付的服务。(确保不丢包)

(4)TCP提供全双工通信。(因为需要接收端的反馈，例如如果接收端处理不过来，可让发送端慢一点，流量控制)

(5)面向字节流。

#### 2.实现可靠传输的工作原理：等待停止协议(无差错情况、超时重传、确认丢失、确认迟到)

#### 3.TCP报文段的首部格式：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MGZkZmQwYjk5OTY2MTg3NDdmMzJmYjU5ZmIwY2FjNmRfWmQwTkhJTmJKNWtnemZXaHV3VDBNZ1Q5cXB3YUY0MFZfVG9rZW46Ym94Y245ZjhkeDJwU0E0TVhvRTBtYWNYVGVoXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

(1)源端口：2个字节16位(65535)

(2)目的端口：2个字节16位。

(3)序号：当前数据的第一个字节在整个文件中的序号。

(4)确认号ack：接收端发送，提示发送端下一次该发的数据在整个文件中的序号。接收端收到后，会把这个序号                之前的数据从缓存中删掉。

(5)数据偏移：当前TCP报文段第多少个字节后是TCP的数据部分了。数据偏移最多表示1111，即15，他最多可以表示15乘以4，即60个字节的偏移量，所以选项+填充最多只能是40个字节。

(6)保留：6位，无作用。

(7)URG：urgent，意思是优先级高，发送端优先发送，而不是在缓存中排队。

(8)ACK：acknowledge，1意味着确认建立了会话。

(9)PSH：1意味着接收端优先读取，而不是在缓存中排队。

(10)RST：reset，1意味着TCP会话出现严重错误，必须释放和重新连接。

(11)SYN：同步。1意味着要发起会话。

(12)FIN：finish，1意味着释放连接。

(13)窗口：接收端先发，发送端根据接收端的窗口尺寸确定发送端窗口尺寸。

(14)检验和：

(15)紧急指针：只有URG为1才有用。

#### 4.TCP如何实现可靠传输？

滑动窗口(用于流量控制)

主要包括：停止等待、后退n帧、选择重传

https://blog.csdn.net/xiaojie_570/article/details/87904328

#### 5.TCP的拥塞控制：

(1)出现资源拥塞的条件：对资源需求的总和>可用资源。

(2)拥塞控制是一个全局性的过程，涉及到所有的主机，所有的路由区，以及与降低网络传输性能有关的所有因素。

(3)流量控制往往指在给定的发送端和接收端之间的点对点通信量的控制，它所要做的就是抑制发送端发送数据的速率，以便使接收端来得及接收。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTdlMmE2ZDdhNzg5MmRiNTQwYTMyMjA4NWQyYmY1YzNfZVJlTk1YVTBVNkV1bkhTV0FLUFQ2NEliWG9WRWd3ZVJfVG9rZW46Ym94Y25oUklqeEY3NEtvZldwTGJVZVd4ZURlXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

(4)慢开始和拥塞避免

发送方维持 拥塞窗口cwnd(congestion window)

发送方控制拥塞窗口的原则是：只要网络没有出现拥塞，拥塞窗口就再增大一些，以便把更多的分组发送出去；只要网络出现拥塞，拥塞窗口就减少一些，以减少注入到网络中的分组数。

(5)慢开始算法的原理

​         设置慢开始门限状态变量ssthresh

慢开始门限状态变量ssthresh的用法如下：

当cwnd>ssthresh时，停止使用慢开始算法，改用拥塞避免算法；

当cwnd=ssthresh时，使用慢开始算法或拥塞避免算法均可；

(6)拥塞避免算法的思路

让拥塞窗口cwnd缓慢地增大，即每经过一个往返时间RTT就把发送方的拥塞窗口cwnd加1，而不是加倍，使拥塞窗口cwnd按线性规律缓慢增长。

当网络出现拥塞时，无论是在慢开始阶段还是在拥塞避免阶段，只要发送方判断网络出现拥塞(其根据就是没有按时收到确认)，就要把慢开始门限ssthresh设置为出现拥塞时的发送方窗口值的一半(但是不能小于2)。然后把拥塞窗口cwnd重新设置为1，执行慢开始算法。这样做的目的就是要迅速减少主机发送到网络中的分组数，使得发生拥塞的路由器有足够的时间吧队列中积压的分组处理完毕。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YmYzNjkyZjFiZWQ5MzdhODk5YmZiMDA3ZTM5NTEzZThfOHoyV3J2STVVTUJtQ2tpWWo3U3REZzFESEpSQVJzdTZfVG9rZW46Ym94Y25qZU5QTzE3elN2M0J5cnltMWltb2NiXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

(7)快重传和快恢复

快重传算法首先要求接收方每收到一个失序的报文段后就立即发出重复确认，这样做可以让发送方及早知道有报文        段没有到达接收方。

当发送端收到连续三个重复的确认时，就执行“乘法减少”算法，即把慢开始门限ssthresh减半，但拥塞窗口cwnd现在不设置为1，而是设置为慢开始门限ssthresh减半后的数值，然后开始执行拥塞避免算法(“加法增大”)，使拥塞窗口缓慢地线性增大。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YTkzOWYwYTMyMjlkYjE2MzA1ZmExMjkzMDJjMDk1ZTZfdEoyQXNTT2dmbUh5TGVYQ0NXc29rWGhjQjdGT3YwWWhfVG9rZW46Ym94Y25xQWxYZ0VrTlhDMWsxOERJamZkWDhmXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

### 2.UDP(用户数据报协议)：

#### 1.UDP首部格式

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=OWVlNzM1NTFhNmI4MTA3ZDBlNTg0MzAzZWJhMmFmMDlfT2lrZUZUWmtTMTFJU2EySW55MUhiTzVubm9Ha1QyQmxfVG9rZW46Ym94Y25ZTk1HR3VLdFplZnRLMlR1aTREb0JlXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

#### 2.UDP特点：

(1)UDP是无连接的，即发送数据之前不需要建立连接。

(2)UDP使用尽最大努力交付，即不保证可靠交付，同时也不使用拥塞控制。

(3)UDP是面向报文的，适合多媒体通信的要求。

(4)UDP支持一对一，一对多，多对一，多对多交互通信。

(5)UDP首部开销小，只有8个字节。

### 3.三次握手：

传输连接有三个阶段，即：连接建立，数据传送，连接释放。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YWZmZDUwY2FjZjBlMDgwNDBhNDAzOWViMzJmOGRiOGZfeUlPZHZxYXpzYU1JbFhVNHAxYVpXSHR5S1RwQ0NuOHRfVG9rZW46Ym94Y24zTDdJUjF2bkV2UEJRRUFOR1B6R0xjXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

(1).客服端向服务器端请求，SYN=1标志是请求会话，seq是发送的序号，ACK为0.

(2).服务器端向客户端回复，SYN=1标志要跟客户端会话，ACK等于1标志着同意建立会话，seq=y是服务器向客户端发送数据的序号，ack=x+1是服务器端向客户端确认已经收到了前x段内容，下次发送的序号是x+1。

(3).ACK=1表示客户端同意服务器的会话，seq=x+1是客户端发送的序号，ack=y+1表示客户端收到了发来的前y段内容，要求服务器下次发送数据的序号是y+1。

头两次握手除了确定双方都能联通外，还通知了双方的一些端口信息。第三次握手原因：假如把三次握手改成仅需要两次握手，死锁是可能发生的。作为例子，考虑计算机A和B之间的通信，假定A给B发送一个连接请求分组，B收到了这个分组，并发送了确认应答分组。按照两次握手的协定，B认为连接已经成功地建立了，可以开始发送数据分组。可是，B的应答分组在传输中被丢失的情况下，A将不知道B是否已准备好，A认为连接还未建立成功，将忽略B发来的任何数据分组，这样就形成了死锁。

### 4.四次分手：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NzZkM2EzZGRjOGQxNThlZWE4Nzc5MTBiZmVhNmFjNDhfT0FIV3I3Z0FGRmVNOVZ0YnVXam1ueHg0eTJQaDgyRU1fVG9rZW46Ym94Y25MallYU012OU9RRzRwbFZsVjFEaXFkXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

(1).客户端向服务器发送FIN=1表示要断开连接，同时它的发送序号是seq=u，客户端进入FIN_WAIT_1的状态。

(2).服务器向客户端发送确认标志位ACK=1表示收到了FIN的标志，ack=u+1表示要客户端下次发送序号是u+1的数据，seq=w表示它发送数据的序号是w，服务器进入CLOSE_WAIT状态，客户端进入FIN_WAIT_2状态。

(3).发完之后，服务器再向客户端发送FIN=1表示同意关闭连接，确认位ACK为1，服务器发送的数据序号seq是w，ack=u+1表示要客户端下次发送数据的序号是u+1，服务器进入LAST_ACK状态。

(4).客户端向服务器发送ACK=1确认标志位，seq=u+1表示自己发送的数据序号是u+1，ack=w+1表示要求服务器下次发送的数据序号是w+1，客户端进入TIME_WAIT状态，服务器收到后进入CLOSED状态。客户端在等待2个MSL后，证明服务器关闭了，客户端随后也进行关闭。

## 4.应用层：

1.网络(NETWORK)和服务器基础

https://blog.csdn.net/weixin_39258979/article/details/80835555

### 1.Socket

开发网络应用网络进程的API函数，使用 socket pair描述网络中的一对一的关系。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDhkYzg0OWY5OWZhZTE3OWRhOTFmZDAxY2ExMTNlNjFfa0JYNHFpR0JaQjRScjhpMkJ0MUxPRWpLdmlNMUNURUlfVG9rZW46Ym94Y244OUpodlVRTXV6U2ZkY2E4bXYwMkVkXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

```C%2B%2B
socket();//创建socket的文件描述符
bind();//绑定指定的ip和端口号
listen(int sockfd, int backlog);//监听sockfd端口 backlog最大连接数 最大为128
connect();//发起建立连接(主动)
accept();//等待建立连接(被动)
inet_pton();//将点分十进制的IP地址转成二进制的数
inet_ntop();//将二进制的数转成点分十进制的数
// getsockname函数用于获取与某个套接字关联的本地协议地址 
// getpeername函数用于获取与某个套接字关联的外地协议地址
int getsockname(int sockfd,struct sockaddr* localaddr,socklen_t *addrlen);
int getpeername(int sockfd,struct sockaddr* peeraddr,socklen_t *addrlen);
```

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDFjOGQ0NWQ3Y2E3ODY3NDRkN2E3NTQyZDFjZmU0ZWNfbWhrTW1yelVKOGFyb1p6dFRPNUFwYUJKN0hBdmpCVmZfVG9rZW46Ym94Y243SGl6MTZuRjVORkZRbzVLblowVHdmXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Zjk4OTI1MmE4YWU5MzU0OGU0ZmQ0YzQxZGY3YjhkMDhfSld1VUllYm5ic1l6b0M3M2hrNjhsb011YXNXVnhocTFfVG9rZW46Ym94Y253T1ZkRE9TMjgwZENLTzVCVk1nMlBkXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MjExZjFmZTg2NjIwNmVhMWI5NGNkZjkwZjE2YjJlY2JfTkJOV3l4cGJLUWdwZE5JM3A1aGxXNEZzN3lONG1zRlZfVG9rZW46Ym94Y25uQmtPT3VWYVJuc0pBYkg2bXNvWk9mXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

recvfrom和sendto是UDP主要用到的函数

read/write/recv/send是TCP主要用到的函数

### 2.Socket选项

https://blog.csdn.net/tianxuhong/article/details/86659887

### 3.套接字状态

https://blog.csdn.net/yss28/article/details/55101084

### 4.服务器基础

服务器类型：文件服务器        数据库服务器        负载均衡服务器        处理服务器

服务器基本特性：1.数据中转能力        2.较强的并发能力                3.网络特征

服务器的主要职责：1.让若干客户端建立联系，实现数据共享(网络穿透)

#### 4.1.持久化能力和数据处理能力

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YWRhYmM0NzM4NTY1NDM4ZTZhMjcwYmM4MTA5NjZkMTJfMlpDMURMZGJvS0NmU0ZuMmdod3lDb25Cd2FqWFlvQUJfVG9rZW46Ym94Y25zWGdpem1Lc1NEclVPZ3pqYmdBTGtoXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

服务器并发(一对多的处理能力)：一个进程或一个线程开发服务器程序(能完成一对多处理)并发服务器。

基础的Demo由于存在堵塞无法实现并发：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MTJlOTVmZjRhNzM5YzU5ZmMyNWUxNjNjODRiNjAyNmZfS0NoQUlKUnRIeHJzdnBRMHJ6akdESk43NVBlRVFHZGFfVG9rZW46Ym94Y25IeXlvWThtWklYMThvZ3RlcmNhS3loXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

采用多线程并发技术让服务器有并发处理能力：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YmZlYTBmYjQxZmUzZWQ4ZmMyOTk4M2QyYzYxZTNiNmVfNzhiMnNyS0xuN0pkOGxmMmZuQ2RGeTFVOUZkTmdUOVJfVG9rZW46Ym94Y24zY3FYdFZtUlpxYlRHRFFLOFYxZlJkXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

多线程模型回收问题(在父进程中创建两个线程，一个主控线程，一个普通线程，主控线程负责accept()接收客户端的连接，普通线程负责回收业务完成的子进程)：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NTZhZjNlY2JlNDM2NDljNTEwNzhhODY4ZGJjMTE2YjdfbXFXdUh5ak9LaXdhYUlYV2k1MkFwWlFkUGswcEY2QlRfVG9rZW46Ym94Y25sUTZuMG1tRDlDWGJLd3BTR2RpSURjXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

多并发服务器总体架构：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTE1Mzc5NzE2YzAxZjFhZjE1YmRhOTQwOWViODhiM2RfZEtUSkJ4SnoycXBTcERpaFlob3NaYVBQUmZvYW54MEpfVG9rZW46Ym94Y25wOWpLTFJQWGtlV1NBWVFkODZCdzJkXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

#### 4.2.多路IO复用/多路IO转接(用于监听多路读写事件)

主要模型有 select         poll                 epoll        完成端口        Kqueue等

主要类别有同步阻塞、同步非阻塞、IO多路复用和异步IO

同步：等待事件，处理事件获取结果，同步分为同步阻塞和同步非阻塞

异步：直接返回处理后的事件结果

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTY1NjFiMjYxZGUyMDU5NWM2M2ZiMjkwY2QzMTljMzNfbTl2TGZaeUhrWTNMbFJURW01d0hXeHZtM1ZxWlFpUm5fVG9rZW46Ym94Y25ZRWlib0ZZNlE0eE85dExpV3ZYYnpiXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

#### 4.3.select模型

##### 1.监听集合

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjlmODcyM2IxZWMzNjZiMzU5ODlkODE5NzdlNWU2NmRfMjZxWVhkTURkNjA3cXVleTFoUEY0clNoaWdhT2kzU3FfVG9rZW46Ym94Y25sZVUyb3hlVlc2cXZBeU90cEdqRTFiXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

监听集合中存的是监听的socket和监听选项  0是非监听 1是监听，集合最大为1024，因此最大并发数就是fd_set的最大值1024。

##### 2.select的流程

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MzBmMDA5MzNjN2I0MzBlZTg2ZDczMWJkNjI5NzVhMjhfdElRV2pCMnpSTFZEQ21xQ3B5TW4za2lvREVOUzhkcTVfVG9rZW46Ym94Y25sWU1FeGhPU0d4eGh2S0RuVVFWT2JiXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MjVhMWQ4ODA4OGM2NDIzNTEzNjhjMTZhMTMyY2UyMmRfdmdEb2JRZ3hWQnJqc1RqT1dnZ0xRMUNCMEFYSHdBY3NfVG9rZW46Ym94Y25JVmdLdEdjWXpzYjBoQ0FwbjA3b3MzXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

##### 3.如何在查找集合中找出就绪的socket

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NzU3YTRmMzgwYzMyZGJjNmIwNzUyNTA3NDUxZWNhZmJfYlhzeEVnandlQWg3RVc1V2J0V3huUkw4NXl2THpVOHhfVG9rZW46Ym94Y24zTGtlTUdJOG92WFlLUnh2YWwyd0xjXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

oset                                                nset

传入的是监听集合，传出的是就绪集合，传出的时候，只会将就绪的socket对应的位保留为1，未就绪的则置为0。要人为的将传入和传出分离。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=Njc5MDEzMGY2YmE4YWVkNmI0NWEzOWFkNTM2NGYwNzBfMEQwQjYzb1Rhb21sYmdJcHBrTWlnZ2hsbVJrWWNVckJfVG9rZW46Ym94Y25QaTNWcUxqZmZqaEVqTVBxbTBwTXJjXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

用到的函数：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YmY0NmYwM2NhYTM0MzE1ODE1NmIxMmIyYTU1ZGY1N2NfcVVzdDVxN1o2bFVGMHdMSnFBelRiUTZvNk5Fa3ZmVG9fVG9rZW46Ym94Y241WDc0ajBKcURpeE5qTUJnakxxdEhkXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

代码见superLinux /006_NetWork/08_Select.c           

https://github.com/leexsh/unp

##### 4.socket模型的利弊：

###### 4.1优点：

- 兼容性强，有较强的跨平台能力和移植能力

- 如果开发环境中对时间的要求精度较高，select可以满足。

- 使用select实现较为简单，工作量小。

###### 4.2缺点：

- 监听数量限制1024， 不能满足大部分的要求。

- select没有将传入和传出分离，用户每次执行要自己设置

- select只返回就绪的数量，用户需要大量的工作来寻找就绪的socket并进行处理，开销大并且无意义。

- select采用轮询的方式过滤访问监听的集合，随着监听数量的增大，开销数量呈线性增长，处理效率低下。

- 单线程select需要逐个处理事件任务，如果任务处理时长导致响应缓慢，可以缩短任务处理时间(变更任务复杂度)。

- select产生大量的无意义系统开销(重复拷贝)。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=OTk0ZTYxNDg0NzM4MWM3NDZkMzUwNWI3Y2JiMjk2YWZfdU1OZ0pscEs4MmptR2s3ZnFXMGRVdFl3anZVVFFFb1dfVG9rZW46Ym94Y25QNFdOWTlCWm1rY0oxY3FpU2tUYkI4XzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

产生无意义开销示意

#### 4.4.poll模型

##### 优点：

突破了1024个限制

将传入和传出分离了

##### 缺点：

同select模型

见：https://github.com/leexsh/unp

#### 4.5.epoll模型

##### 1.epoll模型的使用

创建监听集合：

```C%2B%2B
int epfd = epoll_create(int size); //参数是监听集合的大小 返回是监听集合的文件描述符
```

 设置监听节点信息： 

```C%2B%2B
struct epoll_event tmp;//监听结构体
tmp.data.fd = serverfd;//设置监听项
tmp.events = EPOLLIN;//监听读事件
```

  将监听节点加入到监听集 合：

```C%2B%2B
//epfd 监听集合的文件描述符  fd是监听项的fd 最后一个参数是上面的结构体
epoll_ctl(int epfd, int opt, int fd, struct epoll_event *);
//opt有三个选项：EPOLL_CTL_ADD(增加) EPOLL_CTL_MOD(修改) EPOLL_CTL_DEL(删除)
```

阻塞监听

```C%2B%2B
 // 返回就绪数量
int ready = epoll_wait(epfd, 就绪队列，最大就绪数， -1);//0非阻塞 大于0定时堵塞 小于0阻塞
```

根据就绪数去遍历就绪队列即可。

##### 2.epoll模型的利弊：

###### 2.1优点：

- epoll模型没有监听数量限制，理论上设备能创建多少文件描述符，epoll就监听多少个，没有额外的开销

- epoll将传入和传出分离，传入监听集合，传出就绪队列。

- epoll不仅返回就绪数量，还返回就绪节点队列，用户只要依次遍历就绪队列处理即可。

- epoll代码简单，思路清晰

- epoll没有任何轮询开销

epoll为了避免轮询，没有使用内核IO设备等待队列，底层实现了自己的IO监听队列，如下图，直接将就绪节点映射到就绪队列。

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MDU4ZTUxODk1MzI2ZDdlMGQxMDEwNGQ4YWJlMTcwNzRfV0d1M1RxOXlTTVhLV2xXVzIxUXphU3RJRHhpS2dOVnVfVG9rZW46Ym94Y24wZzlSNjNKVFNPaHp0Mm93YWhrY09xXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

- 所有的监听节点，只会像内核拷贝一次，并且只向设备上挂载一次

(实现原因：epoll模型的监听集合是在内核空间创建初始化的一棵红黑树，每次添加新的监听节点，用户在用户空间准备好以后，将该节点拷贝到内核空间挂载，所以可以避免多余的拷贝开销)

- epoll模型采用mmap(内存共享映射)方式对节点进行传出，减少拷贝开销。

###### 2.2缺点：

- 不具备跨平台的能力

- 处理效能上epoll和select、poll没有任何区别，可能会导致响应缓慢

- 在某种特定的条件下，epoll模型的任务处理效能会线性下降，甚至低于select和poll模型。(原因：红黑树进行频繁的访问的时候，可能会增大开销，处理效能降低)

#### 4.6.线程池服务模型

##### 1.如何将多线程服务器并发模型改造成服务器模型：

- 线程池可以最大化的提高线程的重用性(避免频繁创建和销毁)

- 对线程数量设置上下限，保证系统稳定

- 线程管理，对线程池内部的线程进行状态统计(记录忙闲线程)

##### 对线程池内部的线程进行动态的扩容和缩减(根据忙闲统计，增加线程或减少线程)

- 线程池拥有完成的任务传递与任务执行的过程

- 具有灵活的任务接口(用户根据该接口，自定义的实现任务，然后线程池帮助传递和执行)

线程池内部的阈值是控制线程池内部线程的主要手段。

##### 2.线程池的实现

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NTMwODM4Nzk2MGIzNjMzOWQwMTFlOTQ1NjQyOGEyZmRfTkxmVW96aGo0UG9jemdka2RBeHpQWE1xSDN3S0I2aXpfVG9rZW46Ym94Y25XMnVwaGt0WFBDVVQ3bHdoNjNZaW5iXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

##### 3.线程池的工作流程

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=YmUzYjM5ZjEwYTVhZDAwZGZjNmJkOTk2YzRiOWJmMjhfaG0xekZERlV1bjd5Z1FYaXZ1N3poREx2OVpzNmRrU3BfVG9rZW46Ym94Y25kVTZha1JGWklTTVZZczFHcm9uVmRoXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

生产者生产一个任务后，唤醒消费者，消费者消费一个任务之后唤醒生产者。

##### 4.管理者线程工作流程：

​    

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=ODM1M2Q4ODZlZjliM2MwY2Y4ODc0Njg5ZjU0MzIxZmFfOFdjZUQzSXBFbWpseWFxSVFlZFhjell1M0xlZEEyQXBfVG9rZW46Ym94Y25tdDdScU5MUUlRZjM1VWI0TDlrUWhlXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=M2QyOGU2MjA0ZjQyZGZlYWEwMzRkYzcxNDY2ODllOTJfZ2I0MlBpY3JFbzlBRXJrVGprcWVwb2p5SlVhb1IzWmxfVG9rZW46Ym94Y250WXROVzlLNmZsQjFDd0hqdlhKOHplXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

##### 5.加入服务器并发性：

​    

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=NGVkOTRkOGYyOGM4NGY1MmFhOTU2Yzk3OWJmNjU2OWFfM2dxV29uZklJQ0FXUVFuQ1ZpWTI4ZDFIcURvaHhnWTlfVG9rZW46Ym94Y25xdXlMQ2o5SlRKczh2UXh5bzdmN09oXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)

##### 6.经典的线程池服务器并模型：

![img](https://a46xfr1zy5.feishu.cn/space/api/box/stream/download/asynccode/?code=MzBhZWZhZDVjYzIxODU4MzgxYjc5NzkxZDRkNmZiZWVfcWVQS0ZJcEpTbXM1amQwRDVRMWdkRlRaZGtVNmVJbTdfVG9rZW46Ym94Y241TGRwREtabDlqbk1NZTNuTFpETVBiXzE2NDcxODIzOTI6MTY0NzE4NTk5Ml9WNA)