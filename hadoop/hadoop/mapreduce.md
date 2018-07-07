# MapReduce
* 掌握mapreduce分布式运算框架的编程思想
* 掌握mapreduce常用算法的编程套路
* 掌握mapreduce分布式运算框架的运行机制，自定义开发

##（一）为什么要MAPREDUCE
Mapreduce把业务代码和自带默认组件整合成一个完整的分布式运算程序，并发运行在一个hadoop集群上

* 海量数据受硬件资源限制
* 单机程序扩展到集群难度大
* 集中于业务开发，而复杂性交由框架来处理

##（二）MAPREDUCE框架结构及核心运行机制
一个完整的mapreduce程序在分布式运行时有三类实例进程：

1. MRAppMaster：负责整个程序的过程调度及状态协调
1. mapTask：负责map阶段的整个数据处理流程
1. ReduceTask：负责reduce阶段的整个数据处理流程

**MapReduce工作流程图**
![](https://i.imgur.com/pelPLl2.png)
**MapReduce主要步骤叙述**

Map阶段：

Step 1：读取输入文件的内容，并解析成键值对（<key, value>）的形式，输入文件中的每一行被解析成一个<key, value>对，每个<key, value>对调用一次map()函数。

Step 2：用户写map()函数，对输入的<key,value>对进行处理，并输出新的<key,value>对。

Step 3：对Step 2中得到的<key,value>进行分区操作。

Step 4：不同分区的数据，按照key值进行排序和分组，具有相同key值的value则放到同一个集合中。

Step 5（可选）：分组后的数据进行规约。

Reduce阶段：

Step 1：对于多个map任务的输出，按照不同的分区，通过网络传输到不同的Reduce节点。

Step 2：对多个map任务的输出结果进行合并、排序，用户书写reduce函数，对输入的key、value进行处理，得到新的key、value输出结果。

Step 3：将reduce的输出结果保存在文件中。

**流程解析**

1、一个mr程序启动的时候，最先启动的是MRAppMaster，MRAppMaster启动后根据本次job的描述信息，计算出需要的maptask实例数量，然后向集群申请机器启动相应数量的maptask进程

2、maptask进程启动之后，根据给定的数据切片范围进行数据处理，主体流程为：
a)利用客户指定的inputformat来获取RecordReader读取数据，形成输入KV对
b)将输入KV对传递给客户定义的map()方法，做逻辑运算，并将map()方法输出的KV对收集到缓存
c)将缓存中的KV对按照K分区排序后不断溢写到磁盘文件

3、MRAppMaster监控到所有maptask进程任务完成之后，会根据客户指定的参数启动相应数量的reducetask进程，并告知reducetask进程要处理的数据范围（数据分区）

4、Reducetask进程启动之后，根据MRAppMaster告知的待处理数据所在位置，从若干台maptask运行所在机器上获取到若干个maptask输出结果文件，并在本地进行重新归并排序，然后按照相同key的KV为一个组，调用客户定义的reduce()方法进行逻辑运算，并收集运算输出的结果KV，然后调用客户指定的outputformat将结果数据输出到外部存储
##（三）MapTask并行度决定机制
一个job的map阶段并行度由客户端在提交job时决定
而客户端对map阶段并行度的规划的基本逻辑为：
将待处理数据执行逻辑切片（即按照一个特定切片大小，将待处理数据划分成逻辑上的多个split），然后每一个split分配一个mapTask并行实例处理

这段逻辑及形成的切片规划描述文件，由FileInputFormat实现类的getSplits()方法完成，其过程如下图：










1.3.2 FileInputFormat切片机制
1、切片定义在InputFormat类中的getSplit()方法
2、FileInputFormat中默认的切片机制：
a)简单地按照文件的内容长度进行切片
b)切片大小，默认等于block大小
c)切片时不考虑数据集整体，而是逐个针对每一个文件单独切片
比如待处理数据有两个文件：
file1.txt    320M
file2.txt    10M

经过FileInputFormat的切片机制运算后，形成的切片信息如下：  
file1.txt.split1--  0~128
file1.txt.split2--  128~256
file1.txt.split3--  256~320
file2.txt.split1--  0~10M

3、FileInputFormat中切片的大小的参数配置
通过分析源码，在FileInputFormat中，计算切片大小的逻辑：Math.max(minSize, Math.min(maxSize, blockSize));  切片主要由这几个值来运算决定
minsize：默认值：1  
  	配置参数： mapreduce.input.fileinputformat.split.minsize    
maxsize：默认值：Long.MAXValue  
    配置参数：mapreduce.input.fileinputformat.split.maxsize
blocksize
因此，默认情况下，切片大小=blocksize
maxsize（切片最大值）：
参数如果调得比blocksize小，则会让切片变小，而且就等于配置的这个参数的值
minsize （切片最小值）：
参数调的比blockSize大，则可以让切片变得比blocksize还大


选择并发数的影响因素：
1、运算节点的硬件配置
2、运算任务的类型：CPU密集型还是IO密集型
3、运算任务的数据量

1.4 map并行度的经验之谈
如果硬件配置为2*12core + 64G，恰当的map并行度是大约每个节点20-100个map，最好每个map的执行时间至少一分钟。
如果job的每个map或者 reduce task的运行时间都只有30-40秒钟，那么就减少该job的map或者reduce数，每一个task(map|reduce)的setup和加入到调度器中进行调度，这个中间的过程可能都要花费几秒钟，所以如果每个task都非常快就跑完了，就会在task的开始和结束的时候浪费太多的时间。
配置task的JVM重用[JVM重用技术不是指同一Job的两个或两个以上的task可以同时运行于同一JVM上，而是排队按顺序执行。]可以改善该问题：
（mapred.job.reuse.jvm.num.tasks，默认是1，表示一个JVM上最多可以顺序执行的task
数目（属于同一个Job）是1。也就是说一个task启一个JVM）

如果input的文件非常的大，比如1TB，可以考虑将hdfs上的每个block size设大，比如设成256MB或者512MB



1.5 ReduceTask并行度的决定
reducetask的并行度同样影响整个job的执行并发度和执行效率，但与maptask的并发数由切片数决定不同，Reducetask数量的决定是可以直接手动设置：

//默认值是1，手动设置为4
job.setNumReduceTasks(4);

如果数据分布不均匀，就有可能在reduce阶段产生数据倾斜
注意： reducetask数量并不是任意设置，还要考虑业务逻辑需求，有些情况下，需要计算全局汇总结果，就只能有1个reducetask

尽量不要运行太多的reduce task。对大多数job来说，最好rduce的个数最多和集群中的reduce持平，或者比集群的 reduce slots小。这个对于小集群而言，尤其重要。
##（四）MAPREDUCE程序运行演示
Hadoop的发布包中内置了一个hadoop-mapreduce-example-2.4.1.jar，这个jar包中有各种MR示例程序，可以通过以下步骤运行：
启动hdfs，yarn
然后在集群中的任意一台服务器上启动执行程序（比如运行wordcount）：
hadoop jar hadoop-mapreduce-example-2.4.1.jar wordcount  /wordcount/data /wordcount/out
##（五）MAPREDUCE 示例编写及编程规范
（1）用户编写的程序分成三个部分：Mapper，Reducer，Driver(提交运行mr程序的客户端)
（2）Mapper的输入数据是KV对的形式（KV的类型可自定义）
（3）Mapper的输出数据是KV对的形式（KV的类型可自定义）
（4）Mapper中的业务逻辑写在map()方法中
（5）map()方法（maptask进程）对每一个<K,V>调用一次
（6）Reducer的输入数据类型对应Mapper的输出数据类型，也是KV
（7）Reducer的业务逻辑写在reduce()方法中
（8）Reducetask进程对每一组相同k的<k,v>组调用一次reduce()方法
（9）用户自定义的Mapper和Reducer都要继承各自的父类
（10）整个程序需要一个Drvier来进行提交，提交的是一个描述了各种必要信息的job对象
##（六）MAPREDUCE程序运行模式
2.2.1 本地运行模式
（1）mapreduce程序是被提交给LocalJobRunner在本地以单进程的形式运行
（2）而处理的数据及输出结果可以在本地文件系统，也可以在hdfs上
（3）怎样实现本地运行？写一个程序，不要带集群的配置文件（本质是你的mr程序的conf中是否有mapreduce.framework.name=local以及yarn.resourcemanager.hostname参数）
（4）本地模式非常便于进行业务逻辑的debug，只要在eclipse中打断点即可

如果在windows下想运行本地模式来测试程序逻辑，需要在windows中配置环境变量：
％HADOOP_HOME％  =  d:/hadoop-2.6.1
%PATH% =  ％HADOOP_HOME％\bin
并且要将d:/hadoop-2.6.1的lib和bin目录替换成windows平台编译的版本


2.2.2 集群运行模式
（1）将mapreduce程序提交给yarn集群resourcemanager，分发到很多的节点上并发执行
（2）处理的数据和输出结果应该位于hdfs文件系统
（3）提交集群的实现步骤：
A、将程序打成JAR包，然后在集群的任意一个节点上用hadoop命令启动
     $ hadoop jar wordcount.jar cn.itcast.bigdata.mrsimple.WordCountDriver inputpath outputpath
B、直接在linux的eclipse中运行main方法
（项目中要带参数：mapreduce.framework.name=yarn以及yarn的两个基本配置）
C、如果要在windows的eclipse中提交job给集群，则要修改YarnRunner类

mapreduce程序在集群中运行时的大体流程：

附：在windows平台上访问hadoop时改变自身身份标识的方法之二：



3. MAPREDUCE中的Combiner[Combiner的使用要非常谨慎
因为combiner在mapreduce过程中可能调用也肯能不调用，可能调一次也可能调多次
所以：combiner使用的原则是：有或没有都不能影响业务逻辑]
（1）combiner是MR程序中Mapper和Reducer之外的一种组件
（2）combiner组件的父类就是Reducer
（3）combiner和reducer的区别在于运行的位置：
Combiner是在每一个maptask所在的节点运行
Reducer是接收全局所有Mapper的输出结果；
(4) combiner的意义就是对每一个maptask的输出进行局部汇总，以减小网络传输量
具体实现步骤：
1、自定义一个combiner继承Reducer，重写reduce方法
2、在job中设置：  job.setCombinerClass(CustomCombiner.class)
(5) combiner能够应用的前提是不能影响最终的业务逻辑
而且，combiner的输出kv应该跟reducer的输入kv类型要对应起来
##（七）mapreduce的shuffle机制

##（八）

##（九）

##（十）