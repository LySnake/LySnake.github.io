---
title: TCP包的思考
comments: true
categories:
  - archives
  - technology
tags:
  - TCP
  - 网络
date: 2018-04-18 22:13:12
updated: {{ updated }}
description: 关于TCP包长链接，连接send二个包达到接收端会不会乱包
---

### 问题来源 ###

**继二天学了Lua后，开始查看Lua项目源码，此时丢给我一个Bug，通过边查看代码边学习，发现这个Bug存在一种可能，就是"S端与C端以TCP长链接通信，当S端连续send二个数据包AB给C端，C端应用层如果是先接收并处理BA，则会产生这个Bug。"，会产生这个疑问的原因是，我们知道IP包可以出现丢包、包错误、路径不一致等原因，导致到达时间的不确定，而此项目中的每个TCP包，都是很小的一个包，一个TCP数据可以不分包直接丢给IP包传送。所以我认为这二个TCP包可能会有接收上的先后关系。而以前查看的网络书籍，并没有注意到这点，更多的是说当一个大的TCP包，通过IP包传输时，因为TCP包很大，所以会发成很多小包通过IP包传输，接收端的TCP层再根据TCP小包的序号重新排列数据，达到数据不混乱的目的。而此时是二个TCP包，每个包也都很小，而这个Bug也确实存在一定的随机性，本地复现基本不可能，而生产环境目前并无法获得更多信息。而查看代码逻辑目前仅能查看到这个点。**

### SEQ与ACK ### 

[TCP的SEQ和ACK的生成](https://blog.csdn.net/haitun312366/article/details/9060253)。存在我的疑问的原因就是，我认为SEQ是每次系统调用write都会产生新的，而实际上因为好几年没有这么细的接触过网络问题了，这点已经忘记了，而实际上，一个TCP连接建立，就会完成SEQ与ACK的初始化，并在连接过程中一直使用更新。所以对于长连接无论send几次，与TCP大包分小包传输一样，通过SEQ与ACK实现有序化，因此接收端的应用层一定会按序接收到发送端应用层发送的有序数据。

### PUSH ###

**PUSH**：`PUSH是TCP报头中的一个标志位，发送方在发送数据的时候可以设置这个标志位。该标志通知接收方将接收到的数据全部提交给接收进程。这里所说的数据包括与此PUSH包一起传输的数据以及之前就为该进程传输过来的数据`。这段话在网络与<TCP/IP 卷一>里都有，在我这个TCP长链接，并且每次发送的TCP包都很小的情况下，而且当每个TCP数据包都带PUSH标识时，当S发送TCP包AB，接收端如果先接收到B，再接收到A，虽然都有PUSH标识，但因为B的SEQ检查的情况表明在B前面还有一段数据，此时不能通过B的PUSH标识就把数据交给应用层，因为无法得知仅B就是一个完整的应用层数据了。而我接触的都是游戏相关，所以每个send的TCP包数据都带PUSH标识。

#### 引用下相关资源并整理 #### 

[TCP PUSH参考资料1](https://blog.csdn.net/edison0716/article/details/5134101)与[TCP PUSH参考资料2](http://www.vants.org/?post=102)。

**内核处理**

```
所谓的send buffer,意即发送缓冲区，你看的是bsd的源代码，我这边就以Linux的源代码回答你了，反正原理是相通的。在代表网络层socket的结构体struct sock中有几个关于发送缓冲区的成员数据，其中sk_wmem_queue即为发送缓冲队列，是一个由结构体struct sk_buff组成的队列，一个结构体struct sk_buff承载一个tcp数据报，对于应用程序发送的大应用数据，比如一个write写8K字节的数据，由于TCP协议有MSS(最大报文段长度)的限制，假设MSS为1024字节，则这8K会被拆分成8个1024随载数据的TCP数据报，排列在缓冲队列中，然后一次全部发出，最后一个数据报带上PSH标志，告诉对端，把收到的数据赶紧传给应用层，因为这已经是一个完整的应用数据了。

同样，你8个write，每个写1024数据，协议栈就理解为这是8个完整的应用数据，所以每个都需要带上PSH，告诉对端，这都是完整的应用数据，需要尽快传给应用层。
而这跟滑动窗口大小无关。

end buffer即发送缓冲区的意思，进程通过Write系统调用，将数据报写入发送缓冲区中，而缓冲区中数据报的发送工作交给TCP内核协议栈执行。内核发送缓冲区数据报的时候，所发送数据报的大小受MSS、通告窗口大小影响，这里MSS〉=1024，而且通告窗口大小为4096，也大于1024，所以执行一次write即为一次写操作将1024大小数据报写入发送缓冲区，然后调用内核中的发送函数直接将这1024大小数据报发送出去，由于该发送操作会使发送缓冲区变空，即发送发送缓冲区里所有数据，即empties the sender buffer，所以要给该数据报加上PUSH标志。
```
##### TCP的交互式数据流 #####

**经受时延的确认技术**

```
TCP的交互式数据流通常使用“经过时延的确认”技术。通常Server在接收到从Client发送过来的数据时，并不马上发送ACK，而是等一小段时间，看看本机是否有数据要反馈给Client，如果有，就将数据包含在此ACK包中，以前发送给Client。一般情况下这个时延为200ms。需要注意的时这个 200ms的定时器时相对于内核的时钟滴答的，也就是jeffs的。加入一个数据分组到达后，此定时器已经pass了100ms，那么再过100ms ACK才会被发送，如果在这100ms内有数据要反馈，则在100ms后ACK会和数据一起发送。
```

**Nagle算法分析**:主要用于TCP的交互式数据流，每一个TCP数据包都能完整的给IP包，对于实时的游戏数据则会关闭Nagle算法来提高数据的实时性。

```
Nagle算法主要用来预防小分组的产生。在广域网上，大量TCP小分组极有可能造成网络的拥塞。

Nagle时针对每一个TCP连接的。它要求一个TCP连接上最多只能有一个未被确认的小分组。在改分组的确认到达之前不能发送其他小分组。TCP会搜集这些小的分组，然后在之前小分组的确认到达后将刚才搜集的小分组合并发送出去。

有时候我们必须要关闭Nagle算法，特别是在一些对时延要求较高的交互式操作环境中，所有的小分组必须尽快发送出去。

我们可以通过编程取消Nagle算法，利用TCP_NODELAY选项来关闭Nagle算法。
```

##### TCP成块数据流 #####

**PUSH**

```
PUSH是TCP报头中的一个标志位，发送方在发送数据的时候可以设置这个标志位。该标志通知接收方将接收到的数据全部提交给接收进程。这里所说的数据包括与此PUSH包一起传输的数据以及之前就为该进程传输过来的数据。

当Server端收到这些数据后，它需要立刻将这些数据提交给应用层进程，而不再等待是否还有额外的数据到达。

那么应该合适设置PUSH标志呢？实际上现在的TCP协议栈基本上都可以自行处理这个问题，而不是交给应用层处理。如果待发送的数据会清空发送缓存，那么栈就会自动为此包设置PUSH标志，源于BSD的栈一般都会这么做，而且，BSD TCP STACK也从来不会将收到的数据推迟提交给应用程序，因此，在BSD TCP STACK中，PUSH位是被忽略的，因为根本就没有用。

PUSH位就是用来通告接收方立即将收到的报文连同TCP接收缓存里的数据递交应用进程处理。一般会出现在发送方封装最后一个应用字段的TCP报文中，针对TCP交互式应用，则只要封装有应用字段的TCP报文，均会将PUSH位置一，当然，应用程序的开发者，可以根据需要，在某个应用功能模块或某个应用操作时，将所有封装应用字段的TCP报文PUSH位置一，以提高交互双方的处理效率，这在理论上应该也是可行的。
```

## 后感 ## 

当初学习计算机网络的时候，是借了计算机专业同学的《计算机网络原理》阅读一遍，另外也是看了《鸟书第二部》网络部分，以此建立的网络基础，但其实通过这次事件来看，对于网络来说，仅知道个大概，但对于如何实现的细节问题并不清楚，比如说本次的SEQ是针对一个TCP链接的而不是一个TCP包，SEQ与ACK之间的生成与更新方式，以及其它标识位、网络相关的算法及相关场景等。这些信息在《TCP/IP详解 卷一》里都有相关的描述。如果能对于本书与实际操作起来，应该会好很多。