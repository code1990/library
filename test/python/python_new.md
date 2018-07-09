##（一）Python 简介  环境搭建
Python 是一个高层次的结合了解释性和面向对象的脚本语言。 

Python 环境搭建

Python官网：[https://www.python.org/](https://www.python.org/)

Python文档下载地址：[https://www.python.org/doc/](https://www.python.org/doc/)

SDK下载：[https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)

测试 
![](https://i.imgur.com/MDn4qDr.png)

##（二）基础语法

	#coding=utf-8
	print "你好，世界";

**Python 标识符**

在 Python 里，标识符由字母、数字、下划线组成。

在 Python 中，所有标识符可以包括英文、数字以及下划线(_)，但不能以数字开头。

Python 中的标识符是区分大小写的。

以下划线开头的标识符是有特殊意义的。以单下划线开头 _foo 的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用 from xxx import * 而导入；

以双下划线开头的 __foo 代表类的私有成员；以双下划线开头和结尾的 __foo__ 代表 Python 里特殊方法专用的标识，如 __init__() 代表类的构造函数。

**Python 保留字符**

下面的列表显示了在Python中的保留字。这些保留字不能用作常数或变数，或任何其他标识符名称。

所有 Python 的关键字只包含小写字母。

![](https://i.imgur.com/jyJChkS.png)

**行和缩进**

不使用大括号 {} 来控制类，函数以及其他逻辑判断

所有代码块语句必须包含相同的缩进空白数量，这个必须严格执行

	if True:
	    print "True"
	else:
	  print "False"

**多行语句**

	total = item_one + \
	        item_two + \
	        item_three

**Python 引号**
使用引号( ' )、双引号( " )、三引号( ''' 或 """ ) 来表示字符串

	word = 'word'
	sentence = "这是一个句子。"
	paragraph = """这是一个段落。
	包含了多个语句"""

**Python注释**

python中单行注释采用 # 开头。

python 中多行注释使用三个单引号(''')或三个双引号(""")。

**Python空行**

函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。

类和函数入口之间也用一行空行分隔，以突出函数入口的开始

**Print 输出**

print 默认输出是换行的，如果要实现不换行需要在变量末尾加上逗号 ,

**多个语句构成代码组**

	if expression : 
	   suite 
	elif expression :  
	   suite  
	else :  
	   suite 


##（三）变量类型

Python 中的变量赋值不需要类型声明

	counter = 100 # 赋值整型变量
	miles = 1000.0 # 浮点型
	name = "John" # 字符串
	 
	print counter
	print miles
	print name

Python有五个标准的数据类型：

* Numbers（数字）int（有符号整型） long（长整型）  float（浮点型）  complex（复数） 
* String（字符串）取值顺序:从左到右索引默认0开始的，从右到左索引默认-1开始的
* List（列表）List（列表） 是 Python 中使用最频繁的数据类型
* Tuple（元组）类似于List（列表）,元组不能二次赋值，相当于只读列表
* Dictionary（字典）列表是有序的对象集合，字典是无序的对象集合

**Python数据类型转换**

##（四）Python 条件语句
	if 判断条件：
	    执行语句……
	else：
	    执行语句……

	if 判断条件1:
	    执行语句1……
	elif 判断条件2:
	    执行语句2……
	elif 判断条件3:
	    执行语句3……
	else:
	    执行语句4……
##（五）循环语句
* while 循环	在给定的判断条件为 true 时执行循环体，否则退出循环体。
* for 循环	重复执行语句
* 嵌套循环	你可以在while循环体中嵌套for循环

* break 语句	跳出整个循环
* continue 语句	终止当前循环，执行下一次循环。
* pass 语句	pass是空语句，是为了保持程序结构的完整性。

	while 判断条件：
	    执行语句……


	for iterating_var in sequence:
	   statements(s)
   
for 中的语句和普通的没有区别，else 中的语句会在循环正常执行完  
 
	for num in range(10,20):  # 迭代 10 到 20 之间的数字
	   for i in range(2,num): # 根据因子迭代
	      if num%i == 0:      # 确定第一个因子
	         j=num/i          # 计算第二个因子
	         print '%d 等于 %d * %d' % (num,i,j)
	         break            # 跳出当前循环
	   else:                  # 循环的 else 部分
	      print num, '是一个质数'

循环嵌套

	for iterating_var in sequence:
	   for iterating_var in sequence:
		  statements(s)
	   statements(s)

	while expression:
	   while expression:
		  statement(s)
	   statement(s)
pass

	# 输出 Python 的每个字母
	for letter in 'Python':
	   if letter == 'h':
	      pass
	      print '这是 pass 块'
	   print '当前字母 :', letter
	
	print "Good bye!"

##（六）
* 整型(Int) - 通常被称为是整型或整数，是正或负整数，不带小数点。
* 长整型(long integers) - 无限大小的整数，整数最后是一个大写或小写的L。
* 浮点型(floating point real values) - 浮点型由整数部分与小数部分组成，
* 复数(complex numbers) - 复数由实数部分和虚数部分构成

Python math 模块提供了许多对浮点数的数学运算函数。

Python cmath 模块包含了一些用于复数运算的函数。

	int(x [,base ])         将x转换为一个整数  
	long(x [,base ])        将x转换为一个长整数  
	float(x )               将x转换到一个浮点数  
	str(x )                 将对象 x 转换为字符串  

##（七）Python字符串

* Python字符串更新
* Python转义字符
* Python 字符串格式化

##（八）Python 列表(List)

* 访问列表中的值
* 更新列表
* 删除列表元素
* Python列表截取
##（九）Python 元组

* 创建空元组
* 访问元组
* 修改元组
* 删除元组

##（十）Python 字典(Dictionary)

* 访问字典里的值
* 修改字典
* 删除字典元素
##（十一）Python 日期和时间


Python 提供了一个 time 和 calendar 模块可以用于格式化日期和时间。


	#!/usr/bin/python
	# -*- coding: UTF-8 -*-
	 
	import time;  # 引入time模块
	 
	ticks = time.time()
	print "当前时间戳为:", ticks



	#!/usr/bin/python
	# -*- coding: UTF-8 -*-
	 
	import time
	 
	localtime = time.localtime(time.time())
	print "本地时间为 :", localtime




获取格式化的时间


	#!/usr/bin/python
	# -*- coding: UTF-8 -*-
	 
	import time
	 
	localtime = time.asctime( time.localtime(time.time()) )
	print "本地时间为 :", localtime

实例演示：

	#!/usr/bin/python
	# -*- coding: UTF-8 -*-
	 
	import time
	 
	# 格式化成2016-03-20 11:45:39形式
	print time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()) 
	 
	# 格式化成Sat Mar 28 22:24:24 2016形式
	print time.strftime("%a %b %d %H:%M:%S %Y", time.localtime()) 
	  
	# 将格式字符串转换为时间戳
	a = "Sat Mar 28 22:24:24 2016"
	print time.mktime(time.strptime(a,"%a %b %d %H:%M:%S %Y"))


获取某月日历

Calendar模块有很广泛的方法用来处理年历和月历，例如打印某月的月历：

	#!/usr/bin/python
	# -*- coding: UTF-8 -*-
	 
	import calendar
	 
	cal = calendar.month(2016, 1)
	print "以下输出2016年1月份的日历:"
	print cal

##（十二）函数

def functionname( parameters ):
   "函数_文档字符串"
   function_suite
   return [expression]

##（十三）模块
 import 语句
模块的引入

模块定义好后，我们可以使用 import 语句来引入模块，语法如下：

import module1[, module2[,... moduleN]
##（十四）异常处理

 捕捉异常可以使用try/except语句。

try/except语句用来检测try语句块中的错误，从而让except语句捕获异常信息并处理。

如果你不想在异常发生时结束你的程序，只需在try里捕获它。

语法：

以下为简单的try....except...else的语法： 
##（十五）

##（十六）

##（十七）

##（十八）

##（十九）

##（二十）