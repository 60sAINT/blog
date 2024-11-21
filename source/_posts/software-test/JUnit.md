---
title: JUnit 单元测试
date: 2023-04-14
categories:
- [软件测试与质量保证]
tags:
  - 软件测试
  - JUnit
cover: assets/junit.png
---

# Junit 的核心类结构

+   Assert 类：验证条件是否成立，当条件成立时，assert 方法保持沉默，若条件不成立时就抛出异常
+   Test 接口：测试和收集测试的结果，它是单独的测试用例、聚合的测试模式以及测试扩展的共同接口
+   TestCase 抽象类：根据输入的测试名称来创建一个测试用例，提供测试名的目的在于方便测试失败时查找失败的测试用例
+   TestSuite 类：由几个 TestCase 或其他的 TestSuite 构成。被加入到 TestSuite 中的测试在一个线程上依次被执行
+   TestResult 类：负责收集 TestCase 所执行的结果，它将结果分类，分为客户可预测的错误和没有预测的错误，它还将测试结果转发到 TestListener 处理
+   TestRunner 类：客户对象调用的起点，负责对整个测试过程进行跟踪。能够显示测试结果，并且报告测试的进度
+   TestListener 类：包含 4 个方法：addError ()，addFailure ()，startTest () 和 endTest ()。测试中若产生事件（错误、失败、开始、结束）会通知 TestListener

Assert 类的主要方法：

1.  `void assertEquals(boolean expected, boolean actual)`
    
    检查两个变量或者等式是否平衡
    
2.  `void assertFalse(boolean condition)`
    
    检查条件是假的
    
3.  `void assertNotNull(Object object)`
    
    检查对象不是空的
    
4.  `void assertNull(Object object)`
    
    检查对象是空的
    
5.  `void assertTrue(boolean condition)`
    
    检查条件为真
    
6.  `void fail()`
    
    在没有报告的情况下使测试不通过
    

TestCase 类的主要方法：

1.  `int countTestCases()`
    
    为被 run (TestResult result) 执行的测试案例计数
    
2.  `TestResult createResult()`
    
    创建一个默认的 TestResult 对象
    
3.  `String getName()`
    
    获取 TestCase 的名称
    
4.  `TestResult run()`
    
    一个运行这个测试的方便的方法，收集由 TestResult 对象产生的结果
    
5.  `void run(TestResult result)`
    
    在 TestReault 中运行测试案例并收集结果
    
6.  `void setName(String name)`
    
    设置 TestCase 的名称
    
7.  `void setUp()`
    
    创建固定装置，例如打开一个网络连接
    
8.  `void tearDown()`
    
    拆除固定装置，例如关闭一个网络连接
    
9.  `String toString()`
    
    返回测试案例的一个字符串表示
    

TestSuite 类的主要方法：

1.  `void addTest(Test test)`
    
    在套中加入测试
    
2.  `void addTestSuite(Class<? extends TestCase> testCase)`
    
    将已经给定的类中的测试加到套中
    
3.  `int countTestCases()`
    
    对这个测试即将运行的测试案例进行计数
    
4.  `String getName()`
    
    返回套的名称
    
5.  `void run(TestResult result)`
    
    在 TestResult 中运行测试并收集结果
    
6.  `void setName(String name)`
    
    设置套的名称
    
7.  `Test testAt(int index)`
    
    在给定的目录中返回测试
    
8.  `int testCount()`
    
    返回套中测试的数量
    
9.  `static Test warning(String warning)`
    
    返回会失败的测试并且记录警告信息
    

TestResult 类的主要方法：

1. `void addError(Test test, Throwable t)`

   在错误列表中加入一个错误

2. `void addFailure(Test test, AssertionFailedError t)`

   在失败列表中加入一个失败

3. `void endTest(Test test)`

   显示测试被编译的这个结果

4. `int errorCount()`

   获取被检测出错误的数量

5. `Enumeration errors()`

   返回错误的详细信息

6. `int failureCount()`

   获取被检测出的失败的数量

7. `void run(TestCase test)`

   运行 TestCase

8. `int runCount()`

   获得运行测试的数量

9. `void startTest(Test test)`

   声明一个测试即将开始

10. `void stop()`

    标明测试必须停止


# JUnit 的主要注解

@Test—— 将一个类中方法修饰成一个测试方法

@BeforeClass—— 在所有方法执行前被执行，全局只会执行一次，而且是第一个运行

@AfterClass—— 在所有方法执行后被执行，全局只会执行一次，而且是最后一个运行

@Before—— 在每一个测试方法被运行前执行一次

@After—— 在每一个测试方法被运行后执行一次

@Ignore—— 所修饰的测试方法会被测试运行器忽略

@RunWith—— 更改测试运行器 org.junit.runner.Runner

# Eclipse 中 JUnit 的安装及初始使用

1. 下载：[JUnit – About](https://junit.org/junit4/)

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230414165610.png)

   点击”Download and install“

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230414165745.png)

   点击 `junit.jar`

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230414170010.png)

   点击右边栏 “Latest version” 后的 “View all”

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230414170124.png)

   点击 “Browse”

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230414170215.png)

   点击 “junit-4.13.2.jar” 即可下载

2. 同理下载 “hamcrest-core-2.2-javadoc.jar”

3. 为项目添加 JUnit 类库

   打开 eclipse -> 点击项目名 -> 再点击菜单栏的 “Project” -> Properties

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230414171248.png)

   选择 Java Build Path -> Libraries -> Add Library -> JUnit -> Next -> 下拉 “JUnit library version” 选择 “JUnit 4” -> 点击 “Finish”

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230415200905.png)

4. 在 eclipse 中添加 junit.jar 包和 hamcrest.core.jar 包

   点击 JUnit 4 -> Add External JARs -> 选择刚刚下载的 2 个 jar 包 -> 点击 “打开” 则添加成功

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230415201220.png)

   点击 “Apply and Close”

5. 为项目添加 JUnit 库

   项目名右键 -> properties -> Java Build Path -> Libraries -> Add Library -> JUnit -> Next -> Apply and Close

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230415202220.png)

   这里表示你已经添加 JUnit 类库成功

6. 生成 JUnit 测试框架

   在需要测试的类文件名处右键 -> new -> Other... -> JUnit -> JUnit Test Case -> 选择 “New JUnit 4 test” -> 更改”Package“ -> 勾选”@Before setUp ()“ -> Next

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230415203136.png)

   勾选 “GetMax” 和 “Object” -> Finish

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230415203328.png)

7. 编写测试代码

8. 使用 JUnit 运行测试代码

   GetMaxTest.java 中点击鼠标右键 -> Run As -> 1 JUnit Test 启动 JUnit

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230415203712.png)

9. 运行

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20230415203857.png)

