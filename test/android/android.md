##（一）在Android中测试App
![](https://i.imgur.com/YaJrtru.png)

Espresso是android应用开发自带测试库。

Robolectric是一款第三方的开源的Android单元测试框架。运行速度快

AndroidJUnitRunner本质上不算是个测试工具，基于Junit针对Anroid封装

Android Studio 更好的编写测试用例的平台
##（二）Android 测试基础
测试驱动开发

![](https://i.imgur.com/36cEoli.png)

**测试金字塔**

![](https://i.imgur.com/4lV9eJB.png)

应用程序包括三种类型的测试：小测试、中测试和大测试。


* 小测试是你可以在与生产系统隔离的情况下运行的单元测试。
* 中测试是介于小测试和大测试之间的集成测试。模拟器上调试。
* 大测试是通过完成UI工作流来运行的集成和UI测试。真机调试。

Robolectric测试几乎完全符合Android设备上运行测试的完全保真度

AndroidJUnitRunner类定义了一个基于JUnit框架的测试运行器

Espresso 同步异步任务应用内交互

UI Automator 与系统应用程序UI交互

Espresso-Intents 工具可以帮助您编写这些较小的测试

##（三）Android 单元测试

Android单元测试：

* 本地测试：仅在本地机器上运行的单元测试。
* Instrumented测试： 在Android设备或模拟器上运行的单元测试

<p>接下来将告诉你如何构建这两种类型单元测试。</p>

<p><a href="https://developer.android.com/training/testing/unit-testing/local-unit-tests.html">Building Local Unit Tests（创建本地单元测试）</a></p>

<p>学习如何构建在本地机器上运行的单元测试。</p>

<p><a href="https://developer.android.com/training/testing/unit-testing/instrumented-unit-tests.html">Building Instrumented Unit Tests（创建Instrumented单元测试）</a></p>

<p>了解如何构建在Android设备或模拟器上运行的单元测试。</p>

##（四）Local 单元测试

**设置测试环境**

在Android Studio项目中，必须将本地单元测试的源文件存储在module-name/src/test/java/ 目录中。

在创建新项目时，该目录已经存在。

请包含Mockito库以简化本地单元测试。

在你的App程序的目录下找到build.gradle文件中，将这些库指定为依赖项：

	dependencies {
	    // Required -- JUnit 4 framework
	    testCompile 'junit:junit:4.12'
	    // Optional -- Mockito framework
	    testCompile 'org.mockito:mockito-core:1.10.19'
	}

**创建本地单元测试类**

	import org.junit.Test;
	import java.util.regex.Pattern;
	import static org.junit.Assert.assertFalse;
	import static org.junit.Assert.assertTrue;
	
	public class EmailValidatorTest {
	
	    @Test
	    public void emailValidator_CorrectEmailSimple_ReturnsTrue() {
	        assertThat(EmailValidator.isValidEmail("name@email.com"), is(true));
	    }
	    ...
	}

**Mock Android依赖**

通过用mock对象代替Android依赖关系

可以将单元测试与Android系统的其余部分分离

同时验证这些依赖关系中正确的方法被调用。

要使用此框架将mock对象添加到本地单元测试中，请遵循以下编程模型：

1、在你的 build.gradle 文件中包含Mockito库依赖项，如设置上面的测试环境中所述。

2、在单元测试类定义的开始处，添加 @RunWith（MockitoJUnitRunner.class）注释。简化对象的初始化 

3、要为Android依赖项创建一个模拟对象，请在字段声明之前添加@Mock注释。

4、为了Stub依赖的行为，可以通过使用when（）和return（）方法来满足条件时，可以指定一个条件和返回值。

以下示例显示如何创建使用模拟Context对象的单元测试。

	import static org.hamcrest.MatcherAssert.assertThat;
	import static org.hamcrest.CoreMatchers.*;
	import static org.mockito.Mockito.*;
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import org.mockito.Mock;
	import org.mockito.runners.MockitoJUnitRunner;
	import android.content.SharedPreferences;
	
	@RunWith(MockitoJUnitRunner.class)
	public class UnitTestSample {
	
	    private static final String FAKE_STRING = "HELLO WORLD";
	
	    @Mock
	    Context mMockContext;
	
	    @Test
	    public void readStringFromContext_LocalizedString() {
	        // Given a mocked Context injected into the object under test...
	        when(mMockContext.getString(R.string.hello_word))
	                .thenReturn(FAKE_STRING);
	        ClassUnderTest myObjectUnderTest = new ClassUnderTest(mMockContext);
	
	        // ...when the string is returned from the object under test...
	        String result = myObjectUnderTest.getHelloWorldString();
	
	        // ...then the result should be the expected one.
	        assertThat(result, is(FAKE_STRING));
	    }
	}

运行本地单元测试

要运行您的本地单元测试，请按照下列步骤操作：

1、通过单击工具栏中的“ Sync Project”，确保您的项目与Gradle同步。

2、以下列其中一种方式运行测试：

    要运行单个测试，请打开“Project”窗口，然后右键单击以进行测试，然后单击“Run”。
    要测试类中的所有方法，请右键单击测试文件中的类或方法，然后单击“Run”。
    要在目录中运行所有测试，请右键单击该目录并选择“Run Tests”。

##（五）Instrumented 单元测试

 单元测试是在真机和模拟器上运行的测试

Instrumented单元测试还有助于减少编写和维护mock代码所需的工作量。

设置测试环境

在你的Android Studio项目中，你必须将mock测试的源文件存储在module-name/src/androidTest/java/ 中。 创建新项目时该目录已经存在，并包含示例代码。

下载Android测试支持库安装程序

测试支持库包括用于功能性UI测试（Espresso和UI Automator）的JUnit 4测试运行器（AndroidJUnitRunner）和API。
 

在你的App的顶级build.gradle文件中将这些库指定为依赖项：

	dependencies {
		androidTestCompile 'com.android.support:support-annotations:24.0.0'
		androidTestCompile 'com.android.support.test:runner:0.5'
		androidTestCompile 'com.android.support.test:rules:0.5'
		// Optional -- Hamcrest library
		androidTestCompile 'org.hamcrest:hamcrest-library:1.3'
		// Optional -- UI testing with Espresso
		androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.2'
		// Optional -- UI testing with UI Automator
		androidTestCompile 'com.android.support.test.uiautomator:uiautomator-v18:2.1.2'
	}

请按照下面步骤更新对espresso-core的依赖关系：

	androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
	    exclude group: 'com.android.support', module: 'support-annotations'
	})

要使用JUnit 4测试类，请确保将AndroidJUnitRunner指定为项目中的默认测试工具运行器，
方法是在应用程序的模块级build.gradle文件中包含以下设置：

	android {
	    defaultConfig {
	        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
	    }
	}

创建一个Instrumented的单元测试类


要创建一个Instrumented的JUnit 4测试类，在测试类定义的开头添加@RunWith（AndroidJUnit4.class）注释。 

将Android测试支持库中提供的AndroidJUnitRunner类指定为默认测试运行器。

以下示例显示如何编写一个Instrumented单元测试，以确保LogHistory类正确实现了Parcelable接口：

	import android.os.Parcel;
	import android.support.test.runner.AndroidJUnit4;
	import android.util.Pair;
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import java.util.List;
	import static org.hamcrest.Matchers.is;
	import static org.junit.Assert.assertThat;
	
	@RunWith(AndroidJUnit4.class)
	@SmallTest
	public class LogHistoryAndroidUnitTest {
	
	    public static final String TEST_STRING = "This is a string";
	    public static final long TEST_LONG = 12345678L;
	    private LogHistory mLogHistory;
	
	    @Before
	    public void createLogHistory() {
	        mLogHistory = new LogHistory();
	    }
	
	    @Test
	    public void logHistory_ParcelableWriteRead() {
	        // Set up the Parcelable object to send and receive.
	        mLogHistory.addEntry(TEST_STRING, TEST_LONG);
	
	        // Write the data.
	        Parcel parcel = Parcel.obtain();
	        mLogHistory.writeToParcel(parcel, mLogHistory.describeContents());
	
	        // After you're done with writing, you need to reset the parcel for reading.
	        parcel.setDataPosition(0);
	
	        // Read the data.
	        LogHistory createdFromParcel = LogHistory.CREATOR.createFromParcel(parcel);
	        List<Pair<String, Long>> createdFromParcelData = createdFromParcel.getData();
	
	        // Verify that the received data is correct.
	        assertThat(createdFromParcelData.size(), is(1));
	        assertThat(createdFromParcelData.get(0).first, is(TEST_STRING));
	        assertThat(createdFromParcelData.get(0).second, is(TEST_LONG));
	    }
	}

创建一个测试套件

测试套件包名通常以.suite后缀结尾（例如，com.example.android.testing.mysample.suite）。

以下示例显示了如何实现名为UnitTestSuite的测试套件

	import com.example.android.testing.mysample.CalculatorAddParameterizedTest;
	import com.example.android.testing.mysample.CalculatorInstrumentationTest;
	import org.junit.runner.RunWith;
	import org.junit.runners.Suite;
	
	// Runs all unit tests.
	@RunWith(Suite.class)
	@Suite.SuiteClasses({CalculatorInstrumentationTest.class,
	        CalculatorAddParameterizedTest.class})
	public class UnitTestSuite {}

运行Instrumented单元测试

要运行Instrumented测试，请遵循以下步骤:

1、通过单击工具栏中的“Sync Project”，确保您的项目与Gradle同步。。

2、以下列其中一种方式运行测试：

    要运行单个测试请打开Project窗口，然后单击“Run”。
    要测试类中的所有方法，请右键单击测试文件中的类或方法，然后单击“Run”。
    要在目录中运行所有测试，请右键单击该目录并选择“Run Tests”。

##（六）Android UI自动化测试
用户界面（UI）测试可确保你的应用程序满足其功能要求，并达到用户最可能成功采用的高质量标准

强烈建议您使用Android Studio来构建测试应用程序

Android Studio自动执行UI测试，请在单独的Android测试文件夹src/androidTest/java中实现测试代码

为了测试Android应用程序，通常会创建这些类型的UI自动化测试：

单个应用程序的UI测试： 
跨越多个应用程序的UI测试：

<p><a href="https://developer.android.com/training/testing/ui-testing/espresso-testing.html">Testing UI for a Single App</a>（单个应用程序的UI测试）</p>

<ul>
<li>了解如何使用Espresso测试框架在单个应用程序中测试UI。</li>
</ul>

<p><a href="https://developer.android.com/training/testing/ui-testing/uiautomator-testing.html">Testing UI for Multiple Apps</a> （多个应用程序的UI测试）</p>

<ul>
<li>了解如何使用UI Automator测试框架在多个应用程序中测试UI。</li>
</ul>
##（七）Espresso 自动化测试

Espresso 测试框架，用于编写UI测试的API来模拟单个应用程序内的用户交互。

Espresso测试框架基于instrumentation的API，并通过AndroidJUnitRunner运行测试。
设置Espresso

	在Android应用程序模块的build.gradle文件中，必须设置对Espresso库的依赖关系引用：

	dependencies {
		// Other dependencies ...
		androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.2'
	}

通过打开“开发人员”选项关闭“设置”中的动画，并关闭以下所有选项：

    窗口动画比例

    过渡动画比例

    动画师持续时间比例


创建一个Espresso 测试

下面的代码片段显示了你的测试类可能如何调用这个基本的工作流程：


onView(withId(R.id.my_view))            // withId(R.id.my_view) is a ViewMatcher
        .perform(click())               // click() is a ViewAction
        .check(matches(isDisplayed())); // matches(isDisplayed()) is a ViewAssertion

使用Espresso和ActivityTestRule

以下部分介绍如何使用JUnit 4样式创建新的Espresso测试，并使用ActivityTestRule来减少需要编写的样板代码的数量。 

	package com.example.android.testing.espresso.BasicSample;

	import org.junit.Before;
	import org.junit.Rule;
	import org.junit.Test;
	import org.junit.runner.RunWith;

	import android.support.test.rule.ActivityTestRule;
	import android.support.test.runner.AndroidJUnit4;
	...

	@RunWith(AndroidJUnit4.class)
	@LargeTest
	public class ChangeTextBehaviorTest {

		private String mStringToBetyped;

		@Rule
		public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule<>(
				MainActivity.class);

		@Before
		public void initValidString() {
			// Specify a valid string.
			mStringToBetyped = "Espresso";
		}

		@Test
		public void changeText_sameActivity() {
			// Type text and then press the button.
			onView(withId(R.id.editTextUserInput))
					.perform(typeText(mStringToBetyped), closeSoftKeyboard());
			onView(withId(R.id.changeTextBt)).perform(click());

			// Check that the text was changed.
			onView(withId(R.id.textToBeChanged))
					.check(matches(withText(mStringToBetyped)));
		}
	}

访问UI组建

在Espresso可以与被测试的应用程序进行交互之前，必须先指定UI组件或视图。

Espresso支持使用Hamcrest 匹配器在应用程序中指定视图和适配器。

下面的代码片段展示了如何编写一个测试来访问EditText字段，输入一个文本字符串，关闭虚拟键盘，然后执行按钮单击。

public void testChangeText_sameActivity() {
    // Type text and then press the button.
    onView(withId(R.id.editTextUserInput))
            .perform(typeText(STRING_TO_BE_TYPED), closeSoftKeyboard());
    onView(withId(R.id.changeTextButton)).perform(click());

    // Check that the text was changed.
    ...
}

指定一个视图匹配器

您可以使用以下方法指定视图匹配器：

    在ViewMatchers类中调用方法。 例如，要查找显示的文本字符串来查找视图，可以调用如下所示的方法：

onView(withText("Sign-in"));

也可以调用withId（）并提供视图的资源ID（R.id），如下例所示：

onView(withId(R.id.button_signin));

Android资源ID不保证是唯一的。 如果测试尝试匹配多个视图使用的资源ID，则Espresso会引发AmbiguousViewMatcherException。

    使用Hamcrest Matchers类。可以使用AllOf（）方法来组合多个匹配器，例如containsString（）和instanceOf（）。 这种方法允许更窄地过滤匹配结果，如以下示例所示：

onView(allOf(withId(R.id.button_signin), withText("Sign-in")));

但是，不能使用关键字来筛选不匹配匹配器的视图，如以下示例所示：

onView(allOf(withId(R.id.button_signin), not(withText("Sign-out"))));

要在测试中使用这些方法，请导入org.hamcrest.Matchers包。 要了解有关Hamcrest匹配的更多信息，请参阅Hamcrest网站

要改善Espresso测试的性能，请指定查找目标视图所需的最低匹配信息。例如，如果某个视图可以通过其描述性文本唯一标识，则不需要指定它也可以从TextView实例分配。
在AdapterView中查找视图

在AdapterView小部件中，视图在运行时动态填充子视图。 如果要测试的目标视图位于AdapterView中（如ListView，GridView或Spinner），则onView（）方法可能无法正常工作，因为只有视图的子集可能会加载到当前视图层次结构中。

相反，调用onData（）方法来获取DataInteraction对象来访问目标视图元素。 Espresso将目标视图元素加载到当前视图层次结构中。 Espresso还负责滚动目标元素，并将元素放在焦点上。

    注意：onData（）方法不会检查你指定的项目是否与视图对应。Espresso只搜索当前的视图层次结构。如果找不到匹配，则该方法将引发NoMatchingViewException。

以下代码片段显示了如何使用onData（）方法和Hamcrest匹配一起搜索包含给定字符串的列表中的特定行。 在此示例中，LongListActivity类包含通过SimpleAdapter公开的字符串列表。

onData(allOf(is(instanceOf(Map.class)),
        hasEntry(equalTo(LongListActivity.ROW_TEXT), is("test input")));

执行操作

调用 ViewInteraction.perform（） 或 DataInteraction.perform（） 方法来模拟UI组件上的用户交互。 您必须传入一个或多个ViewAction对象作为参数。 Espresso根据给定的顺序依次触发每个动作，并在主线程中执行它们。

ViewActions类提供了用于指定常用操作的帮助程序方法列表。 可以使用这些方法作为方便的快捷方式，而不是创建和配置单独的ViewAction对象。 你可以指定如下操作：

    ViewActions.click（）：点击视图。

    ViewActions.typeText（）：点击一个视图并输入一个指定的字符串。

    ViewActions.scrollTo（）：滚动到视图。 目标视图必须从ScrollView继承，其android：visibility属性的值必须是可见的。 对于扩展AdapterView的视图（例如，ListView），onData（）方法负责为您滚动。

    ViewActions.pressKey（）：使用指定的键码进行按键操作。

    ViewActions.clearText（）：清除目标视图中的文本。

如果目标视图位于ScrollView的内部，则先执行ViewActions.scrollTo（）操作，然后再执行其他操作。 如果已经显示视图，则ViewActions.scrollTo（）操作将不起作用。
用Espresso Intents隔离测试你的活动

Espresso Intents可以验证应用程序发送的意图的验证和存根。 通过Espresso Intents，可以通过拦截传出的意图，对结果进行存根并将其发送回被测组件来隔离测试应用程序，活动或服务。

dependencies {
  // Other dependencies ...
  androidTestCompile 'com.android.support.test.espresso:espresso-intents:2.2.2'
}

为了测试一个intent，你需要创建一个IntentsTestRule类的实例，它与ActivityTestRule类非常相似。 IntentsTestRule类在每次测试之前初始化Espresso Intents，终止主活动，并在每次测试后释放Espresso Intents。

以下代码片段中显示的测试类为明确的意图提供了一个简单的测试。 它测试建立你的第一个应用程序教程中创建的活动和意图。

@Large
@RunWith(AndroidJUnit4.class)
public class SimpleIntentTest {

    private static final String MESSAGE = "This is a test";
    private static final String PACKAGE_NAME = "com.example.myfirstapp";

    /* Instantiate an IntentsTestRule object. */
    @Rule
    public IntentsTestRule≶MainActivity> mIntentsRule =
      new IntentsTestRule≶>(MainActivity.class);

    @Test
    public void verifyMessageSentToMessageActivity() {

        // Types a message into a EditText element.
        onView(withId(R.id.edit_message))
                .perform(typeText(MESSAGE), closeSoftKeyboard());

        // Clicks a button to send the message to another
        // activity through an explicit intent.
        onView(withId(R.id.send_message)).perform(click());

        // Verifies that the DisplayMessageActivity received an intent
        // with the correct package name and message.
        intended(allOf(
                hasComponent(hasShortClassName(".DisplayMessageActivity")),
                toPackage(PACKAGE_NAME),
                hasExtra(MainActivity.EXTRA_MESSAGE, MESSAGE)));

    }
}

有关 Espresso Intents的更多信息，请参阅Android测试支持库网站上的Espresso Intent文档。 您还可以下载IntentsBasicSample和IntentsAdvancedSample代码示例。
用Espresso Web测试WebViews

Espresso Web允许测试活动中包含的WebView组件。 它使用WebDriver API来检查和控制WebView的行为。

要开始使用Espresso Web进行测试，需要将以下行添加到应用程序的build.gradle文件中：

dependencies {
  // Other dependencies ...
  androidTestCompile 'com.android.support.test.espresso:espresso-web:2.2.2'
}

使用Espresso Web创建测试时，需要在实例化ActivityTestRule对象以测试活动时在WebView上启用JavaScript。 在测试中，可以选择显示在WebView中的HTML元素，并模拟用户交互，例如在文本框中输入文本，然后单击按钮。在完成操作后，你可以验证网页上的结 果是否符合预期结果。

在以下代码片段中，这个类测试一个WebView组件，该组件的id值“WebView”在被测试的活动中。 verifyValidInputYieldsSuccesfulSubmission（）测试选择网页上的<input>元素，输入一些文本，并检查出现在另一个元素中的文本。

@LargeTest
@RunWith(AndroidJUnit4.class)
public class WebViewActivityTest {

    private static final String MACCHIATO = "Macchiato";
    private static final String DOPPIO = "Doppio";

    @Rule
    public ActivityTestRule mActivityRule =
        new ActivityTestRule(WebViewActivity.class,
            false /* Initial touch mode */, false /*  launch activity */) {

        @Override
        protected void afterActivityLaunched() {
            // Enable JavaScript.
            onWebView().forceJavascriptEnabled();
        }
    }

    @Test
    public void typeTextInInput_clickButton_SubmitsForm() {
       // Lazily launch the Activity with a custom start Intent per test
       mActivityRule.launchActivity(withWebFormIntent());

       // Selects the WebView in your layout.
       // If you have multiple WebViews you can also use a
       // matcher to select a given WebView, onWebView(withId(R.id.web_view)).
       onWebView()
           // Find the input element by ID
           .withElement(findElement(Locator.ID, "text_input"))
           // Clear previous input
           .perform(clearElement())
           // Enter text into the input element
           .perform(DriverAtoms.webKeys(MACCHIATO))
           // Find the submit button
           .withElement(findElement(Locator.ID, "submitBtn"))
           // Simulate a click via JavaScript
           .perform(webClick())
           // Find the response element by ID
           .withElement(findElement(Locator.ID, "response"))
           // Verify that the response page contains the entered text
           .check(webMatches(getText(), containsString(MACCHIATO)));
    }
}

有关Espresso Web的更多信息，请参阅Android测试支持库网站上的Espresso Web文档。您也可以将此代码段作为Espresso Web代码示例的一部分下载。
验证结果

调用ViewInteraction.check（）或DataInteraction.check（）方法来声明UI中的视图匹配某个预期的状态。 必须传递给ViewAssertion对象作为参数。如果断言失败，Espresso将抛出一个AssertionFailedError。

ViewAssertions类提供了用于指定公共断言的帮助器方法列表。 你可以使用的断言包括：

    doesNotExist：断言当前视图层次结构中没有与指定条件匹配的视图。

    matches：断言指定的视图存在于当前的视图层次结构中，并且其状态匹配给定的Hamcrest匹配器。

    selectedDescendentsMatch：声明指定的父视图的子视图存在，并且它们的状态匹配给定的Hamcrest匹配器。

以下代码片段显示如何检查用户界面中显示的文本与先前在EditText字段中输入的文本具有相同的值。

public void testChangeText_sameActivity() {
    // Type text and then press the button.
    ...

    // Check that the text was changed.
    onView(withId(R.id.textToBeChanged))
            .check(matches(withText(STRING_TO_BE_TYPED)));
}

在设备或模拟器上运行Espresso测试 

你可以从Android Studio或从命令行运行Espresso测试。 确保将AndroidJUnitRunner指定为项目中的默认检测工具。

要运行Espresso测试，请按照前面章节介绍的步骤运行已测试的测试。
##（八）UI Automator 自动化测试
 UI Automator API可让你与设备上的可见元素进行交互，而不管哪个Activity处于焦点。

UI Automator测试框架是基于 instrumentation的API，并与AndroidJUnitRunner测试运行器一起使用。
设置UI Automator

在使用UI Automator构建UI测试之前，请确保配置测试源代码位置和项目依赖关系，如前面章节中所述。

在Android应用程序模块的build.gradle文件中，必须设置对UI Automator库的依赖关系引用：

dependencies {
    ...
    androidTestCompile 'com.android.support.test.uiautomator:uiautomator-v18:2.1.1'
}

为了优化您的UI Automator测试，您应该首先检查目标应用程序的UI组件，并确保它们可以访问。 这些优化技巧将在下面两节中介绍。
查看设备上的UI

在设计您的测试之前，检查设备上可见的UI组件。 为确保您的UI Automator测试可以访问这些组件，请检查这些组件是否具有可见的文本标签、android:contentDescription 值。

uiautomatorviewer工具提供了一个方便的可视化界面来检查布局层次结构，并查看设备前台中可见的UI组件的属性。这个信息可以让你使用UI Automator创建更细致的测试。例如，可以创建一个匹配特定可见属性的UI选择器。

启动uiautomatorviewer工具：

1.在物理设备上启动目标应用程序。

2.将设备连接到你的开发机器。

3.打开终端窗口并导航到<android-sdk> / tools /目录。

4.使用以下命令运行该工具：

$ uiautomatorviewer

要查看您的应用程序的UI属性：

1.在uiautomatorviewer界面中，点击 Device Screensho 按钮。

2.将鼠标悬停在左侧面板中的快照上，查看由uiautomatorviewer工具标识的UI组件。 属性列在右下方的面板中，右上方的布局层次中列出。

3.或者，单击 Toggle NAF Nodes 按钮以查看UI Automator无法访问的UI组件。 这些组件只有有限的信息可用。
确保你的Activity是可访问的

UI Automator测试框架对已实施Android辅助功能的应用程序执行得更好。当你使用View类型的UI元素或SDK或Support Library中的View的子类时，不需要实现可访问性支持，因为这些类已经为你完成了。

但是，有些应用程序使用自定义UI元素来提供更丰富的用户体验。 这些元素不会提供自动的可访问性支持。如果你的应用程序包含不是来自SDK或支持库的View子类的实例，请确保通过完成以下步骤将可访问性功能添加到这些元素：

1.创建一个扩展ExploreByTouchHelper的具体类。

2.通过调用setAccessibilityDelegate（）将新类的实例与特定的自定义UI元素相关联。

有关将辅助功能添加到自定义视图元素的其他指导，请参阅构建可访问自定义视图要详细了解Android上可访问性的一般最佳实践，请参阅使应用程序更易于访问。
创建一个UI Automator测试类

UI Automator测试类应该像JUnit 4测试类一样编写。 要了解有关创建JUnit 4测试类和使用JUnit 4断言和注释的更多信息，请参阅创建测试单元测试类。

在测试类定义的开始处添加@RunWith（AndroidJUnit4.class）注释。 还需要将Android测试支持库中提供的AndroidJUnitRunner类指定为您的默认测试运行器。 在设备或模拟器上运行UI Automator测试中将更详细地描述此步骤。

在UI Automator测试类中实现以下编程模型：

1.通过调用getInstance（）方法并传递一个Instrumentation对象作为参数，获取一个UiDevice对象来访问要测试的设备。

2.通过调用findObject（）方法，获取UiObject对象以访问设备上显示的UI组件（例如前景中的当前视图）。

3.通过调用UiObject方法来模拟特定的用户交互以在该UI组件上执行; 例如，调用performMultiPointerGesture（）来模拟多点触摸手势，setText（）来编辑文本字段。你可以根据需要重复调用步骤2和3中的API，以测试涉及多个UI组件或用户操作序列的更复杂的用户交互。

4.在执行这些用户交互之后，检查UI是否反映了预期的状态或行为。

以下各节将详细介绍这些步骤。
访问UI组件

UiDevice对象是访问和操作设备状态的主要方式。在测试中调用UiDevice方法来检查各种属性的状态，例如当前方向或显示大小。 可以使用UiDevice对象执行设备级别的操作，例如强制设备进入特定的旋转状态，按下D-pad硬件按钮，然后按Home（主页）和Menu（菜单）按钮。

从设备的主屏幕开始测试是一种很好的做法。从主屏幕（或在设备中选择的其他位置），你可以调用UI Automator API提供的方法来选择特定的UI元素并与之交互。

测试如何得到一个UiDevice的实例，并模拟按下一个主页按钮：

	import org.junit.Before;
	import android.support.test.runner.AndroidJUnit4;
	import android.support.test.uiautomator.UiDevice;
	import android.support.test.uiautomator.By;
	import android.support.test.uiautomator.Until;
	...

	@RunWith(AndroidJUnit4.class)
	@SdkSuppress(minSdkVersion = 18)
	public class ChangeTextBehaviorTest {

		private static final String BASIC_SAMPLE_PACKAGE
				= "com.example.android.testing.uiautomator.BasicSample";
		private static final int LAUNCH_TIMEOUT = 5000;
		private static final String STRING_TO_BE_TYPED = "UiAutomator";
		private UiDevice mDevice;

		@Before
		public void startMainActivityFromHomeScreen() {
			// Initialize UiDevice instance
			mDevice = UiDevice.getInstance(InstrumentationRegistry.getInstrumentation());

			// Start from the home screen
			mDevice.pressHome();

			// Wait for launcher
			final String launcherPackage = mDevice.getLauncherPackageName();
			assertThat(launcherPackage, notNullValue());
			mDevice.wait(Until.hasObject(By.pkg(launcherPackage).depth(0)),
					LAUNCH_TIMEOUT);

			// Launch the app
			Context context = InstrumentationRegistry.getContext();
			final Intent intent = context.getPackageManager()
					.getLaunchIntentForPackage(BASIC_SAMPLE_PACKAGE);
			// Clear out any previous instances
			intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
			context.startActivity(intent);

			// Wait for the app to appear
			mDevice.wait(Until.hasObject(By.pkg(BASIC_SAMPLE_PACKAGE).depth(0)),
					LAUNCH_TIMEOUT);
		}
	}

在示例中，@SdkSuppress（minSdkVersion = 18）语句有助于确保测试只能在Android 4.3（API级别18）或更高级别的设备上运行，正如UI Automator框架所要求的那样。

使用findObject（）方法来检索一个UiObject，它表示一个匹配给定选择器条件的视图。根据需要重复使用在应用程序测试的其他部分创建的UIObject实例。 请注意，每次测试使用UiObject实例单击UI元素或查询属性时，UI Automator测试框架都会在当前显示中搜索匹配项。

以下片段显示了测试如何构建表示应用程序中的“取消”按钮和“确定”按钮的UIObject实例。

UiObject cancelButton = mDevice.findObject(new UiSelector()
        .text("Cancel"))
        .className("android.widget.Button"));
UiObject okButton = mDevice.findObject(new UiSelector()
        .text("OK"))
        .className("android.widget.Button"));

// Simulate a user-click on the OK button, if found.
if(okButton.exists() && okButton.isEnabled()) {
    okButton.click();
}

指定选择器

如果要访问应用程序中的特定UI组件，请使用UiSelector类。 这个类表示对当前显示的UI中特定元素的查询。

如果找到多个匹配元素，则层次结构中的第一个匹配元素将作为目标UiObject返回。 构建UiSelector时，可以将多个属性链接在一起以优化搜索。 如果找不到匹配的UI元素，则抛出UiAutomatorObjectNotFoundException。

你可以使用childSelector（）方法来嵌套多个UiSelector实例。 例如，下面的代码示例显示如何测试搜索以在当前显示的UI中查找第一个ListView，然后在该ListView内搜索以查找具有文本属性Apps的UI元素。

UiObject appItem = new UiObject(new UiSelector()
        .className("android.widget.ListView")
        .instance(0)
        .childSelector(new UiSelector()
        .text("Apps")));

最佳做法是，在指定选择器时，应使用资源ID（如果将其分配给UI元素）而不是文本元素或内容描述符。并非所有元素都有文本元素（例如，工具栏中的 图标）。文本选择器很脆弱，如果UI发生细微的变化，可能会导致测试失败。他们也可能不能跨越不同的语言; 您的文本选择器可能不匹配翻译的字符串。

在选择器条件中指定对象状态可能很有用。例如，如果要选择所有选中的元素的列表，以便取消选中它们，请使用参数设置为true的checked（）方法。
执行操作

一旦你的测试获得了一个UIObject对象，可以调用UIObject类中的方法在该对象表示的UI组件上执行用户交互。指定如下操作：

    click（）：单击UI元素的可见边界的中心。

    dragTo（）：将此对象拖动到任意坐标。

    setText（）：清除字段内容后，设置可编辑字段中的文本。 
	相反，clearTextField（）方法会清除可编辑字段中的现有文本。

    swipeUp（）：对UiObject执行滑动操作。 同样，swipeDown（），
	swipeLeft（）和swipeRight（）方法也会执行相应的操作。

UI Automator测试框架允许您通过getContext（）获取Context对象来发送Intent或启动一个Activity，而无需使用shell命令。

以下片段显示了测试如何使用Intent来启动测试中的应用程序。 这种方法是有用的，当你只是在测试计算器应用程序感兴趣，并不关心发射器。

	public void setUp() {
	    ...
	
	    // Launch a simple calculator app
	    Context context = getInstrumentation().getContext();
	    Intent intent = context.getPackageManager()
	            .getLaunchIntentForPackage(CALC_PACKAGE);
	    intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
	            // Clear out any previous instances
	    context.startActivity(intent);
	    mDevice.wait(Until.hasObject(By.pkg(CALC_PACKAGE).depth(0)), TIMEOUT);
	}

对集合执行操作

如果要模拟项目集合（例如，音乐相册中的歌曲或收件箱中的电子邮件列表）上的用户交互，请使用UiCollection类。要创建UiCollection对象，请指定一个UiSelector，用于搜索UI容器或其他子UI元素的包装器，例如包含子UI元素的布局视图。

以下代码片段显示了测试如何构建UiCollection来表示在FrameLayout中显示的视频专辑：

	UiCollection videos = new UiCollection(new UiSelector()
	        .className("android.widget.FrameLayout"));
	
	// Retrieve the number of videos in this collection:
	int count = videos.getChildCount(new UiSelector()
	        .className("android.widget.LinearLayout"));
	
	// Find a specific video and simulate a user-click on it
	UiObject video = videos.getChildByText(new UiSelector()
	        .className("android.widget.LinearLayout"), "Cute Baby Laughing");
	video.click();
	
	// Simulate selecting a checkbox that is associated with the video
	UiObject checkBox = video.getChild(new UiSelector()
	        .className("android.widget.Checkbox"));
	if(!checkBox.isSelected()) checkbox.click();

在可滚动视图上执行操作

使用UiScrollable类模拟垂直或水平滚动显示。 当UI元素位于屏幕外并且需要滚动以将其显示在视图中时，此技术很有用。

以下代码片段显示了如何模拟向下滚动“Settings ”菜单并点击“About tablet option”：

	UiScrollable settingsItem = new UiScrollable(new UiSelector()
	        .className("android.widget.ListView"));
	UiObject about = settingsItem.getChildByText(new UiSelector()
	        .className("android.widget.LinearLayout"), "About tablet");
	about.click();

验证结果

InstrumentationTestCase扩展了TestCase，因此你可以使用标准的JUnit Assert方法来测试应用程序中的UI组件，以返回预期的结果。

以下片段显示了如何在计算器应用程序中找到几个按钮

	private static final String CALC_PACKAGE = "com.myexample.calc";
	
	public void testTwoPlusThreeEqualsFive() {
	    // Enter an equation: 2 + 3 = ?
	    mDevice.findObject(new UiSelector()
	            .packageName(CALC_PACKAGE).resourceId("two")).click();
	    mDevice.findObject(new UiSelector()
	            .packageName(CALC_PACKAGE).resourceId("plus")).click();
	    mDevice.findObject(new UiSelector()
	            .packageName(CALC_PACKAGE).resourceId("three")).click();
	    mDevice.findObject(new UiSelector()
	            .packageName(CALC_PACKAGE).resourceId("equals")).click();
	
	    // Verify the result = 5
	    UiObject result = mDevice.findObject(By.res(CALC_PACKAGE, "result"));
	    assertEquals("5", result.getText());
	}

在设备或模拟器上运行UI Automator测试

你可以从Android Studio或从命令行运行UI Automator测试。 确保将AndroidJUnitRunner指定为项目中的默认检测工具。
##（九）

##（十）