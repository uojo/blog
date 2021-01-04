# Chrome Devtool Network 指标说明

## Network 列表

### 表头

- Type：文件的 MIME Type 类型
- Initiator：请求发起者

## Network 列表内单条记录

### Initiator（Tab）：请求发起来源（调用栈）

### Timing（Tab）：请求与加载情况

```
Queued at 3.05 s  # 进入（开始）等待队列的时间
Started at 3.05 s  # 开始阻塞的时间

Resource Scheduling  # 资源调度
  Queueing          1 ms  # 放入队列内持续时间（Started 减去 Queued）

Connection Start  # 建立连接
  Stalled           2 ms  # 从等待队列到开始发送之间的持续时间

Request/Response
  Request sent      0.28 ms  # 总的数据上传时间。从第一个字节发出前，到最后一个字节发出后的时间间隔
  Waiting(TTFB)     10 ms  # 请求发出后，收到响应的第一个字节，所花费的时间(Time To First Byte)
  Content Download  1 ms 下载持续时间。从收到响应的第一个字节，到接受完最后一个字节的时间。
```

#### Queueing

还未收到可以建立连接的指令，在等待队列中停留的时间。

##### 出现 Queueing 的原因

1、存在更高优先级的请求。关键资源较高，例如脚本、样式，图片资源通常会遇到这种情况。

2、当前域名已打开六个 TCP 连接，达到限值，仅适用于 HTTP/1.0 和 HTTP/1.1。

3、生成磁盘缓存条目所用的时间（通常非常快）。

#### Stalled

收到浏览器可以建连的指令，从等待队列中取出，到可以发送请求的等待时间。

##### 出现 Stalled 的原因

除了遇到 Queueing 中介绍的任何一个原因外。还包括：

1、网络或服务不稳定。

TCP 三次握手时，发送端发送数据后，一段时间内（不同的操作系统时间段不同）接收不到服务端 ACK 包，就会以某一时间间隔(时间间隔一般为指数型增长)重新发送，从重传开始到接收端正确响应的时间就是 stalled 阶段。而重传超过一定的次数（windows 系统是 5 次），发送端就认为本次 TCP 连接已经 down 掉了，需要重新建立连接。

##### 总结

通常，造成 Queueing 与 Stalled 的原因：

- 网络或服务端问题。
- 单个域名的并发连接数超出限制。
- 被关键资源的优先级抢占。
- 脚本或样式正在解析、执行。

## 参考

- https://developers.google.com/web/tools/chrome-devtools/network/reference?utm_source=devtools#timing-explanation
- https://www.cnblogs.com/both-eyes/p/10573713.html
- https://www.cnblogs.com/ywsoftware/p/10996078.html
- https://www.cnblogs.com/kjcy8/p/6802203.html
