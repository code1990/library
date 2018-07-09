##（一）appium介绍

appium是开源的移动端自动化测试框架；

官方网站 [http://appium.io/](http://appium.io/)

appium与Selenium appium类库封装了标准Selenium客户端类库

Selenium 功能自动化测试工具

appium工作原理

![](https://i.imgur.com/sqdpqVt.png)

通过Java编写了一个appium自动化脚本并执行，

请求会首先到appium-Server通过解析，驱动模拟器执行appium脚本。

**安装**

需要搭建java或者python开发环境

##（二）安装 Android SDK
Android Studio & Android SDK下载地址：[https://developer.android.com/studio/index.html?hl=zh-cn](https://developer.android.com/studio/index.html?hl=zh-cn)

Android SDK 下载地址：[http://tools.android-studio.org/index.php/sdk](http://tools.android-studio.org/index.php/sdk)

2、设置Android环境变量

下面设置环境变量：
	
	“我的电脑” 右键菜单 —> 属性 —> 高级 —> 环境变量 —> 系统变量 —> 新建…
	变量名 	变量值
	ANDROID_HOME 	D:\android\Android\sdk
	
	找到 path 变量名—> “编辑” 添加：
	变量名 	变量值
	PATH 	;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools;

安装Android 版本

双击 SDK Manage.exe 启动SDK管理器。

![](https://i.imgur.com/RDB8lW5.png)

推荐网站 [http://www.androiddevtools.cn/](http://www.androiddevtools.cn/)

创建安卓手机模拟器

![](https://i.imgur.com/5i91ETl.png)

![](https://i.imgur.com/QbNaWmx.png)

![](https://i.imgur.com/21MBkDu.png)
##（三） 安装 appium Server
Appium版本 [https://bitbucket.org/appium/appium.app/downloads/](https://bitbucket.org/appium/appium.app/downloads/)

百度网盘的下载链接： [https://pan.baidu.com/s/1pKMwdfX](https://pan.baidu.com/s/1pKMwdfX)

，点击 appium-installer.exe 进行安装，生成 Appium图标 , 双击启动，appium server 

![](https://i.imgur.com/46KSNET.png)

打开Windows命令提示符，输入“appium-doctor”命令
![](https://i.imgur.com/GY34dnl.png)

如果提示：“appium-doctor”不是内部或外部命令，找到Appium的安装目录，添加到环境变量path


##（四）java-client安装与测试

创建Maven项目

	<dependency>
	    <groupId>io.appium</groupId>
	    <artifactId>java-client</artifactId>
	    <version>5.0.0-BETA9</version>
	    <scope>test</scope>
	</dependency>


![](https://i.imgur.com/7Ar4CYb.png)

运行第一个Appium测试

第一步，启动Android模拟器。
![](https://i.imgur.com/YYs5sGp.png)

第二步，启动 Appium Server。
![](https://i.imgur.com/7ox5TM1.png)

Appium在启动时默认占用本机的4723端口，即：127.0.0.1:4723

第三步，编写第一个Appium测试程序。在appium.tests项目下创建，AppiumDemo类。

	package com.example;
	
	import org.openqa.selenium.*;
	import org.openqa.selenium.remote.DesiredCapabilities;
	
	import io.appium.java_client.AppiumDriver;
	import io.appium.java_client.android.AndroidDriver;
	
	import java.net.MalformedURLException;
	import java.net.URL;
	
	
	public class CalculatorTest {
	
	    public static void main(String[] args) throws Exception {
	
	        DesiredCapabilities capabilities = new DesiredCapabilities();
	        capabilities.setCapability("deviceName", "Android Emulator");
	        capabilities.setCapability("automationName", "Appium");
	        capabilities.setCapability("platformName", "Android");
	        capabilities.setCapability("platformVersion", "6.0");
	        capabilities.setCapability("appPackage", "com.android.calculator2");
	        capabilities.setCapability("appActivity", ".Calculator");
	
	        AndroidDriver driver = new AndroidDriver(
		new URL("http://127.0.0.1:4723/wd/hub"), capabilities);
	
	        driver.findElement(By.name("1")).click();
	        driver.findElement(By.name("5")).click();
	        driver.findElement(By.name("9")).click();
	        driver.findElement(By.name("delete")).click();
	        driver.findElement(By.name("+")).click();
	        driver.findElement(By.name("6")).click();
	        driver.findElement(By.name("=")).click();
	        Thread.sleep(2000);
	
	        String result = driver.findElement(
		By.id("com.android.calculator2:id/formula")).getText();
	        System.out.println(result);
	
	        driver.quit();
	    }
	
	}
第四步，通过 IDEA 运行程序。将会看到 Android 模拟器如下运行界面：

![](https://i.imgur.com/p8EnIif.png)
##（五）python-client安装与测试
安装 python-client

	pip install Appium-Python-Client

python-client 的项目名称叫：Appium-Python-Client。

启动Android模拟器，启动 Appium Server

Appium在启动时默认占用本机的4723端口，即：127.0.0.1:4723

编写 appnium 测试脚本
 
	#coding=utf-8
	from appium import webdriver
	
	desired_caps = {}
	desired_caps['platformName'] = 'Android'
	desired_caps['platformVersion'] = '6.0'
	desired_caps['deviceName'] = 'Android Emulator'
	desired_caps['appPackage'] = 'com.android.calculator2'
	desired_caps['appActivity'] = '.Calculator'
	
	driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
	
	driver.find_element_by_name("1").click()
	
	driver.find_element_by_name("5").click()
	
	driver.find_element_by_name("9").click()
	
	driver.find_element_by_name("delete").click()
	
	driver.find_element_by_name("9").click()
	
	driver.find_element_by_name("5").click()
	
	driver.find_element_by_name("+").click()
	
	driver.find_element_by_name("6").click()
	
	driver.find_element_by_name("=").click()
	
	driver.quit()

运行上面的脚本，你将会看到 Android 模拟器如下运行界面(同java省略)


##（六）

##（七）

##（八）

##（九）

##（十）
