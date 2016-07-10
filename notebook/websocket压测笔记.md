>	最近工作接触到了关于推送服务的测试，故整理下此次的收获，后续会继续学习这块


###	测试前的学习阶段

Q1:	什么是websocket？

A:	出门左拐知乎 >> [websocket知识普及贴](https://www.zhihu.com/question/20215561)


Q2:	推送服务怎么测？

A: 首先你要熟悉业务，设计测试用例，制定测试策略blahblah，这些不是本篇文章的重点，我略。

我先简单说明一下业务:
	
	推送服务(push notification)
	
	“手机用过吗？你的各种【APP运行的时候】，动不动就会给你【推送】的【消息】”
	
	让我们来分析一下上面这句话的几个关键字:
	
		APP(手机) -- > 客户端 client /要考虑到不同网络情况2G,3G,4G
		
		APP运行的时候 --> 向服务端发送心跳包ping pong(保持心跳)，保持【长连接】
		
		推送 --> 推送消息接口(我们用的是http或者是rpc协议)
	
		消息 --> 一旦调用了推送接口，客户端会接收到推送消息
		
业务分析完了，来讲下最简单的测试case(真的是最简单的):
	
	1、客户端A连上服务端，定时发送心跳包，保持长连接，客户端A不断线(online)
		
	2、调用推送接口，指定向客户端A发送消息
		
	3、客户端A收到推送消息
	
对于步骤1我刚开始使用了jmeter来操作，发现很难实现ping pong操作(client持续发ack消息给服务端)，必须的是client连上服务端，服务端马上响应发一个特定的syn-ack消息过来，jmeter脚本内设计逻辑收到该消息继续使用该连接blahblah ... 各种鸡肋。也可能是我没有悟出jmeter websocket test的精髓(后面再研究下写扩展脚本的方式看看)，各位可以参照下这篇文章:出门直走头朝下点开查看 >> [WebSocket Testing With Apache JMeter](https://www.blazemeter.com/blog/websocket-testing-apache-jmeter)

随着放弃jmeter来做客户端连接操作，剩下的一个办法就是自己写客户端连接工具(开发写了，在此我就不得不感叹心有余而力不足),这里我们开发使用的是GO写的(不难的，网上也有很多demo)，后面自己也会具体研究下怎么自己写，马克留白记笔记 ^^

- - - -
- - - -
- - - -
- - - -
- - - -
- - - -

###	测试中的学习阶段

Q3:	推送服务怎么进行压测？

A:	确认性能需求，设计压测场景，执行测试，同步监控各种指标。

	___ client online
	
	___ msg send to client

	步骤：
	
		0.测试虚机（0/3）
		
		1.client连接工具(1/3)
		
		2.推送接口jmeter脚本(2/3)
		
		3.执行1，执行2，监控数据
			
	Tips: 
	
		0. client分布平均（100W连接，4台服务器，每台服务器有25W连接）
		
			*	单台服务器的性能压测就另说
		
		1. client连上服务器后，可以观察一段时间，看ping pong是否正常，监控服务器指标
		
		2. client主动断开连接，监控服务器指标
		
		3. 服务端主动断开连接，查看client是否又进行重连，重连失败连不上的等情况
		
			*	一般服务端部署有多台服务器，可随意reroll任一台
			
		4. 并发推送消息，确保消息不会丢失blahblahblah
		
		... 
		
		各位也看出来了，重点就是做好监控，监控，监控!!! 重要的事情，说三遍。
		
以上个人认为属于比较基本的业务压测，在进行压测之前还看过这样一篇以为会有用的文章:

[七种WebSocket框架的性能比较](http://colobu.com/2015/07/14/performance-comparison-of-7-websocket-frameworks/)

对客户端的机子要求较高，后面我们的压测没有能力做，用的都是虚机，不过文章内写的一些观点，还是值得参照的 ~

###	测试后的学习阶段


1.	client连接脚本需要学习怎么写

2.	shell命令需要学习

3.	jmeter beanshell学习


>	书到用时方恨少啊同志们

我举个小小的栗子：

测试机几十台机子，每台机子登上去连接5W连接，容易吗? 
	
当然，如果你会shell，分分钟的事儿。[Pexpect](https://pexpect.readthedocs.io/en/stable/)


先整理到这儿了，下周继续。

***

##	Websocket Client

> 继续上周的议题。因为我不会写go,不会写websocket客户端,导致整个测试过程我是个完美的酱油党。上周我终于把客户端服务写好了。


###	选择Websokcet框架

现在想想自己浪费了太多时间，实际上上手写代码仅需几个小时。

为了让少走点弯路，以后但凡你要学什么，找什么。。很简单。。跟着感觉走：

1.	谷歌 best xxxx xxxx xxxxx
2.	github 搜索关键字 (再筛选个语言，结果么么哒哟)

然后的然后圈好我要用的框架了 就是他他他 -->  [websocket client for python](https://github.com/liris/websocket-client)

###	开始coding

不要绕圈子了，直接：

*	自己看md
*	自己看demo
*	自己写demo

![Smaller icon](http://img1.dzwww.com:8080/tupian/20150820/39/15260244116402600403.jpg)


###	留个白




		

		
	
	
	
		
	
		
		
		
		
		
	












		
		
		
	
		
	
		
		
		
	
	
	
