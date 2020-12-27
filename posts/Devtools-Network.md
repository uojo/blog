# Chrome Devtool 内的 Network 指标说明

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
  Queueing          1 ms  # 放入队列内持续时间（Started-Queued）

Connection Start  # 建立连接
  Stalled           2 ms  # 阻塞的持续时间

Request/Response
  Request sent      0.28 ms  # 总的数据上传时间。第一个字节发出前到最后一个字节发出后的时间
  Waiting(TTFB)     10 ms  # 请求发出后，到收到响应的第一个字节所花费的时间(Time To First Byte)
  Content Download  1 ms 总的数据下载时间。收到响应的第一个字节，到接受完最后一个字节的时间
```

#### Stalled

浏览器街道发出请求的指令，再等待队列中取出，到请求可以发送的等待时间。

##### 引发 Stalled 的情形

1、网络或服务不稳定。

TCP 三次握手时，发送端发送数据后，一段时间内（不同的操作系统时间段不同）接收不到服务端 ACK 包，就会以某一时间间隔(时间间隔一般为指数型增长)重新发送，从重传开始到接收端正确响应的时间就是 stalled 阶段。而重传超过一定的次数（windows 系统是 5 次），发送端就认为本次 TCP 连接已经 down 掉了，需要重新建立连接。

2、浏览器对同一个主机域名的并发连接数达到上限。因为当前的连接数已经超过上限（Chrome 连接数是 6 个），所以其余请求就会被阻塞，等待新的可用连接。

3、脚本执行会阻塞其他资源的下载。

##### 总结

1、单一服务发送时 stalled 过长，往往是丢包所致，这也意味着网络或服务端有问题。

2、多个服务并发时 stalled 过长，是浏览器对同一个主机域名的并发连接数有限制，过多的请求会被阻塞，处在队列中等待 tcp 连接

## 参考

- https://developers.google.com/web/tools/chrome-devtools/network/reference?utm_source=devtools#timing-explanation
- https://www.cnblogs.com/both-eyes/p/10573713.html
- https://www.cnblogs.com/ywsoftware/p/10996078.html
- https://www.cnblogs.com/kjcy8/p/6802203.html
