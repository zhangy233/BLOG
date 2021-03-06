# TCP协议的三次握手和四次挥手

## 三次握手的动作及作用
![三次握手](https://raw.githubusercontent.com/zhangyu012/picture_picgo/master/blogImg/20180717202520531.png)


### 三次握手的意义?

答: 三次握手实际就是指建立一个TCP连接时服务端和客户端发出的三个报文; 主要是为了确认双方发送接收能力, 确定初始话的序列号(第一个SYN中就携带了序列号).

### 为什么三次?

* 第一次握手: 客户端发送SYN号, 确定字节的序列号

* 第二次握手: 发送SYN+ACK, 确定收到报文, 并指定了自己的ISN; 返回的ACK总是对端的序列号+1

* 第三次握手: 表示收到确认.

* 进行三次握手后, 双方各发送了一个报文并且受到了响应, 进入ESTABLISHED状态.

* 如果仅两次确认连接, 有可能出现: 比如客户端发出一个连接请求, 但是超时了于是发送了两次, 第二次的报文建立的连接正常关闭了; 然后第一次的报文有可能延误到之后再收到, 如果服务端不做第三次握手直接建立连接的话, 就会一直等待客户端发送报文了.

### 三次握手是否可以携带数据?

答: 最后一个ACK可以携带数据, 并且携带数据的话其需要需要+1; 第一次是不可以的, 防止攻击.

## 什么是半连接队列?

答:  服务端收到SYN报文后会进入SYN_REVD阶段, 处于该阶段的连接均放在一个队列中称为半连接队列; 当然也有全连接队列.

## ISN是固定的吗?

答: 是一个32bit的计数器, 每4ms加1, 随机生成. ISN交换也是三次握手的功能之一, 当然是随机的避免攻击.

## SYN 攻击是什么?

答: 	服务端当接收到一个SYN后就会建立半连接回复一个SYN/ACK等待响应, SYN攻击就是伪造源IP地址发送SYN, 由于源地址是假的导致服务端不断重发SYN直到超时引起阻塞.

## 四次挥手
![四次挥手](https://raw.githubusercontent.com/zhangyu012/picture_picgo/master/blogImg/20180717204202563.png)

### 挥手为什么需要四次?

答: 因为服务端和客户端并不同步, 一方发出FIN结束信号进入`FIN_WAIT1`状态, 但是另外一次可能还在处理信息并不能关闭连接; 所以回复的只能是ACK, 等待服务端回复FIN才表示停止数据流动. 三次握手只不过是, 服务端发出的SYN/ACK合并在一起了.

### 2MSL等待状态(TIME_WAIT)?

答: MSL表示一个报文的最大生存时间, 可以防止最后一个ACK丢失(FIN超时重发). 在该时间内, 该IP+Port不能被使用.


