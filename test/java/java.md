6/24/2018 7:30:54 AM 
##（一）Java环境安装

设置环境变量：

“我的电脑” 右键菜单—>属性—>高级—>环境变量—>系统变量—>新建..

	变量名： JAVA_HOME
	变量值： D:\Program Files\Java\jdk1.8.0_101
	变量名： CALSS_PATH
	变量值： .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;

找到 path 变量名—>“编辑” 添加：

	变量名： PATH
	变量值： %JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;

在命令提示窗口输入 java,javac,java -version

##（二）Hello world

	package Base;

	public class HelloWrold {
		//main()方法是Java应用程序的入口方法
		public static void main(String[] args) {
			//打印并换行
			System.out.println("Hello World!");
		} 
	}

##（三）Java的强类型定义

	Java要求你所定义的常量/变量和方法必须要声明类型

	public class JavaType {

		public static void main(String[] args) {
		
			//声明常量与变量
			final double PI = 3.14;  //定义常量
			int R = 5;   //定义变量
			System.out.printf("圆周率的面积:" + PI * R * R);
			
			//Java中常见的数据类型
			short s = 3;      // 定义short类型变量
			int i = 22;       // 定义int类型变量
			float f = 1.23f;  // 定义float类型变量
			char a = 'a';     // 定义字符类型用char
			String string = "hello world";  // 定义字符串类型String
			boolean status = true;   // 定义布尔类型类型用 boolean
			
			//声明字符串数组 String[]
			String[] fruits = new String[3];
			fruits[0] = "apple";
			fruits[1] = "banana";
			fruits[2] = "grape";

			//声明字符串字典 HashMap<String, String>
			HashMap<String, String> hm = new HashMap<String, String>();
			//添加字典
			hm.put("Jim","1155689");
			hm.put("Jane","1255669");
			hm.put("Kevin","1165669");
		}
	}

------------
	
	public class JavaType {
		//方法上指定了参数的类型
		public static String person(String name, int age){
			return "name: "+name+", age:" + age;
		}

		public static void main(String[] args) {
			String name = "alen";
			int age = 22;
			String p = person(name, age);
			System.out.println(p);
		}
	}

##（四）Java的三种基本结构

结构化编程的三种基本结构1顺序结构2分支结构3循环结构



	public class JavaStructure {

		public static void main(String[] args) {

			// 计算10人的阶乘
			int number = 10;

			// 0和1的阶乘都为1
			if (number == 0 || number == 1) {
				System.out.print(1);
			}else {
				//循环累乘 10
				int count = 1;
				for (int i = number; i > 0; i--) {
					count *= i;
				}
				System.out.print(count);
			}

		}
	}

-------------	


	public class JavaStructure {

		public static void main(String[] args) {

			//打印金子塔
			for (int i = 1; i <= 5; i++) {
				for (int j = 5 - i; j > 0; j--) {
					System.out.print(" ");
				}
				for (int k = 1; k <= 2 * i - 1; k++) {
					System.out.print("*");
				}
				System.out.print("\n");
			}
		}
	}

------------------	
	

	public class JavaStructure {

		public static void main(String[] args) {

			// 打印九九乘法表
			for (int i = 1; i <= 9; i++) {
				for (int j = 1; j <= i; j++) {
					System.out.print(j + "*" + i + "=" + (i * j) + " ");
				}
				System.out.println();
			}

		}
	}


##（五）Java数据结构：数组和HashMap


数组 保存的是一组有顺序的、具有相同类型的数据，可以通过数组的下标来访问数组。

	public class JavaList {
	
	    public static void main(String[] args) {
	
	        String[] group = new String[5];
	        group[0] = "小明";
	        group[1] = "老师";
	        group[2] = "老王";
	        group[3] = "张三";
	        group[4] = "李四";
	
			//创建数组的方式
			String[] fruits = {"bananas", "apples", "pears", "oranges"};
	        for (int i = 0; i < group.length; i++) {
	            System.out.println(group[i]);
	        }
	    }
	}

------

	//冒泡排序
	public class BubbleSort {

		public static void main(String[] args) {

			//排序数组
			int[] intArray = {12, 11, 45, 6, 4, 8, 43, 40, 57, 3, 22};

			System.out.println("排序前的数组:");
			for (int i = 0; i < intArray.length; i++) {
				System.out.print(intArray[i] + " ");
			}
			System.out.println();

			// 冒泡排序
			int temp;
			for (int i = 0; i < intArray.length; i++) {

				for (int j = i + 1; j < intArray.length; j++) {

					//当后一个数大于前一个，交换位置
					if (intArray[i] < intArray[j]) {
						temp = intArray[i];
						intArray[i] = intArray[j];
						intArray[j] = temp;
					}
				}
			}

			System.out.println("排序后的数组:");
			for (int i = 0; i < intArray.length; i++) {
				System.out.print(intArray[i] + " ");
			}
			System.out.println();
		}
	}

	
HashMap 是一个散列表，它存储的内容是键值对(key-value)映射。 其实，我们一般叫 “字典”。

	import java.util.HashMap;
	import java.util.Iterator;

	public class JavaHashMap {

		public static void main(String[] args) {

			//定义HashMap
			HashMap<String, String> hm = new HashMap<String, String>();
			//添加字典
			hm.put("username", "password");
			hm.put("Jim", "1155689");
			hm.put("Jane", "1255669");
			hm.put("Kevin", "1165669");

			//测试 key 是否包含 username,返回值为 true/false
			System.out.println(hm.containsKey("username"));
			System.out.println("===================>");

			//获取 key 所对应的 vlaue
			System.out.println(hm.get("Jim"));
			System.out.println("===================>");

			//获取整个字典数据
			System.out.println(hm.entrySet());
			System.out.println("===================>");

			//循环打印每一对 key:value
			Iterator<?> it = hm.entrySet().iterator();
			while (it.hasNext()) {
				System.out.println(it.next());
			}
			System.out.println("===================>");

			//分别获取 key 的值， 和 value 的值。
			Iterator<String> it2 = hm.keySet().iterator();
			while (it2.hasNext()) {
				//获得字典的 key(username)
				String username = (String) it2.next();
				System.out.println(username);
				//获得字典的 value(节点)
				String password = hm.get(username);
				System.out.println(password);
			}
		}
	}

##（六）Package 和 Import

Java类大体上分三类：1由我们自己创建的类。2由Java JDK提供的类。3由第三方库或框架提供。

package 名称就像是我们的姓，而 class 名称就像是我们的名字。

比如说java.lang.String，就是复姓 java.lang，名字为 String 的类别。

	public class JavaPackage {

		public static void main(String[] args) {
			org.openqa.selenium.WebDriver driver = 
			new org.openqa.selenium.chrome.ChromeDriver();
			// ……
		}
	}
	
import 就是在程式一开头的时候，先说明程式中会用到那些类别的简称。

	import org.openqa.selenium.WebDriver;
	import org.openqa.selenium.chrome.ChromeDriver;


	public class JavaImport {

		public static void main(String[] args) {
			WebDriver driver = new ChromeDriver();

			//……
		}

	}

##（七）类与对象

万物皆对象

	public class Programmer {

		/**
		/声明各类变量来描述程序类的属性
		 */
		 //private: 只有本类可见。
		//protected：本类、子类、同一包的类可见。
		//default(无修饰符)：本类、同一包的类可见。
		//public：对任何类可见。
		protected String name;
		protected String sex;
		protected int age;
		protected String address;

		public void eat(){
			System.out.println("我在吃饭！");
		}

		public void sleep(){
			System.out.println("我在睡觉！");
		}

		public void code(){
			System.out.printf("我在写代码！");
		}

	}

-------

	public class FindObject {

		public static void main(String[] args) {
			//使用new关键字创建对象
			Programmer programmer = new Programmer();

			// 定义对象属性
			programmer.name = "小明";
			programmer.sex = "男";
			programmer.age = 27;
			programmer.address = "北京";

			//调用对象行为
			programmer.eat();
			programmer.sleep();
			programmer.code();
		}

	}

##（八）构造方法

构造方法

构造方法用于初始化一个新建的对象，必须与类同名


	public class Father {

		public Father() {
			System.out.println("调用了无参的构造函数.");
		}

		public Father(String msg) {
			System.out.println("调用了有参的构造函数： " + msg);
		}

		public static void main(String args[]) {
			Father f1 = new Father();
			Father f2 = new Father("Hello");
		}
	}

----------------

super() 调用父类的构造方法


	public class Son extends Father {

		public Son(String msg) {
			super(msg);  // 调用父类的构造方法
		}

		public static void main(String args[]) {
			Son f = new Son("Hello");
		}
	}

this的用法 表示当前对象

this（参数）：在自身构造方法内部调用其它构造方法。

this.成员变量：调用本类中的成员变量。


	public class Son2  extends Father {

		//定义成员变量
		private String name; // 实例化对象时，默认值是null

		/**
		 * 无参数的构造方法
		 */
		Son2() {
			System.out.println("我是二儿子！");
		}

		/**
		 * 有参数的构造方法
		 */
		Son2(String name) {
			this();  //调用无参数的构造方法
			this.name = name;  // 调用成员变量
		}

		public void say() {
			System.out.println("我叫：" + name);
		}

		public static void main(String[] args) {
			Son2 s = new Son2("王小二");
			s.say();
		}
	}
