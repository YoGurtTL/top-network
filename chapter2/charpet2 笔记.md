#### 2.1应用层协议原理

**应用程序体系结构**

客户-服务器体系结构 客户之间不直接通信 通过服务器进行通信

![image-20211014155048912](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014155048912.png)

P2P结构 应用程序在间断连接的主机对之间使用直接通信，这些主机对被称为对等方。自扩展性

进程通信

进程与计算机网络之间的接口

- 套接字 ![img](https://api2.mubu.com/v3/document_image/eb29d1b5-f74f-4095-bb16-ca2ed8c00938-4246198.jpg)
- 进程寻址
  - 除了发送地址以外还需要发送端口号

可供应应用程序的运输服务

- 可靠数据传输  安全地交付给该应用程序的另一端

  - 运输协议不提供可靠数据传输时，可以被容忍丢失的应用接受

- 吞吐量

  - 运输层协议能够提供确保可用的吞吐量

  - 带宽敏感的应用

  - 弹性应用

- 定时

- 安全性

  ![image-20211107213334580](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211107213334580.png)

因特网提供的运输服务

-    UDP

  - 轻量级运输协议

  - 不可靠数据传输服务

-   TCP

  - 面向连接

  - 可靠数据传输

  - 拥塞控制

因特网运输协议所不提供的服务

- 定时和吞吐量，因特网运输协议并没有提供



应用层协议 定义了运行在不能端系统上的应用程序进程如何相互传递报文

- 交换报文类型

- 各种报文类型的语法

- 字段的语义

- 一个进程何时以及如何发送报文，对报文进行响应的规则

  

#### 2.2 web和HTTP

##### web术语

​	web页面 ：由对象组成。

​	web浏览器：HTTP的客户端

​    web服务器：实现了HTTP的服务端

​    

##### HTTP概括

web的应用层协议是超文本传输协议。由两个程序实现：客户端和服务器端。

![image-20211014135048708](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014135048708.png)

HTTP的支撑协议是TCP

服务器不存储任何关于该客户端的状态信息，所以HTTP服务器是一个无状态协议

###### 2.2.2 非持续连接和持续连接

> 非持续性连接：每一个请求或者响应都经过一个单独的TCP连接发送
>
> 持续连接： 所有的请求以及响应经相同的TCP连接发送

HTTP：默认情况下使用持续连接，可以选择非持续连接或者持续连接。

**1.采用非持续连接的HTTP **

![image-20211014142351493](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014142351493.png)

往返时间（RTT） 2个RRT+文件传输

缺点：

1. 每一个请求需要建立和维护一个全新连接，web服务器带来了严重的负担
2. 一个对象经受两倍的RTT的交付时延，一个RTT创建TCP，一个RTT用于请求和接受一个对象

**2.采用持续连接的HTTP**

位于同一台服务器的多个web页面从该服务器发送给同一个客户时，可以在单个持续TCP连接上进行。经过一段时间间隔任然未被使用，HTTP服务器就关闭该连接



###### 2.2.3HTTP报文格式

**请求报文**

![image-20211014144602430](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014144602430.png)

  ASCII文本书写

请求行：方法字段，URL字段 和HTTP版本字段

HOST:  指明对象所在的主机

Connection：close 不希望麻烦低使用持续连接,要求服务器在发送完被请求的对象后就关闭这条连接

User-agent：指明用户代理，即服浏览器类型

Accept-language:想得到对象的语言版本

![image-20211014145539563](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014145539563.png)

**响应报文**

![image-20211014145851086](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014145851086.png)

初始状态行   首部行 实体类

###### 2.2.4 用户与服务器的交互：cookie

cookie技术有4个组件：

1.  HTTP响应报文中的一个cookie首部行

2.  在HTTP请求报文中的一个cookie首部行

3. 端系统保留一个cookie文件，并由用户的浏览器进行管理

4. 位于web站点的一个后端据数据库

   <img src="https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014150853193.png" alt="image-20211014150853193" style="zoom: 200%;" />

##### 2.2.5 web缓存（代理服务器）

![image-20211014151447052](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211014151447052.png)

web缓存器是服务器同时又是客户

配置web缓存器的原因：

1. web缓存器可以大大减少对客户请求的响应时间
2. web缓存器能从整体上大大减低因特网上得web 流量，从而改善所有的应用性能

2.2.6 条件GET方法

允许缓存器证实它的对象是最新的

使用条件GET方法的方式：1.GET方法  2.请求报文包含一个IF-Modified-Since，缓冲器为了检查IF-Modified-Since，会向Web服务器发发送一个条件GET执行最新检查、web服务器会响应报文，但是没有对象，因为会很浪费宽带。具体请查看书P77页 

#### 2.3文件传输协议：FTP

 为使用户能访问它的远程账户，用户必须提供一个用户标识和口令，在提供了这种授权信息后，用户就能从本地文件系统向远程主机文件系统传送文件。

![image-20211015144727125](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015144727125.png)

FTP使用了两个并行的TCP连接来传输文件，一个是控制连接，一个是数据连接

控制连接：两主机之间传输控制信息，是持续的的

数据连接：实际发送一个文件，是非持续的



![image-20211015145232058](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015145232058.png)

>   HTTP：是在传输文件的同一个TCP连接中发送请求喝响应首部行的。因此是带内发送控制信息的

FTP服务器必须在整个会话期间保留用户的状态，将特定的用户账号与控制连接联结起来，随着用户在远程目录树上徘徊，服务器必须追踪用户在远程目录树上的当前位置。

##### 常见命令

![image-20211015150927799](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015150927799.png)

每个命令对应一个从服务器发向客户的回答，回答是一个3位的数字

![image-20211015151054525](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015151054525.png)

#### 2.4 因特网中的电子邮件

异步通信媒体

因特网电子邮件系统和他的关键组件

![image-20211015151603103](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015151603103.png)

用户代理：允许用户阅读，回复，转发，保存和拽写报文

邮件服务器：（核心）：每个接收方在其中的某个邮件服务器上由一个邮箱。

如果发送方的邮件服务器不能将邮件交付给接收方的服务器，则发送方的邮件服务器在一个报文队列中保持该报文并在以后尝试再次发送

简单邮件传输协议

##### 2.4.1 SMTP （因特网电子邮件应用的核心）

  SMTP：用于从发送方的邮件服务器发送报文到接收方的邮件服务器

  它限制所有邮件报文的体部分，只能采用简单的7比特ASCII表示。

![image-20211015154235871](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015154235871.png)

![image-20211015163253431](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015163253431.png)

SMTP传送邮件之前，需要将二进制多媒体数据编码为ASCII码，而HTTP传送前不需要将多媒体数据编码为ASCII码

SMTP使用TCP连接，如果发送邮件服务器有几个报文发往同一个接受文件服务器，他可以通过同一个TCP连接发送这些所有的报文。对每个报文，该客户用一个新的mail from ： xxx 开始，使用独立的句号表示结束。



##### 2.4.2 与HTTP的对比



| SMTP                                 | HTTP                                 |
| ------------------------------------ | ------------------------------------ |
| 邮件服务器向另一个邮件服务器传送文件 | web服务器向web客户传送文件           |
| 持续连接                             | 持续连接                             |
| 推协议（Push protocol）              | 拉协议(pull protocol)                |
| 每个报文使用7比特ASCII码格式         | 不受限制                             |
| 既包含文本又包含图形，放在报文中     | 把每个对象封装到自己的HTTP响应报文中 |

##### 2.4.3  邮件报文格式和MIME

![image-20211015170801842](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015170801842.png)

##### 2.4.4 邮件访问协议

邮件访问：客户-服务器体系结构

解决像bob这样的接收方，是如何通过运行其本地PC上得用户代理，获得位于他的某个IS的邮件服务器上得邮件呢？通过引入一些特殊的邮件访问协议。比如第三版的邮局协议，因特网邮件访问协议以及HTTP

![image-20211015172259955](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211015172259955.png)

##### POP3

简短 可读性强，当用户代理打开了一个邮件服务器端口110上得TCP连接后，POP3就开始工作，建立了TCP连接，按照特许，事物处理以及更新

详细情况书P86页 

#### 2.5 DNS：因特网的目录服务

识别主机的方式：

1.主机的一种标识方法是用它的主机名。如www.yahoo.com

2.主机也可以使用IP地址进行标识

 ##### 2.5.1DNS提供的服务

路由器喜欢定长的，人喜欢好记的，我们需要一个主机名到IP地址转换的目录服务，这是域名系统的主要任务。

DNS（UDP之上，53号端口）

1. 由分层的DNS服务器实现的分布式数据库；

2. 一个使得主机能够查询分布式数据库的应用层协议

![image-20211019160800538](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211019160800538.png)

通常想获得的IP地址缓存在一个附近的DNS服务器中，为了有助于减少DNS网络流量和DNS的平均时延

DNS提供的服务有

1. 主机名到IP地址的转换
2. 主机别名
3. 邮件服务器别名 ：MX记录
4. 负载分配

##### 2.5.2 DNS工作机理概述

1. 简单设计，只需要在因特网上使用一个DNS服务器，该服务器包含映射

   1. 单点故障
   2. 通信容量
   3. 远距离的集中式数据库
   4. 维护

2. 分布式设计方案

   ![image-20211019162704387](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211019162704387.png)

   3种类型的DNS服务器

   根DNS服务器：13个根DNS服务器

   顶级域DNS服务器： 负责顶级域名com.org.net

   权威DNS服务器：在因特网上具有公共可访问主机的每个组织机构必须提供机构必须提公共可访问的DNS记录，这些记录将这些主机的名字映射到IP地址

   本地DNS服务器：一个本地DNS服务器并不属于服务器的层次结构，每个ISP都有一台本地DNS服务器

   

![image-20211019164715078](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211019164715078.png)

![image-20211019164729427](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211019164729427.png)

2.DNS缓存

##### 2.5.3 DNS工作机理概述

共同实现DNS分布式数据库的所有DNS服务器存储了资源记录（RR）

RR提供了主机名到IP地址的映射

每个DNS回答都包好了一条或多条资源记录

![image-20211019165934081](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211019165934081.png)

  TTL：该记录的生存时间，他记录了资源记录应当从缓存中删除的时间。

Name 和Value的值取决于Type

![image-20211019170218703](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211019170218703.png)



1.DNS报文（查询报文和回答报文）



![image-20211019170628294](https://minyeon.oss-cn-beijing.aliyuncs.com/tenglingImg/image-20211019170628294.png)

nslookup程序可以让正在工作的主机直接向默写DNS服务器发送一个DNS查询报文

2.在DNS数据库中插入记录

注册登记机构

提供基本和辅助权威DNS服务器的名字和IP地址

 一条A 和一条NS

MX

