6/23/2018 3:52:15 PM 

## （一）Maven 介绍 

Apache Maven 是一个软件项目管理的综合工具

官方网站：http://maven.apache.org/

## （二）Maven 安装

JDK ： Java 开发程序所使用的环境。

IntelliJ IDEA ： Java 开发程序所使用的 IDE。

Maven ： 下载地址 http://maven.apache.org/download.cgi

设置maven环境变量：

“此电脑” 右键菜单—>属性—>高级—>环境变量—>系统变量—>新建..

    变量名： MAVEN_HOME

    变量值： D:\java\apache-maven-3.5.0

找到 path 变量名—>“编辑” 添加：

    变量名： PATH

    变量值： %MAVEN_HOME%\bin;

验证 Maven 环境是否配置成功

    mvn -v

##（三）Maven 设置仓库地址

修改本地仓库地址

打开 Maven 目录（\apache-maven-3.5.0\conf\settings.xml) 添加如下位置修改配置
    
    <localRepository>D:/Java/maven/repo</localRepository>

设置中央仓库地址

打开 Maven 目录（\apache-maven-3.5.0\conf\settings.xml) 添加如下位置修改配置

    同样打开 Maven 目录下的配置文件（\apache-maven-3.5.0\conf\settings.xml) 
    
    <mirrors>
      <mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <!--<mirrorOf>central</mirrorOf> -->
        <mirrorOf>*</mirrorOf>
      </mirror>
    </mirrors>

##（四）通过 mvn 命令创建 Maven 项目

通过 mvn 命令创建一个 Maven 项目。

    mvn -B archetype:generate \
    -DarchetypeGroupId=org.apache.maven.archetypes \
    -DgroupId=com.mycompany.app \
    -DartifactId=my-app

*  -B ：该参数表示让Maven使用批处理模式构建项目。

* archetype:generate : 创建 Maven 项目的命令。

* DarchetypeGroupId ： 指定 Apache maven 组织名。

* DgroupId ： 指定项目或公司组名。

* DartifactId ：指定项目名。

## （五） IntelliJ-IDEA 创建 Maven 项目

修改 Maven 配置

菜单栏： “File” —>“Settings…” ， 搜索“Maven” 选项

![](https://i.imgur.com/2U8wCEU.png)

创建 Maven 项目

菜单栏： “File” —>“New” —>“Project…” ， 打开创建 Maven 

![](https://i.imgur.com/NfIRTcL.png)

## （六） pom.xml 配置文件

以 （四）通过 mvn 命令创建 Maven 项目 创建的项目为例。打开项目根目录下的 pom.xml 文件。

	<project xmlns="http://maven.apache.org/POM/4.0.0" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
	http://maven.apache.org/maven-v4_0_0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	
	  <groupId>com.myapp.pro</groupId>
	  <artifactId>myapp</artifactId>
	  <packaging>jar</packaging>
	  <version>1.0-SNAPSHOT</version>
	
	  <name>myapp</name>
	  <url>http://maven.apache.org</url>
	
	 <!--denpencies指定整个项目的jar包依赖关系-->
	  <dependencies>
		<!--dependency单独指定和声明某一个jar包信息-->
	    <dependency>
	      <groupId>junit</groupId>
	      <artifactId>junit</artifactId>
	      <version>3.8.1</version>
	      <scope>test</scope>
	    </dependency>
	
	  </dependencies>
	
	</project>

* project：pom.xml文件中的顶层元素； 

* modelVersion：指明POM使用的对象模型的版本。这个值很少改动。

* groupId：指明创建项目的组织或者小组的唯一标识。 

* artifactId：指明此项目产生的主要产品的基本名称。

* version：项目产品的版本号。SNAPSHOT这个版本，表明项目处于开发阶段。 

* name：项目的显示名称，通常用于maven产生的文档中。 

* url：指定项目站点，通常用于maven产生的文档中。 

* description：描述此项目，通常用于maven产生的文档中。

**管理第三方库**

Maven仓库：http://mavenrepository.com/

![](https://i.imgur.com/ZRg80Dm.png)

idea针对maven项目报错的管理如下

![](https://i.imgur.com/TPEWKyG.png)
