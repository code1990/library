##（一）Hello World
pytest测试框架可以让我们很方便的编写测试用例

安装

cmd 打开命令提示窗口 输入下面的命令

	pip install -U pytest

检查版本信息

	pytest --version

HelloWorld

创建名为test\_quick\_start.py的文件，敲如下内容

	def reverse(string):
	    return string[::-1]
	
	def test_reverse():
	    string = "good"
	    assert reverse(string) == "doog"
	
	    another_string = "itest"
	    assert reverse(another_string) == "tseti"

在命令行中使用下面的命令去运行用例

	pytest


##（二）运行多个测试文件
所有的test_*.py或*_test.py的文件，把其当作测试文件

新建test_calc.py文件，与上一节的test_quick_start.py放在同一文件夹下

	def add(x, y):
	    return x + y
	
	def test_add():
	    assert add(1, 0) == 1
	    assert add(1, 1) == 2
	    assert add(1, 99) == 100

在命令行中使用下面的命令去运行用例

	pytest


##（三）Assert

Assert就是断言，每个测试用例都需要断言。

与unittest不同，pytest使用的是python自带的assert关键字来进行断言

assert 表达式 只要表达式的最终结果为True，那么断言通过，用例执行成功，否则用例执行失败

**详尽的用例失败描述**

	# content of test_assert1.py
	def f():
	    return 3
	
	def test_function():
    assert f() == 4

**断言异常抛出**

	import pytest
	
	def test_zero_division():
	    with pytest.raises(ZeroDivisionError):
	        1 / 0

##（四）Fixture
把Fixture理解为准备测试数据和初始化测试对象的阶段

users.dev.json的文件

	[
	  {"name":"jack","password":"Iloverose"},
	  {"name":"rose","password":"Ilovejack"},
	  {"name":"tom","password":"password123"}
	]

新建名为test\_user\_password.py的文件

	import pytest
	import json
	
	class TestUserPassword(object):
	    @pytest.fixture
	    def users(self):
	        return json.loads(open('./users.dev.json', 'r').read()) 
			# 读取当前路径下的users.dev.json文件，返回的结果是dict
	
	    def test_user_password(self, users):
	        # 遍历每条user数据
	        for user in users:
	            passwd = user['password']
	            assert len(passwd) >= 6
	            msg = "user %s has a weak password" %(user['name'])
	            assert passwd != 'password', msg
	            assert passwd != 'password123', msg

运行

	pytest test_user_password.py
	#或者显示指出 fixtures环境
	pytest --fixtures test_user_password.py

**数据清理**

新建名为test\_clear\_data.py的文件

	import smtplib
	import pytest
	
	@pytest.fixture(scope="module")
	def smtp():
	    smtp = smtplib.SMTP("smtp.gmail.com", 587, timeout=5)
	    yield smtp  # provide the fixture value
	    print("teardown smtp")
	    smtp.close()

##（五）参数化的Fixture
一次性把所有的不符合结果

新建users.test.json文件，内容如下

	[
	  {"name":"jack","password":"Iloverose"},
	  {"name":"rose","password":"Ilovejack"}
	  {"name":"tom","password":"password123"},
	  {"name":"mike","password":"password"},
	  {"name":"james","password":"AGoodPasswordWordShouldBeLongEnough"}
	]

参数化fixture的语法

	@pytest.fixture(params=["smtp.gmail.com", "mail.python.org"])

新建文件test\_user\_password\_with\_params.py
	
	import pytest
	import json
	users = json.loads(open('./users.test.json', 'r').read())
	
	class TestUserPasswordWithParam(object):
	    @pytest.fixture(params=users)
	    def user(self, request):
	        return request.param
	
	    def test_user_password(self, user):
	        passwd = user['password']
	        assert len(passwd) >= 6
	        msg = "user %s has a weak password" %(user['name'])
	        assert passwd != 'password', msg
	        assert passwd != 'password123', msg

##（六）Parametrize Fixture
@pytest.mark.parametrize 装饰器可以让我们每次参数化fixture的时候传入多个项目
	
	import pytest
	@pytest.mark.parametrize("test_input,expected", [
	    ("3+5", 8),
	    ("2+4", 6),
	    ("6*9", 42),
	])
	def test_eval(test_input, expected):
	    assert eval(test_input) == expected

##（七）常见套路
<h3 id="基础用法">基础用法</h3>

<ul>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#pass-different-values-to-a-test-function-depending-on-command-line-options">把命令行参数传入到用例</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#dynamically-adding-command-line-options">动态添加命令行参数</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#control-skipping-of-tests-according-to-command-line-option">根据命令行参数来忽略用例执行</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#writing-well-integrated-assertion-helpers">编写集成度更好的辅助断言</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#detect-if-running-from-within-a-pytest-run">判断是否由pytest执行</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#adding-info-to-test-report-header">在测试报告的头部添加内容</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#profiling-test-duration">统计用例运行时间</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#profiling-test-duration">定义测试步骤，也就是让用例按照一定的顺序执行</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#package-directory-level-fixtures-setups">Package/Directory-level fixtures (setups)</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#post-process-test-reports-failures">在报告和用例失败之前添加钩子</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#making-test-result-information-available-in-fixtures">在fixtures中访问测试结果</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#pytest-current-test-environment-variable">PYTEST_CURRENT_TEST环境变量</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/simple.html#freezing-pytest">冻结pytest</a></li>
</ul>

<h3 id="参数化">参数化</h3>

<ul>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#generating-parameters-combinations-depending-on-command-line">根据命令行参数来组合测试参数</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#generating-parameters-combinations-depending-on-command-line">配置test ID</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#a-quick-port-of-testscenarios">快速创建测试场景的功能</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#deferring-the-setup-of-parametrized-resources">延迟参数资源加载</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#apply-indirect-on-particular-arguments">间接参数</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#parametrizing-test-methods-through-per-class-configuration">为不同的方法设置不同的参数</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#indirect-parametrization-with-multiple-fixtures">在多个fixture中使用间接参数</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#indirect-parametrization-of-optional-implementations-imports">Indirect parametrization of optional implementations/imports</a></li>
<li><a href="https://docs.pytest.org/en/latest/example/parametrize.html#set-marks-or-test-id-for-individual-parametrized-test">单独的为每个参数化用例设置标记和ID</a></li>
</ul>

##（八）接口测试

新建名为v2ex_api_test.py的文件

	import requests
	
	class TestV2exApi(object):
	    domain = 'https://www.v2ex.com/'
	
	    def test_node(self):
	        path = 'api/nodes/show.json?name=python'
	        url = self.domain + path
	        res = requests.get(url).json()
	        assert res['id'] == 90
	        assert res['name'] == 'python'


##（九）使用fixture参数化接口入参

在v2ex_api_test.py的文件中加入如下内容:
	
	class TestV2exApiWithExpectation(object):
	    domain = 'https://www.v2ex.com/'
	
	    @pytest.mark.parametrize('name,node_id', 
		[('python', 90), ('java', 63), ('go', 375), ('nodejs', 436)])
	
	    def test_node(self, name, node_id):
	        path = 'api/nodes/show.json?name=%s' %(name)
	        url = self.domain + path
	        res = requests.get(url).json()
	        assert res['name'] == name
	        assert res['id'] == node_id
	        assert 0


##（十）使用fixture参数化测试预期结果
在v2ex_api_test.py的文件中加入如下内容:

	class TestV2exApiWithExpectation(object):
	    domain = 'https://www.v2ex.com/'
	
	    @pytest.mark.parametrize('name,node_id', 
	    [('python', 90), ('java', 63), ('go', 375), ('nodejs', 436)])
	
	    def test_node(self, name, node_id):
	        path = 'api/nodes/show.json?name=%s' %(name)
	        url = self.domain + path
	        res = requests.get(url).json()
	        assert res['name'] == name
	        assert res['id'] == node_id
	        assert 0

##（十一）生成xml格式的测试报告

pytest可以生成junit格式的xml报告，在命令行中加入--junit-xml=path 参数就可以了。

	pytest test_quick_start.py --junit-xml=report.xml

report.xml测试报告

	<?xml version="1.0" encoding="utf-8"?>
		<testsuite errors="0" failures="0" name="pytest" 
				skips="0" tests="1" time="0.009">
			<testcase classname="test_quick_start" 
				file="test_quick_start.py" line="3" 
				name="test_reverse" time="0.000499725341797">
			</testcase>
		</testsuite>
	</xml>