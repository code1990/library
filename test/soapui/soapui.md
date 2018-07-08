##（一）Web Services基本概念

SOA:面向服务架构又称“面向服务的体系结构”。SOA本质上是服务的集合.

SOA归功于Web Services标准的成熟和应用普及。

Web Services 所有的通信是通过 SOAP 进行的

SOAP 是基于 XML 的

XML 是结构化的文本消息。


HTTP 是一种网络传输协议。 

Web Services 是一种部署在Web上的对象或者是应用程序组件


##（二）Web Services技术之SOAP

Web Services 技术由 SOAP、WSDL 和 UDDI 就构成。

SOAP，简单对象访问协议。

SOAP 是基于 XML 在分散或分布式的环境中交换信息的简单的协议。

XML是可以扩展标记语言。
	
WSDL,网络服务描述语言。

WSDL是一门基于 XML 的语言，用于描述 Web Services 以及如何对它们进行访问.

UDDI，可译为 “通用描述、发现与集成服务

UDDI 可以帮助 Web 服务提供商在互联网上发布 Web Services 的信息


##（三）Web Services技术之REST


REST 对 Web 的影响非常大，已经普遍地取代了基于 SOAP 和 WSDL 的接口设计

REST 具有夸语言、夸平台的特点。所以，它是一种遵循 REST 风格的 Web Services。

如果一个架构符合 REST 原则，就称它为 REST ful架构。

RESTful 架构

* 每一个URI代表一种资源；
* 客户端和服务器之间，传递这种资源的某种表现层；
* 客户端通过四个HTTP动词GET、POST、PUT、DELETE，对服务器端资源进行操作，实现 “表现层状态转化” 。

##（四）soapUI介绍与安装

官方地址：[https://www.soapui.org/](https://www.soapui.org/)

soapUI 是一款针对 REST 、SOAP 和其它的 API 自动化测试框架。

提供SOAP WebServices功能测试、REST API功能测试、WSDL覆盖、消息断言测试、测试重构。


##（五）soapUI创建SOAP项目
菜单栏，右键 Projects –> New SOAP Project
![](https://i.imgur.com/JNzB7gq.png)
以国内手机号码归属地查询接口为例：

Project Name：MobileCodeWS为项目名称。

Initial WSDL：http://ws.webxml.com.cn/WebServices/MobileCodeWS.asmx?wsdl 为接口URL。
![](https://i.imgur.com/5CpOGM7.png)
点击 OK 按钮，创建项目完成。

依次展开：MobileCodeWS–>MobileCodeWSSoap–>getMobileCodeInfo/

双击 Request 1，填写接口查询的手机号。如下图。
![](https://i.imgur.com/7na0KKy.png)

请求接口详细配置如下：

	<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org
		/soap/envelope/" xmlns:web="http://WebXml.com.cn/">
	   <soapenv:Header/>
	   <soapenv:Body>
	      <web:getMobileCodeInfo>
	         <!--Optional:-->
	         <web:mobileCode>186xxxxxxxx</web:mobileCode>
	         <!--Optional:-->
	         <web:userID></web:userID>
	      </web:getMobileCodeInfo>
	   </soapenv:Body>
	</soapenv:Envelope>

点击 Request 1 窗口左上角的绿色 运行 按钮，发送 SOAP 请求。右侧窗口将会显示接口返回结果。
![](https://i.imgur.com/kTz9gTG.png)

##（六）soapUI创建REST项目
右键 Projects –> New REST Project
![](https://i.imgur.com/brloqQe.png)

以本地查询发布会接口为例。

URI：http://www.kuaidi100.com 为接口URI。

![](https://i.imgur.com/CRLD4cm.png)

点击 OK 按钮，创建项目完成。

如图所示 输入参数

![](https://i.imgur.com/I97Ua1K.png)

结果如下
![](https://i.imgur.com/IpQ5Uzb.png)

##（七）soapUI设置Auth

当我们请求一个接口时，一般需要认证，认证是判断用户是否有请求权限的常用手段。
![](https://i.imgur.com/QewYC5X.png)
点击 “Request 1” 窗口左下角 “Auth” 按钮，Authoriaztion 选项中选择 “add New Authoriaztion” 

在弹出的窗口中 Type 选择 “Basic” 选项，点击 “OK” 按钮，如下图。

![](https://i.imgur.com/hTNU4ZD.png)

填写用户认证 Username 和 Password ，勾选 “Authenticate pre-emptvely” 选项。

	Username：用于填写基本认证的用户名。
	
	Password：用于填写基本认证的密码。
	
	Domain：域名是基本认证的可选项，设置为空。
	
	Pre-emptive auth：设置定义认证的行为。

    Use global preference ：用于定义HTTP设置为全局首选项。
    Authenticate pre-emptively：仅适用于此请求，不需要等待身份验证质询时发送凭据。

##（八）soapUI创建性能测试

右键点击 “requests 1” 请求，选择 “Add to TestCase” …，如下图。

![](https://i.imgur.com/VFCL5Rt.png)

第一步，默认设置测试套件名为：TestSuite 1。

第二步，默认设置测试用例名为：TestCase 1。

第三步，添加 Requests 到 测试用例，如下图。
![](https://i.imgur.com/zpj5vee.png)
点击 “OK” 完整测试用例的创建，如下图。
![](https://i.imgur.com/4g6iugi.png)
接下来，创建性能测试用例，右键点击 “Load Tests (1)” –> “New LoadTest” , 创建完成，

参考上图，多出来一个 “LoadTest 1” 的选项。

![](https://i.imgur.com/Tx1o1iG.png)

	Limit：表示要持续执行时间，秒为单位，默认是60。
	
	Threads：负载测试所用的线程数，性能测试中所说的并发数。默认是5。
	
	TestDelay：设置测试时线程的休眠时间
	
	Random：该值得设置，于testDelay的设置结合在一起

