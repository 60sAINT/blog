---
title: 二.C++核心编程
cover: assets/cpp.png
categories:
- [C++]
tags:
  - C++
---

本阶段主要针对C++==面向对象==编程技术做详细讲解，探讨C++中的核心和精髓。

# 1 内存分区模型

C++程序在执行时，将内存大方向划分为**4个区域**

- 代码区：存放函数体的二进制代码，由操作系统进行管理的

  > 写的所有的代码都会放到代码区，所有的英文字母和中文注释都会放到代码区
  >
  > 先把程序员写的代码转成0、1数据，再放到代码区

- 全局区：存放全局变量和静态变量以及常量

- 栈区：由编译器自动分配释放, 存放函数的参数值,局部变量等

- 堆区：由程序员分配和释放,若程序员不释放,程序结束时由操作系统回收

**内存四区意义：**

不同区域存放的数据，赋予不同的生命周期, 给我们更大的灵活编程

## 1.1 程序运行前

在程序编译后，生成了exe可执行程序，**未执行该程序前**分为两个区域

**代码区：**

- 存放 CPU 执行的机器指令

  > 就是写的所有代码的二进制0、1

- 代码区是**共享**的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可

- 代码区是**只读**的，使其只读的原因是防止程序意外地修改了它的指令

**全局区：**

- 全局变量和静态变量存放在此.
- 全局区还包含了常量区, 字符串常量和const修饰的全局常量也存放在此.
- ==该区域的数据在程序结束后由操作系统释放==.

**示例：**

```c++
#include <iostream>
using namespace std;

// 全局区：存放全局变量、静态变量、常量
// 全局变量：只要没有写在函数体中的变量都叫全局变量
int g_a = 10;
int g_b = 10;

// const修饰的全局变量，叫全局常量
const int c_g_a = 10;
const int c_g_b = 10;

int main() {
  // 创建普通局部变量：只要是写在函数体内的变量都叫局部变量
  int a = 10;
  int b = 10;
  cout << "局部变量a的地址为：" << &a << endl;  // 0x16fdfee28
  cout << "局部变量b的地址为：" << &b << endl;  // 0x16fdfee24

  cout << "全局变量g_a的地址为：" << &g_a << endl;  // 0x100008000
  cout << "全局变量g_b的地址为：" << &g_b << endl;  // 0x100008004
  // 局部变量16fdf开头，全局变量10000开头，不在一个段里。全局变量在全局区里，局部变量在四区中的另一个区域中

  // 静态变量：在普通变量的前面加static，属于静态变量
  static int s_a = 10;
  static int s_b = 10;
  cout << "静态变量s_a的地址为：" << &s_a << endl;  // 0x100008008
  cout << "静态变量s_b的地址为：" << &s_b << endl;  // 0x10000800c
  // 全局变量和静态变量放得很近，都在一个区域段中

  // 常量
  // 1.字符串常量
  cout << "字符串常量的地址为：" << &"hello world" << endl;  // 0x100003ef9
  // 2.const修饰的变量
  // 2.1 const修饰的全局变量
  cout << "全局常量 c_g_a的地址：" << &c_g_a << endl;  // 0x100003f00
  cout << "全局常量 c_g_b的地址：" << &c_g_b << endl;  // 0x100003f04
  // 2.2 const修饰的局部变量
  const int c_l_a = 10;
  const int c_l_b = 10;
  cout << "局部常量 c_l_a的地址为：" << &c_l_a << endl;  // 0x16fdfee20
  cout << "局部常量 c_l_b的地址为：" << &c_l_b << endl;  // 0x16fdfee1c

  return 0;
}
```

> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410141549873.png)

总结：

* C++中在程序运行前分为全局区和代码区
* 代码区特点是共享和只读
* 全局区中存放全局变量、静态变量、常量区
* 常量区中存放 const修饰的全局常量  和 字符串常量


## 1.2 程序运行后

**栈区：**

由编译器自动分配释放, 存放函数的参数值,局部变量等

注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

**示例：**

```c++
int* func(int b) {// 形参数据也会放在栈区
  b = 100;
	int a = 10;// 局部变量，存放在栈区，栈区的数据在函数执行完后自动释放
	return &a;// 返回局部变量的地址
}

int main() {

	int* p = func(1);

	cout << *p << endl;// 10
	cout << *p << endl;// 267955168

	return 0;
}
```

> 函数 `func` 返回了一个指向局部变量 `a` 的指针。这是一个典型的错误用法，因为局部变量的生命周期仅限于函数的执行。当函数执行完毕时，局部变量 `a` 的内存空间会被释放，指向该内存的指针将变得无效。
>
> **第一次和第二次打印的差异**：
>
> - 第一次打印 `cout << *p << endl;` 可能仍然输出 `10`，因为在栈上，`a` 的内存空间可能没有被其他数据覆盖（这取决于编译器和运行时环境）。
> - 第二次打印 `cout << *p << endl;` 可能输出一个随机值（例如 `267955168`），因为 `a` 的内存空间已经被释放，可能被其他函数调用或操作覆盖。	

**堆区：**

由程序员分配释放,若程序员不释放,程序结束时由操作系统回收

> 这些数据不会一直放在内存上，只是在程序运行期间，如果程序员不释放它，它就不释放。当程序已经结束了，由系统回收

在C++中主要利用new在堆区开辟内存

**示例：**

```c++
int* func() {
  // 利用new关键字，可以将数据开辟到堆区。它会返回new在堆区开辟的这块内存的地址
	int* p = new int(10);// 指针本质也是局部变量，放在栈上，指针指向的数据是放在堆区
	return p;
}

int main() {

	int* p = func();

	cout << *p << endl;// 10
	cout << *p << endl;// 10

	return 0;
}
```

**总结：**

堆区数据由程序员管理开辟和释放

堆区数据利用new关键字进行开辟内存

## 1.3 new操作符

C++中利用==new==操作符在堆区开辟数据

堆区开辟的数据，由程序员手动开辟，手动释放，释放利用操作符 ==delete==

语法：` new 数据类型`

利用new创建的数据，会返回该数据对应的类型的指针

**示例1： 基本语法**

```c++
// 1、new的基本语法
int* func() {
	int* a = new int(10);
	return a;
}

int main() {

	int* p = func();

	cout << *p << endl;// 10
	cout << *p << endl;// 10

  // 堆区的数据，由程序员管理开辟，程序员管理释放
	// 如果想释放堆区的数据，利用关键字delete
	delete p;

	//cout << *p << endl; // 报错，内存已经被释放，再次访问就是非法操作

	return 0;
}
```

**示例2：开辟数组**

```c++
// 2、在堆区利用new开辟数组
int main() {
  int* arr = new int[10];// 返回这段连续线性空间的首地址

  for (int i = 0; i < 10; i++) {
    arr[i] = i + 100;
  }
  for (int i = 0; i < 10; i++) {
    cout << arr[i] << endl;
  }
  
  // 释放堆区数组 delete 后加 []
  delete[] arr;

  return 0;
}
```

> 在 C++ 中，`int* arr = new int[10];` 和 `int arr[10];` 这两种方式都可以用来创建一个整数数组，但它们之间有几个重要的区别：
>
> 1. **内存分配方式**
>
>    - **`int* arr = new int[10];`**：
>      - 这是动态内存分配。使用 `new` 关键字在堆（heap）上分配内存。
>      - 这意味着数组的大小可以在运行时确定，而不是在编译时。
>      - 可以根据需要动态创建不同大小的数组。例如，可以在运行时根据用户输入的值来决定数组的大小。
>    - **`int arr[10];`**：
>      - 这是静态内存分配。数组在栈（stack）上分配内存。
>      - 数组的大小必须在编译时确定，不能在运行时改变。
>
> 2. **生命周期**
>
>    - **动态数组** (`new int[10]`)：
>      - 生命周期由程序员控制。只有在调用 `delete[]` 时，内存才会被释放。
>      - 如果忘记释放，可能会导致内存泄漏。
>    - **静态数组** (`int arr[10]`)：
>      - 生命周期与其作用域相关。当数组超出其作用域（例如，当函数返回时）时，内存会自动释放。
>
> 3. **性能**
>
>    - **动态内存分配**：
>
>      相比于静态数组，动态分配可能稍慢，因为它涉及到堆内存的管理和分配。
>
>    - **静态数组**：
>
>      通常更快，因为栈内存的分配和释放速度较快。

# 2 引用

## 2.1 引用的基本使用

**作用： **给变量起别名

**语法：** `数据类型& 别名 = 原名`

> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410151104186.png)

**示例：**

```C++
int main() {

	int a = 10;
	int& b = a;
	cout << "a = " << a << endl;// 10
	cout << "b = " << b << endl;// 10

	b = 100;
	cout << "a = " << a << endl;// 100
	cout << "b = " << b << endl;// 100

	return 0;
}
```

## 2.2 引用注意事项

* 引用必须初始化
* 引用在初始化后，不可以改变

> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410151109626.png)
>
> 假设下边还有一个变量c。b是a的别名，不可以后来更改成c的别名

示例：

```C++
int main() {

	int a = 10;
	int b = 20;
	//int& c; //错误，引用必须初始化
	int& c = a; //一旦初始化后，就不可以更改
	c = b; //这是赋值操作，不是更改引用

	cout << "a = " << a << endl;// 20
	cout << "b = " << b << endl;// 20
	cout << "c = " << c << endl;// 20

	return 0;
}
```

## 2.3 引用做函数参数

**作用：**函数传参时，可以利用引用的技术让形参修饰实参

**优点：**可以简化指针修改实参

**示例：**

```C++
// 1、值传递
void mySwap01(int a, int b) {
  int temp = a;
  a = b;
  b = temp;
  cout << "swap01 a = " << a << endl;  // 20
  cout << "swap01 b = " << b << endl;  // 10
}

// 2、地址传递
void mySwap02(int* a, int* b) {
  int temp = *a;
  *a = *b;
  *b = temp;
}

// 3、引用传递
void mySwap03(int& a, int& b) {
  int temp = a;
  a = b;
  b = temp;
}

int main() {
  int a = 10;
  int b = 20;

  mySwap01(a, b);               // 值传递，形参不会修饰实参
  cout << "a = " << a << endl;  // 10
  cout << "b = " << b << endl;  // 20

  mySwap02(&a, &b);             // 地址传递，形参会修饰实参
  cout << "a = " << a << endl;  // 20
  cout << "b = " << b << endl;  // 10

  mySwap03(a, b);               // 引用传递，形参会修饰实参
  cout << "a = " << a << endl;  // 10
  cout << "b = " << b << endl;  // 20

  return 0;
}
```

> 为什么引用传递，形参会修改实参？
>
> 当调用mySwap03的时候，a是实参中的a=20。但函数参数用引用的方式接收，所以函数参数中的a是实参a的别名。对别名做任何的操纵修改，其实就是在修改原名指向的数据

总结：通过引用参数产生的效果同按地址传递是一样的。引用的语法更清楚简单

## 2.4 引用做函数返回值

作用：引用是可以作为函数的返回值存在的

注意：**不要返回局部变量引用**

用法：函数调用作为左值

**示例：**

```C++
// 引用做函数的返回值
// 1、不要返回局部变量的引用
int& test01() {
  int a = 10;  // 局部变量存放在四区中的栈区
  return a;    // returan的就是这个a，并且用引用的方式返回了。相当于这个a有一个别名，把别名返回了
}

// 2、函数的调用可以作为左值
int& test02() {
  static int a = 10;  // 静态变量，存放在全局区，全局区上的数据在程序结束后系统释放
  return a;
}

int main() {
  int& ref = test01();              // 在等号的左侧用ref接收这个别名
  cout << "ref = " << ref << endl;  // 10。第一次结果正确，是因为编译器做了保留
  cout << "ref = " << ref << endl;  // 260090848。第二次结果错误，因为a的内存已经释放

  int& ref2 = test02();               // ref2是a的别名
  cout << "ref2 = " << ref2 << endl;  // 10
  cout << "ref2 = " << ref2 << endl;  // 10
  test02() = 1000;                    // 把1000赋值给原名。如果函数的返回值是引用，这个函数调用可以作为左值
  cout << "ref2 = " << ref2 << endl;  // 1000。用别名去访问原名a的这块内存
  cout << "ref2 = " << ref2 << endl;  // 1000
}
```

## 2.5 引用的本质

本质：**引用的本质在c++内部实现是一个指针常量.**

> 对于指针常量而言，指针的指向不可以修改，指针指向的值是可以改动的

讲解示例：

```C++
//发现是引用，转换为 int* const ref = &a;
void func(int& ref){
	ref = 100; // ref是引用，转换为*ref = 100
}
int main(){
	int a = 10;
    
    //自动转换为 int* const ref = &a; 指针常量是指针指向不可改，也说明为什么引用不可更改
	int& ref = a; 
	ref = 20; //内部发现ref是引用，自动帮我们转换为: *ref = 20;
    
	cout << "a:" << a << endl;
	cout << "ref:" << ref << endl;
    
	func(a);
	return 0;
}
```

结论：C++推荐用引用技术，因为语法方便，引用本质是指针常量，但是所有的指针操作编译器都帮我们做了

## 2.6 常量引用

**作用：**常量引用主要用来修饰形参，防止误操作

在函数形参列表中，可以加==const修饰形参==，防止形参改变实参

**示例：**

```C++
//引用使用的场景，通常用来修饰形参
void showValue(const int& v) {
	//v += 10;// 如果形参不加const，那么在这里修改了v，传进来的main中的实参a也会加10
	cout << v << endl;// 10
}

int main() {

	//int& ref = 10;// 报错。单写10，它是一个常量。而引用必须引一块合法的内存空间，比如在栈上的数据或者堆区的数据。而这个10是一个字面量，在常量区。
  
	//加入const就可以了，编译器优化代码，int temp = 10; const int& ref = temp;
	const int& ref = 10;
  /*相当于：
  int temp = 10;
  const int* const ref2 = &temp;
  cout << *ref2 << endl;// 10
  */

	//ref = 100;  // 报错。加入const后变为只读，不可以修改
	cout << ref << endl;// 10

	//函数中利用常量引用防止误操作修改实参
	int a = 10;
	showValue(a);

	return 0;
}
```

# 3 函数提高

## 3.1 函数默认参数

在C++中，函数的形参列表中的形参是可以有默认值的。

语法：` 返回值类型  函数名 （参数= 默认值）{}`

**示例：**

```C++
//1. 如果某个位置参数有默认值，那么从这个位置往后，从左向右，必须都要有默认值
int func(int a, int b = 10, int c = 10) {
	return a + b + c;
}

//2. 如果函数声明有默认值，函数实现的时候就不能有默认参数。声明和实现只能有一个有默认参数
int func2(int a = 10, int b = 10);
int func2(int a, int b) {
	return a + b;
}

int main() {

  // 如果调用函数时自己传入数据，就用传入的数据，如果没有才用默认值
	cout << "ret = " << func(20, 20) << endl;// 50
	cout << "ret = " << func(100) << endl;// 120
  cout << "ret = " << func2() << endl;// 20

	return 0;
}
```

## 3.2 函数占位参数

C++中函数的形参列表里可以有占位参数，用来做占位，调用函数时必须填补该位置

**语法：** `返回值类型 函数名 (数据类型){}`

在现阶段函数的占位参数存在意义不大，但是后面的课程中会用到该技术

**示例：**

```C++
//函数占位参数 ，占位参数也可以有默认参数
// 给第二个占位参数设置默认值：void func(int a, int = 10)
void func(int a, int) {
	cout << "this is func" << endl;
}

int main() {

	func(10,10); //占位参数必须填补

	return 0;
}
```

## 3.3 函数重载

### 3.3.1 函数重载概述

**作用：**函数名可以相同，提高复用性

**函数重载满足条件：**

* 同一个作用域下
* 函数名称相同
* 函数参数**类型不同**  或者 **个数不同** 或者 **顺序不同**

**注意:**  函数的返回值不可以作为函数重载的条件

**示例：**

```C++
//函数重载需要函数都在同一个作用域下
void func() {
	cout << "func 的调用！" << endl;
}
void func(int a) {// 参数个数不同
	cout << "func (int a) 的调用！" << endl;
}
void func(double a) {// 参数类型不同
	cout << "func (double a)的调用！" << endl;
}
void func(int a ,double b) {
	cout << "func (int a ,double b) 的调用！" << endl;
}
void func(double a ,int b) {// 参数顺序不同
	cout << "func (double a ,int b)的调用！" << endl;
}
//函数返回值类型不可以作为函数重载条件。func(3.14 , 10);可以调下边这个函数，也可以调上边的函数，编译器不知道到底调哪个，产生歧义，也就是二义性
//int func(double a, int b) {
//	cout << "func (double a ,int b)的调用！" << endl;
//}

int main() {

	func();// func 的调用！
	func(10);// func (int a) 的调用！
	func(3.14);// func (double a)的调用！
	func(10,3.14);// func (int a ,double b) 的调用！
	func(3.14 , 10);// func (double a ,int b)的调用！

	return 0;
}
```

### 3.3.2 函数重载注意事项

* 引用作为重载条件
* 函数重载碰到函数默认参数

**示例：**

```C++
//函数重载注意事项
//1、引用作为重载条件
void func(int& a) {
	cout << "func (int& a) 调用 " << endl;
}
void func(const int& a) {// 类型不同，属于函数重载
	cout << "func (const int& a) 调用 " << endl;
}

//2、函数重载碰到函数默认参数
void func2(int a, int b = 10) {
	cout << "func2(int a, int b = 10) 调用" << endl;
}
void func2(int a) {// 发生重载，参数个数不同
	cout << "func2(int a) 调用" << endl;
}

int main() {
	
	int a = 10;
	func(a); //调用无const
	func(10);//调用有const，相当于调用：const int& a = 10;，合法
  // func(10);假设如果调用无const，相当于形参接受实参是这样一个代码：int& a = 10;，引用直接等于10这种语法不能通过，因为引用必须要引一个合法的内存空间，要么引栈区要么引堆区，而10放在常量区

	//func2(10); //报错。当函数重载碰到默认参数，产生歧义即二义性。需要避免，当写函数重载的时候就不要写默认参数

	return 0;
}
```

# **4** 类和对象

C++面向对象的三大特性为：==封装、继承、多态==

C++认为==万事万物都皆为对象==，对象上有其属性和行为

**例如：**

人可以作为对象，属性有姓名、年龄、身高、体重...，行为有走、跑、跳、吃饭、唱歌...

车也可以作为对象，属性有轮胎、方向盘、车灯...,行为有载人、放音乐、放空调...

具有相同性质的==对象==，我们可以抽象称为==类==，人属于人类，车属于车类

## 4.1 封装

### 4.1.1 封装的意义

封装是C++面向对象三大特性之一

封装的意义：

* 将属性和行为作为一个整体，表现生活中的事物
* 将属性和行为加以权限控制

**封装意义一：**

在设计类的时候，属性和行为写在一起，表现事物

**语法：** `class 类名{   访问权限： 属性  / 行为  };`

**示例1：**设计一个圆类，求圆的周长

**示例代码：**

```C++
const double PI = 3.14;  // 圆周率

// class代表设计一个类，类后面紧跟着的就是类名称
class Circle {
  // 访问权限。public是公共权限
 public:
  // 属性
  int m_r;  // 半径

  // 行为
  double calculateZC() {  // 获取圆的周长
    return 2 * PI * m_r;
  }
};

int main() {
  Circle c1;    // 通过圆类创建具体的圆（对象）。即实例化：通过一个类，创建一个对象的过程
  c1.m_r = 10;  // 给圆对象的属性进行赋值
  cout << "圆的周长为：" << c1.calculateZC() << endl;

  return 0;
}
```

**示例2：**设计一个学生类，属性有姓名和学号，可以给姓名和学号赋值，可以显示学生的姓名和学号

**示例2代码：**

```C++
#include <iostream>
using namespace std;

// 学生类
class Student {
  // 类中的属性和行为，统一称为成员。属性也称为成员属性或成员变量；行为也称为成员函数或成员方法
 public:
  void setName(string name) {
    m_name = name;
  }
  void setID(int id) {
    m_id = id;
  }
  void showStudent() {
    cout << "name:" << m_name << " ID:" << m_id << endl;
  }

 public:
  string m_name;
  int m_id;
};

int main() {
  Student stu;
  stu.setName("德玛西亚");
  stu.setID(250);
  stu.showStudent();// name:德玛西亚 ID:250
  
  Student s2;
  s2.m_name = "李四";
  s2.m_id = 2;
  s2.showStudent();// name:李四 ID:2

  return 0;
}
```

**封装意义二：**

类在设计时，可以把属性和行为放在不同的权限下，加以控制

访问权限有三种：

1. public        公共权限  
2. protected 保护权限
3. private      私有权限

**示例：**

```C++
//三种权限
//公共权限  public     类内可以访问  类外可以访问
//保护权限  protected  类内可以访问  类外不可以访问  儿子也可以访问父亲中的保护内容
//私有权限  private    类内可以访问  类外不可以访问  儿子不可以访问父亲的私有内容

class Person {
	//姓名  公共权限
 public:
	string m_Name;

	//汽车  保护权限
 protected:
	string m_Car;

	//银行卡密码  私有权限
 private:
	int m_Password;

 public:
	void func() {
		m_Name = "张三";
		m_Car = "拖拉机";
		m_Password = 123456;
	}
};

int main() {

	Person p;
	p.m_Name = "李四";
	//p.m_Car = "奔驰";  //保护权限，类外访问不到
	//p.m_Password = 123; //私有权限，类外访问不到

	return 0;
}
```

### 4.1.2 struct和class区别

在C++中 struct和class唯一的**区别**就在于 **默认的访问权限不同**

区别：

* struct 默认权限为公共
* class   默认权限为私有

```C++
class C1 {
	int m_A; //默认是私有权限
};

struct C2 {
	int m_A;  //默认是公共权限
};

int main() {

	C1 c1;
	c1.m_A = 10; //错误，访问权限是私有

	C2 c2;
	c2.m_A = 10; //正确，访问权限是公共

	return 0;
}
```

> 在 C++ 中，`struct` 和 `class` 的内存存放方式是相似的，主要取决于它们的实例是如何创建的。它们的成员变量在内存中是连续存放的。
>
> **实例的存放位置**
>
> - **栈（Stack）**：
>   - 当在函数内部创建 `C1` 或 `C2` 的实例时（如 `C1 c1;` 和 `C2 c2;`），它们的内存会分配在栈上。
>   - 栈内存的分配和释放速度较快，且在函数返回时自动释放。
> - **堆（Heap）**：
>   - 如果使用 `new` 关键字创建 `C1` 或 `C2` 的实例（如 `C1* c1 = new C1();`），那么它们的内存会分配在堆上。
>   - 堆内存需要手动释放，使用 `delete` 关键字。

### 4.1.3 成员属性设置为私有

**优点1：**将所有成员属性设置为私有，可以自己控制读写权限

**优点2：**对于写权限，我们可以检测数据的有效性

**示例：**

```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
using namespace std;

class Person {
 private:
  string m_Name;   // 姓名，可读可写
  int m_Age = 18;  // 年龄，只读。默认值18。也可以写，必须在0到150之间
  string m_Idol;   // 偶像，只写

  // 提供一些公有的方法对私有的属性进行一些读写的控制
 public:
  // 设置姓名
  void setName(string name) {
    m_Name = name;
  }
  // 获取姓名
  string getName() {
    return m_Name;
  }
  // 获取年龄
  int getAge() {
    return m_Age;
  }
  // 设置年龄（0～150）
  void setAge(int age) {
    if (age < 0 || age > 150) {
      cout << "年龄" << age << "输入有误，赋值失败" << endl;
      return;
    }
    m_Age = age;
  }
  // 设置偶像
  void setIdol(string idol) {
    m_Idol = idol;
  }
};

int main() {
  Person p;

  // p.m_Name = "张三";
  // cout << "姓名：" << p.m_Name << endl;
  // 报错：成员 "Person::m_Name" (已声明 所在行数:7) 不可访问

  p.setName("张三");
  cout << "姓名：" << p.getName() << endl;
  p.setAge(160);
  cout << "年龄：" << p.getAge() << endl;
  p.setIdol("anna");

  return 0;
}
```

> `#define _CRT_SECURE_NO_WARNINGS` 是一个预处理指令，主要用于 Microsoft Visual C++ 编译器中的安全性警告控制。它的作用是禁用一些关于使用不安全的 C 标准库函数的警告。
>
> **背景**
>
> 在使用 C++ 的时候，特别是涉及到 C 标准库的函数（例如 `strcpy`、`sprintf` 等），这些函数被认为是不安全的，因为它们可能导致缓冲区溢出等安全问题。为了鼓励开发者使用更安全的替代函数（例如 `strcpy_s`、`sprintf_s` 等），Microsoft 的编译器会在代码中使用这些不安全函数时发出警告。
>
> **具体作用**
>
> - **禁用警告**：当你在代码中使用某些不安全的函数时，编译器会发出警告。如果你定义了 `_CRT_SECURE_NO_WARNINGS`，这些警告就会被禁用。
> - **方便开发**：在开发和调试阶段，可能会频繁使用一些传统的 C 函数，而不想每次都看到警告信息。定义这个宏可以减少这些干扰。

**练习案例1：设计立方体类**

设计立方体类(Cube)

求出立方体的面积和体积

分别用全局函数和成员函数判断两个立方体是否相等。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410151715505.png)

```cpp
class Cube {
 private:
  int m_L;
  int m_W;
  int m_H;

 public:
  // 设置长
  void setL(int l) {
    m_L = l;
  }
  // 获取长
  int getL() {
    return m_L;
  }
  // 设置宽
  void setW(int w) {
    m_W = w;
  }
  // 获取宽
  int getW() {
    return m_W;
  }
  // 设置高
  void setH(int h) {
    m_H = h;
  }
  // 获取高
  int getH() {
    return m_H;
  }
  // 获取立方体面积
  int calculateS() {
    return 2 * m_L * m_W + 2 * m_W * m_H + 2 * m_L * m_H;
  }
  // 获取立方体体积
  int calculateV() {
    return m_L * m_W * m_H;
  }

  // 利用成员函数判断两个立方体是否相等
  bool isSameByClass(Cube& c) {
    if (c.getL() == m_L && c.getW() == m_L && c.getH() == m_H) {
      return true;
    }
    return false;
  }
};

// 利用全局函数判断两个立方体是否相等
bool isSame(Cube& c1, Cube& c2) {  // 值传递会拷贝出来一份数据，为了传入数据的时候更简单，最好通过引用传递，就不会再拷贝出来一份数据了，而是用原始的数据
  if (c1.getL() == c2.getL() && c1.getW() == c2.getW() && c1.getH() == c2.getH()) {
    return true;
  }
  return false;
}

int main() {
  Cube c1;
  c1.setL(10);
  c1.setW(10);
  c1.setH(10);
  cout << "c1的面积为：" << c1.calculateS() << endl;
  cout << "c1的体积为：" << c1.calculateV() << endl;

  Cube c2;
  c2.setL(10);
  c2.setW(10);
  c2.setH(10);

  // 利用全局函数判断两个立方体是否相等
  bool ret = isSame(c1, c2);
  if (ret) {
    cout << "c1和c2是相等的" << endl;
  } else {
    cout << "c1和c2是不相等的" << endl;
  }
  // 利用成员函数判断两个立方体是否相等
  ret = c1.isSameByClass(c2);
  if (ret) {
    cout << "成员函数判断：c1和c2是相等的" << endl;
  } else {
    cout << "成员函数判断：c1和c2是不相等的" << endl;
  }

  return 0;
}
```

**练习案例2：点和圆的关系**

设计一个圆形类（Circle），和一个点类（Point），计算点和圆的关系。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410151738234.png)

> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410151759929.jpg)

1. point.h：

   ```cpp
   #pragma once  // 防止头文件重复包含
   #include <iostream>
   using namespace std;  // 标准的命名空间
   
   // 点类
   class Point {
    private:
     int m_X;
     int m_Y;
   
    public:
     // 设置x
     void setX(int x);  // 一般在一个类设计的时候，只需要成员函数的声明就可以了
     // 获取x
     int getX();
     // 设置y
     void setY(int y);
     // 获取y
     int getY();
   };
   ```

2. point.cpp：

   ```cpp
   #include "point.h"
   
   // 只需要留住函数的所有实现
   // 设置x
   void Point::setX(int x) {  // 函数写在这是一个全局函数，但我们知道这些都是成员函数。所以要加Point::告诉编译器这是Point作用域下的成员函数，否则会报错
     m_X = x;
   }
   // 获取x
   int Point::getX() {
     return m_X;
   }
   // 设置y
   void Point::setY(int y) {
     m_Y = y;
   }
   // 获取y
   int Point::getY() {
     return m_Y;
   }
   ```

3. circle.h：

   ```cpp
   #pragma once
   #include <iostream>
   using namespace std;
   #include "point.h"  // 在一个类中应用到另一个类，只需要把另一个类头文件包含进来
   
   // 圆类
   class Circle {
    private:
     int m_R;         // 半径
     Point m_Center;  // 圆心
   
    public:
     // 设置半径
     void setR(int r);
     // 获取半径
     int getR();
     // 设置圆心
     void setCenter(Point center);
     // 获取圆心
     Point getCenter();
   };
   ```

4. circle.cpp：

   ```cpp
   #include "circle.h"
   
   // 设置半径
   void Circle::setR(int r) {
     m_R = r;
   }
   // 获取半径
   int Circle::getR() {
     return m_R;
   }
   // 设置圆心
   void Circle::setCenter(Point center) {
     m_Center = center;
   }
   // 获取圆心
   Point Circle::getCenter() {
     return m_Center;
   }
   ```

5. main.cpp：

   ```cpp
   #include <iostream>
   using namespace std;
   #include "circle.h"
   #include "point.h"
   
   // 判断点和圆关系
   void isInCircle(Circle& c, Point& p) {
     // 计算圆心和点之间距离的平方
     int distance = (c.getCenter().getX() - p.getX()) * (c.getCenter().getX() - p.getX()) + (c.getCenter().getY() - p.getY()) * (c.getCenter().getY() - p.getY());
     // 计算半径的平方
     int rDistance = c.getR() * c.getR();
     // 判断关系
     if (distance == rDistance) {
       cout << "点在圆上" << endl;
     } else if (distance > rDistance) {
       cout << "点在圆外" << endl;
     } else {
       cout << "点在圆内" << endl;
     }
   }
   
   int main() {
     // 创建圆
     Circle c;
     c.setR(10);
     Point center;
     center.setX(10);
     center.setY(0);
     c.setCenter(center);
     // 创建点
     Point p;
     p.setX(10);
     p.setY(10);
   
     // 判断关系
     isInCircle(c, p);
   
     return 0;
   }
   ```

6. **运行 make 命令**：在项目根目录下的终端，输入以下命令来编译项目：

   ```makefile
   make
   ```

   这将根据 `Makefile` 中的规则编译源文件并生成可执行文件。

7. **运行程序**：编译成功后，可以运行生成的可执行文件：

   ```bash
   ./output/main
   ```

8. 项目结构如下：

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410151906691.png)

9. 清理项目

   如果想清理生成的对象文件和可执行文件，可以运行：

   ```makefile
   make clean
   ```

## 4.2 对象的初始化和清理

*  生活中我们买的电子产品都基本会有出厂设置，在某一天我们不用时候也会删除一些自己信息数据保证安全
*  C++中的面向对象来源于生活，每个对象也都会有初始设置以及 对象销毁前的清理数据的设置。

### 4.2.1 构造函数和析构函数

对象的**初始化和清理**也是两个非常重要的安全问题

- 一个对象或者变量没有初始状态，对其使用后果是未知


- 同样的使用完一个对象或变量，没有及时清理，也会造成一定的安全问题


c++利用了**构造函数**和**析构函数**解决上述问题，这两个函数将会被编译器自动调用，完成对象初始化和清理工作。

对象的初始化和清理工作是编译器强制要我们做的事情，因此如果**程序员不提供构造和析构，编译器会提供**

**编译器提供的构造函数和析构函数是空实现。**

> 相当于函数体是空的，一行代码都没有

* 构造函数：主要作用在于创建对象时为对象的成员属性初始化赋值，构造函数由编译器自动调用，无须手动调用。
* 析构函数：主要作用在于对象**销毁前**系统自动调用，执行一些清理工作。

**构造函数语法：**`类名(){}`

1. 构造函数，没有返回值也不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象时候会自动调用构造函数，无须手动调用,而且只会调用一次

**析构函数语法：** `~类名(){}`

1. 析构函数，没有返回值也不写void
2. 函数名称与类名相同,在名称前加上符号  ~
3. 析构函数不可以有参数，因此不可以发生重载
4. 程序在对象销毁前会自动调用析构，无须手动调用,而且只会调用一次

```C++
class Person {
 public:
	//构造函数
	Person() {
		cout << "Person的构造函数调用" << endl;
	}
	//析构函数
	~Person() {
		cout << "Person的析构函数调用" << endl;
	}
};

void test01() {
	Person p;// 在栈上的数据，test01执行完毕后，释放对象。所以在释放这个对象前，就会自动调用它的析构函数
}

int main() {
	
	test01();// 打印"Person的构造函数调用"和"Person的析构函数调用"
  Person p;// 打印"Person的构造函数调用"和"Person的析构函数调用"

	return 0;
}
```

### 4.2.2 构造函数的分类及调用

两种分类方式：

- 按参数分为： 有参构造和无参构造


- 按类型分为： 普通构造和拷贝构造


三种调用方式：

- 括号法
- 显示法
- 隐式转换法

**示例：**

```C++
class Person {
 public:
  // 构造函数按照参属分类：无参构造（默认构造）和有参构造
  Person() {
    cout << "Person的无参构造函数调用" << endl;
  }
  Person(int a) {
    age = a;
    cout << "Person的有参构造函数调用" << endl;
  }

  // 构造函数按照类型分类：普通构造和拷贝构造
  Person(const Person& p) {
    // 拷贝构造函数：要让一个Person复制一份一模一样的Person出来。所以要传一个Person进来，并且把传进来的Person的所有属性拷在要构造的Person身上
    // 不能把传进来的对象本体修改掉，所以要加一个const限定。其次在拷贝的同时还要按照引用的方式传进来
    cout << "Person的拷贝构造函数调用" << endl;
    age = p.age;
  }

  ~Person() {
    cout << "Person的析构函数调用" << endl;
  }

 public:
  int age;
};

// 调用
void test01() {
  // 1、括号法
  Person p;                // 依次打印"Person的无参构造函数调用"、"Person的析构函数调用"
  Person p2(10);           // 依次打印"Person的有参构造函数调用"、"Person的析构函数调用"
  Person p3(p2);           // 依次打印"Person的拷贝构造函数调用"、"Person的析构函数调用"
  cout << p2.age << endl;  // 10
  cout << p3.age << endl;  // 10
  // 注意事项1：调用默认构造函数的时候，不要加()。因为下面这行代码，编译器会认为是一个函数的声明，不会认为在创建对象
  Person p1();  // 没调用任何构造函数和析构函数，不会创建出来对象
  void func();  // 这叫函数声明，没有写大括号函数实现。在一个函数体内部可以写另一个函数声明，语法是允许的

  // 2、显示法
  Person p4;
  Person p5 = Person(10);  // 有参构造
  Person p6 = Person(p5);  // 拷贝构造
  /* 匿名对象。创建了一个对象，只不过这个对象没有名。但如果它放在等号右侧，等号的左侧就是这个对象的名
  特点：当前行执行结束后，系统会立即回收掉匿名对象。因为没有名，后边就没法使这个对象了*/
  Person(10);
  cout << "aaaaa" << endl;
  // 上面两行执行依次打印："Person的有参构造函数调用"、"Person的析构函数调用"、"aaaaa"
  // 注意事项2:不要利用拷贝构造函数，初始化匿名对象
  Person(p6);  // 报错："Person p6"重定义。编译器会认为Person (p6)等价于Person p6;对象声明

  // 3、隐式转换法
  Person p7 = 10;  // 相当于写了Person p7 = Person(10);，编译器来转换。有参构造
  Person p8 = p7;  // 拷贝构造
}
int main() {
  test01();
  return 0;
}
```

### 4.2.3 拷贝构造函数调用时机

C++中拷贝构造函数调用时机通常有三种情况

* 使用一个已经创建完毕的对象来初始化一个新对象
* 值传递的方式给函数参数传值
* 以值方式返回局部对象

**示例：**

```C++
class Person {
 public:
  Person() {
    cout << "Person默认构造函数调用" << endl;
  }
  Person(int age) {
    cout << "Person有参构造函数调用" << endl;
    m_Age = age;
  }
  Person(const Person& p) {
    cout << "Person拷贝构造函数调用" << endl;
    m_Age = p.m_Age;
  }

  ~Person() {
    cout << "Person析构函数调用" << endl;
  }

  int m_Age;
};

// 1、使用一个已经创建完毕的对象来初始化一个新对象
void test01() {
  Person p1(20);
  Person p2(p1);
  cout << "p2的年龄为：" << p2.m_Age << endl;  // 20
}

// 2、值传递的方式给函数参数传值
void doWork(Person p) {
  // 值传递的本质：拷贝一个临时的副本出来，并不会影响实参
}
void test02() {
  Person p;
  doWork(p);  // 调用doWrok的时候，把实参p传给形参的时候，会调用拷贝构造函数
}

// 3、值方式返回局部对象
Person doWork2() {
  Person p1;
  cout << &p1 << endl;
  return p1;
  // 局部变量在函数执行完毕后就被释放，所以返回的不是p1本身。用值方式来返回，会根据p1来创建出来一个新的对象，然后返回给test03
  // 返回局部对象 p1，按理说应该调用拷贝构造函数
}
void test03() {
  Person p = doWork2(); // 按理说这里应该构造一个新的对象 p
  cout << &p << endl;
}

int main() {
  test01();
  test02();
  test03();
  return 0;
}
```

> - 调用test03后的**预期输出**依次为：
>
>   ```bash
>   Person默认构造函数调用
>   0x16fdfedfc
>   Person拷贝构造函数调用
>   Person析构函数调用
>   0x16fdfefd4
>   Person析构函数调用
>   ```
>
>   可能认为在 `doWork2()` 返回 `p1` 时，应该调用 **拷贝构造函数**，因为 `p1` 是局部变量，而函数返回时需要将其拷贝或移动到 `test03` 中的 `p` 对象。然而，输出中并没有看到 **拷贝构造函数** 的调用。
>
> - **实际输出**依次为：
>
>   ```bash
>   Person默认构造函数调用
>   0x16fdfedfc
>   0x16fdfedfc
>   Person析构函数调用
>   ```
>
>   所观察到的行为实际上是由于编译器的优化机制所导致的。具体来说，这种现象是 **返回值优化（Return Value Optimization, RVO）** 或 **命名返回值优化（Named Return Value Optimization, NRVO）** 的结果。
>
>   由于编译器的优化（特别是 RVO 或 NRVO），编译器会**消除**不必要的对象拷贝。在 `doWork2` 中，编译器知道 `p1` 会被返回并赋值给 `test03` 中的 `p`，因此它会**直接在 `test03` 的 `p` 的内存位置上构造对象**，而不需要在 `doWork2` 内创建一个新的临时对象并拷贝或移动它。
>
>   也就是说，编译器优化了 `p1` 的生命周期和内存位置，使得它的内存地址与 `test03` 中的 `p` 是相同的。
>
> - **为什么没有调用拷贝构造函数？**
>
>   编译器在优化过程中**消除了拷贝构造**，因此你没有看到拷贝构造函数的调用，而是通过 RVO 或 NRVO 直接构造在目标位置上。这是 C++ 标准允许的优化，即使有这个优化，行为依然符合 C++ 规范。
>
> - **解释析构函数调用**
>
>   在函数 `doWork2()` 返回后，`p1` 没有被显式地销毁，因为 `p` 和 `p1` 实际上是同一个对象。程序运行到 `test03` 的结束时，`p` 的析构函数被调用，因此你看到析构函数的输出：
>
>   ```bash
>   Person析构函数调用
>   ```
>
> - **总结**
>
>   看到的现象是由编译器的返回值优化导致的，编译器避免了创建临时对象，并直接在目标位置构造了返回的对象，因此没有调用拷贝构造函数，且 `p1` 和 `p` 实际上是同一个对象，指向同一块内存。
>
>   要看到没有优化的效果，你可以尝试关闭编译器优化，或者在某些情况下通过禁止 RVO 来观察拷贝构造函数的调用。

### 4.2.4 构造函数调用规则

默认情况下，c++编译器至少给一个类添加3个函数

1．默认构造函数(无参，函数体为空)

2．默认析构函数(无参，函数体为空)

3．默认拷贝构造函数，对属性进行值拷贝

构造函数调用规则如下：

* 如果用户定义有参构造函数，c++不再提供默认无参构造，但是会提供默认拷贝构造


* 如果用户定义拷贝构造函数，c++不会再提供其他构造函数

示例：

```C++
class Person {
 public:
	//无参（默认）构造函数
	Person() {
		cout << "无参构造函数!" << endl;
	}
	//有参构造函数
	Person(int a) {
		age = a;
		cout << "有参构造函数!" << endl;
	}
	//拷贝构造函数
	Person(const Person& p) {
		age = p.age;
		cout << "拷贝构造函数!" << endl;
	}
	//析构函数
	~Person() {
		cout << "析构函数!" << endl;
	}
 public:
	int age;
};

void test01() {
	Person p1(18);
  
	//如果不写拷贝构造，编译器会自动添加拷贝构造，并且做浅拷贝操作
	Person p2(p1);
	cout << "p2的年龄为： " << p2.age << endl;// 18
}

void test02() {
	//如果用户已提供有参构造，编译器不会提供默认构造，会提供拷贝构造
	Person p1; //此时如果用户自己没有提供默认构造，会出错
	Person p2(10); //用户提供的有参
	Person p3(p2); //此时如果用户没有提供拷贝构造，编译器会提供

	//如果用户已提供拷贝构造，编译器不会提供其他构造函数
	Person p4; //此时如果用户自己没有提供默认构造，会出错
	Person p5(10); //此时如果用户自己没有提供有参，会出错
	Person p6(p5); //用户自己提供拷贝构造
}

int main() {

	test01();
  test02();

	return 0;
}
```

### 4.2.5 深拷贝与浅拷贝

深浅拷贝是面试经典问题，也是常见的一个坑

浅拷贝：简单的赋值拷贝操作

深拷贝：在堆区重新申请空间，进行拷贝操作

**示例：**

```C++
class Person {
 public:
  Person() {
    cout << "Person的默认构造函数调用" << endl;
  }
  Person(int age, int height) {
    m_Age = age;
    m_Height = new int(height);  // new int返回的就是指向int类型的指针
    // 堆区的数据由程序员手动开辟，也需要由程序员手动释放
    cout << "Person的有参构造函数调用" << endl;
  }
  ~Person() {
    // 析构代码，将堆区开辟数据做释放操作
    if (m_Height != NULL) {
      delete m_Height;  // 释放这块内存
      m_Height = NULL;  // 防止野指针出现，做滞空操作
    }
    cout << "Person的析构函数调用" << endl;
  }

  int m_Age;
  int* m_Height;  // 把身高的数据开辟到堆区
};

void test01() {
  Person p1(18, 160);
  cout << "p1的年龄为：" << p1.m_Age << " 身高为：" << *p1.m_Height << endl;
  Person p2(p1);
  cout << "pe的年龄为：" << p2.m_Age << " 身高为：" << *p2.m_Height << endl;
}
int main() {
  test01();
  return 0;
}
```

> 运行以上代码，程序崩溃
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410241638736.png)
>
> 浅拷贝操作`Person p2(p1)`会逐字节地把18拷到p2身上
>
> 代码出现问题主要就是m_Height身高出现问题了，因为这是一个指针，记录的0x0011。当`Person p2(p1)`调用拷贝构造函数做浅拷贝操作的时候，相当于把0x0011逐字节地拷贝到p2了，所以p2记录的指针也是0x0011
>
> 当提供析构代码的时候，p1和p2都会执行析构，栈后进先出，所以p2先被释放。p2被释放的时候会执行析构代码，m_Height0x0011不为空，释放0x0011指向的堆区内存。p1这个对象在自己被释放的时候也要执行析构代码，它的m_Height0x0011不为空，又来释放一次0x0011指向的堆区内存，但这块内存已经被p2释放过了，p1再去释放已经是非法操作了
>
> 这就是浅拷贝带来的问题，堆区的内存重复释放

浅拷贝的问题，要利用深拷贝进行解决。深拷贝就是重新在堆区申请一块内存，自己写一个拷贝构造函数，p1指向一块堆区，p2也指向一块堆区，堆区存放的数据是一样的，但指针指向的内存是不一样的。p2和p1去执行析构代码的时候，释放的不是同一块堆区，不会有同一个堆区重复释放的问题

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410251059692.png)

```cpp
class Person {
 public:
  Person() {
    cout << "Person的默认构造函数调用" << endl;
  }
  Person(int age, int height) {
    m_Age = age;
    m_Height = new int(height);
    cout << "Person的有参构造函数调用" << endl;
  }
  
  // 自己实现拷贝构造函数，解决浅拷贝带来的问题
  Person(const Person& p) {
    cout << "Person拷贝构造函数调用" << endl;
    m_Age = p.m_Age;
    // m_Height = p.m_Height;// 编译器默认实现就是这行代码
    // 深拷贝操作
    m_Height = new int(*p.m_Height);
  }
  
  ~Person() {
    if (m_Height != NULL) {
      delete m_Height;
      m_Height = NULL;
    }
    cout << "Person的析构函数调用" << endl;
  }

  int m_Age;
  int* m_Height;
};

void test01() {
  Person p1(18, 160);
  cout << "p1的年龄为：" << p1.m_Age << " 身高为：" << *p1.m_Height << endl;
  Person p2(p1);
  cout << "pe的年龄为：" << p2.m_Age << " 身高为：" << *p2.m_Height << endl;
}
```

总结：如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题

### 4.2.6 初始化列表

**作用：**

C++提供了初始化列表语法，用来初始化类中的属性

**语法：**`构造函数()：属性1(值1),属性2（值2）... {}`

**示例：**

```C++
class Person {
 public:
  // 传统方式初始化
  // Person(int a, int b, int c) {
  //	m_A = a;
  //	m_B = b;
  //	m_C = c;
  //}

  // 初始化列表方式初始化
  // Person() : m_A(10), m_B(20), m_C(30) {}对应的对象创建为Person p;，创建这个对象的同时调用这个构造函数，顺便将对象的属性初始化为10、20、30
  Person(int a, int b, int c) : m_A(a), m_B(b), m_C(c) {}

  void PrintPerson() {
    cout << "mA:" << m_A << endl;
    cout << "mB:" << m_B << endl;
    cout << "mC:" << m_C << endl;
  }

 private:
  int m_A;
  int m_B;
  int m_C;
};

int main() {
  Person p(1, 2, 3);
  p.PrintPerson();

  return 0;
}
```

### 4.2.7 类对象作为类成员

C++类中的成员可以是另一个类的对象，我们称该成员为 对象成员

例如：

```C++
class A {}
class B {
    A a；
}
```

B类中有对象A作为成员，A为对象成员

那么当创建B对象时，A与B的构造和析构的顺序是谁先谁后？

**示例：**

```C++
class Phone {
 public:
	Phone(string name) {
		m_PhoneName = name;
		cout << "Phone构造" << endl;
	}

	~Phone() {
		cout << "Phone析构" << endl;
	}

	string m_PhoneName;

};

class Person {
 public:

	//初始化列表可以告诉编译器调用哪一个构造函数
	Person(string name, string pName) :m_Name(name), m_Phone(pName) {
    // 传入第二个参数相当于Phone m_Phone = pName;。隐式转换法创建对象，编译器来转换成Phone m_Phone = Phone(pName);
		cout << "Person构造" << endl;
	}

	~Person() {
		cout << "Person析构" << endl;
	}

	void playGame() {
		cout << m_Name << " 使用" << m_Phone.m_PhoneName << " 牌手机! " << endl;
	}

	string m_Name;
	Phone m_Phone;

};

void test01() {
	//当类中成员是其他类对象时，我们称该成员为 对象成员
	//构造的顺序是 ：先调用对象成员的构造，再调用本类构造
	//析构顺序与构造相反
	Person p("张三" , "苹果X");
	p.playGame();

}

int main() {

	test01();
  // 依次打印"Phone构造"、"Person构造"、"张三 使用苹果X 牌手机！"、Person析构"、"Phone析构"

	return 0;
}
```

### 4.2.8 静态成员

静态成员就是在成员变量和成员函数前加上关键字static，称为静态成员

静态成员分为：

*  静态成员变量
   *  所有对象共享同一份数据
   * 在编译阶段分配内存
   
     > 分配内存时机并不是在创建一个对象之后分配在栈上或堆区。在编译阶段就分配内存，相当于程序还没有运行之前，生成了一个exe程序，还没有双击它之前就已经给它分配内存了。分在内存的全局区里
   * 类内声明，类外初始化
   
     > 这个数据必须得有一个初始值，否则没法去用它
   
   **示例 ：**
   
   ```C++
   class Person {
   	
    public:
   	static int m_A;
   	//静态成员变量特点：
   	//1 在编译阶段分配内存
   	//2 类内声明，类外初始化
   	//3 所有对象共享同一份数据
   
    private:
   	static int m_B; //静态成员变量也是有访问权限的
   };
   int Person::m_A = 10;// Person作用域下边的m_A静态成员
   int Person::m_B = 10;
   
   void test01() {
     // 静态成员变量不属于某个对象上，所有对象都共享同一份数据
   	// 因此静态成员变量有两种访问方式
   
   	//1、通过对象
   	Person p1;
     cout << "p1.m_A = " << p1.m_A << endl;// 10
   	p1.m_A = 100;
   	cout << "p1.m_A = " << p1.m_A << endl;// 100
   	Person p2;
   	p2.m_A = 200;
   	cout << "p1.m_A = " << p1.m_A << endl; // 200
   	cout << "p2.m_A = " << p2.m_A << endl;// 200
   
   	//2、通过类名
   	cout << "m_A = " << Person::m_A << endl;// 200
   	//cout << "m_B = " << Person::m_B << endl; //类外访问不到私有静态成员变量
   }
   
   int main() {
   
   	test01();
   
   	return 0;
   }
   ```
   
*  静态成员函数
   *  所有对象共享同一个函数
   *  静态成员函数只能访问静态成员变量
   
   **示例：**
   
   ```cpp
   class Person {
   
    public:
   
   	//静态成员函数特点：
   	//1 程序共享一个函数
   	//2 静态成员函数只能访问静态成员变量
   	static void func() {
   		cout << "func调用" << endl;
   		m_A = 100;// 静态成员函数可以访问静态成员变量
   		//m_B = 100; //错误，不可以访问非静态成员变量。因为func在内存中是只有一份的，m_B这个非静态成员变量必须创建一个对象来进行访问。当调用func这个函数的时候，这个函数体内部无法区分到底是哪个对象的m_B
   	}
   
   	static int m_A; //静态成员变量
   	int m_B;// 非静态成员变量
     
    private:
   	//静态成员函数也是有访问权限的
   	static void func2() {
   		cout << "func2调用" << endl;
   	}
   };
   int Person::m_A = 10;
   
   void test01() {
   	//静态成员变量两种访问方式
   
   	//1、通过对象
   	Person p1;
   	p1.func();
   
   	//2、通过类名
   	Person::func();
   
   	//Person::func2(); //类外访问不到私有静态成员函数
   }
   
   int main() {
   
   	test01();
   
   	return 0;
   }
   ```

## 4.3 C++对象模型和this指针

### 4.3.1 成员变量和成员函数分开存储

在C++中，类内的成员变量和成员函数分开存储

只有非静态成员变量才属于类的对象上

```C++
class Empty {
};

class Person {
 public:
	Person() {
		mA = 0;
	}
	int mA;// 非静态成员变量占对象空间，属于类的对象上
	static int mB;// 静态成员变量不占对象空间，不属于类的对象上
  
	void func() {
		cout << "mA:" << this->mA << endl;
	}// 函数也不占对象空间，所有函数共享一个函数实例，不属于类的对象上
  
	static void sfunc() {
	}// 静态成员函数也不占对象空间，不属于类的对象上
};

int main() {
  Empty e;
  cout << sizeof(e) << endl;
  // 空对象占用的内存空间为：1（字节）
  // C++编译器会给每个空对象也分配一个字节空间，是为了区分空对象占内存的位置。每个空对象也应该有一个独一无二的内存地址，不能跟其他对象占重的位置
  
  Person p;
  cout << sizeof(p) << endl;// 4

	cout << sizeof(Person) << endl;// 4。sizeof(Person) 只计算非静态成员变量的大小

	return 0;
}
```

### 4.3.2 this指针概念

通过4.3.1我们知道在C++中成员变量和成员函数是分开存储的

每一个非静态成员函数只会诞生一份函数实例，也就是说多个同类型的对象会共用一块代码

那么问题是：这一块代码是如何区分哪个对象调用自己的呢？

c++通过提供特殊的对象指针，this指针，解决上述问题。**this指针指向被调用的成员函数所属的对象**

this指针是隐含每一个非静态成员函数内的一种指针

this指针不需要定义，直接使用即可

this指针的用途：

*  当形参和成员变量同名时，可用this指针来区分
*  在类的非静态成员函数中返回对象本身，可使用return *this

```C++
class Person {
 public:

	Person(int age) {
		//1、当形参和成员变量同名时，可用this指针来区分
    //this指针指向被调用的成员函数所属的对象。谁调用这个有参构造，this就指向谁
		this->age = age;
	}

  // 如果该函数返回值类型是Person，那么打印p2.age = 20。因为以值的方式来返回，调用完第一次该函数，p2加了10岁之后，返回的是按照p2拷贝构造的一个新数据，调用了拷贝构造函数，因为用值的方式返回会复制一份新的数据出来，相当于返回的Person和p2自身是不一样的数据。每调用一次该函数，返回的都是新拷贝出来的对象，而不是调用这个函数的对象本身。但如果是用引用的方式来返回，不会创建新的对象，而是一直返回p2
	Person& PersonAddPerson(Person& p) {
		this->age += p.age;
		//返回对象本身
		return *this;// this指向p2的指针，而*this指向的就是p2这个对象本体
	}

	int age;
};

void test01() {
	Person p1(10);
	cout << "p1.age = " << p1.age << endl;// 10

	Person p2(10);
  p2.PersonAddPerson(p1).PersonAddPerson(p1).PersonAddPerson(p1);// 链式编程思想
	cout << "p2.age = " << p2.age << endl;// 40
}

int main() {
	test01();
	return 0;
}
```

### 4.3.3 空指针访问成员函数

C++中空指针也是可以调用成员函数的，但是也要注意有没有用到this指针

如果用到this指针，需要加以判断保证代码的健壮性

**示例：**

```C++
//空指针访问成员函数
class Person {
 public:

	void ShowClassName() {
		cout << "我是Person类!" << endl;
	}

	void ShowPerson() {
		if (this == NULL) {
			return;
		}// 若没有该判断，p->ShowPerson();报错
		cout << mAge << endl;// 其实在属性前面都默认加了一个this->，表示这是当前对象的属性。this指向p的指针，是一个空指针NULL，相当于this是一个空的东西，不可能从没有确定的对象上访问它的成员
	}

 public:
	int mAge;
};

void test01() {
	Person* p = NULL;
	p->ShowClassName(); //空指针，可以调用成员函数
	p->ShowPerson();  //但是如果成员函数中用到了this指针，就不可以了
}

int main() {
	test01();
	return 0;
}
```

### 4.3.4 const修饰成员函数

**常函数：**

* 成员函数后加const后我们称为这个函数为**常函数**
* 常函数内不可以修改成员属性
* 成员属性声明时加关键字mutable后，在常函数中依然可以修改

**常对象：**

* 声明对象前加const称该对象为常对象
* 常对象只能调用常函数

**示例：**

```C++
class Person {
 public:
	Person() {
		m_A = 0;
		m_B = 0;
	}

	//如果想让指针指向的值也不可以修改，需要声明常函数
	void showPerson() const {
    /*
    this指针的本质是一个指针常量Person* const this
		this = NULL; //不能修改指针的指向
		this->mA = 100; //但是this指针指向的对象的数据是可以修改的
		如果想让this指针指向的值也不允许修改，就要再加一个const变成const Person* const this。编译器就把这个const加在成员函数名后，相当于常函数。常函数里修改this指向的对象的属性值this->mA = 100;就报错了
		*/
    m_A = 100;// 报错

		//const修饰成员函数，表示指针指向的内存空间的数据不能修改，除了mutable修饰的变量
		this->m_B = 100;
	}
  
  void func() {
    m_A = 100;
  }

 public:
	int m_A;
	mutable int m_B; // 特殊变量，即使在常函数中，也可以修改这个值
};

void test01() {
  Person p;
  p.showPerson();
}

//const修饰对象  常对象
void test02() {

	const Person person; //在对象前加const，变为常对象  
	cout << person.m_A << endl;
	//person.mA = 100; //常对象不能修改成员变量的值,但是可以访问
	person.m_B = 100; //但是常对象可以修改mutable修饰的成员变量

	//常对象只能调用常函数
  person.showPerson();
	//person.func(); // 报错，因为常对象不能调用普通的成员函数。普通成员函数可以修改属性，而常对象本身就不允许修改属性，违背协定
}

int main() {
	test01();
  test02();
	return 0;
}
```


## 4.4 友元

生活中你的家有客厅(Public)，有你的卧室(Private)

客厅所有来的客人都可以进去，但是你的卧室是私有的，也就是说只有你能进去

但是呢，你也可以允许你的好闺蜜好基友进去。

在程序里，有些私有属性 也想让类外特殊的一些函数或者类进行访问，就需要用到友元的技术

友元的目的就是让一个函数或者类 访问另一个类中私有成员

友元的关键字为  ==friend==

友元的三种实现

* 全局函数做友元
* 类做友元
* 成员函数做友元

### 4.4.1 全局函数做友元

```C++
// 建筑物类
class Building {
	//告诉编译器 goodGay全局函数 是 Building类的好朋友，可以访问类中的私有内容
	friend void goodGay(Building* building);

 public:
	Building() {
		this->m_SittingRoom = "客厅";
		this->m_BedRoom = "卧室";
	}

 public:
	string m_SittingRoom; //客厅
 private:
	string m_BedRoom; //卧室
};

// 全局函数
void goodGay(Building* building) {
	cout << "好基友全局函数正在访问： " << building->m_SittingRoom << endl;
	cout << "好基友全局函数正在访问： " << building->m_BedRoom << endl;
}

int main(){
	Building b;
	goodGay(&b);
	return 0;
}
```

### 4.4.2 类做友元

```C++
class Building {
  // 告诉编译器 GoodGay类是Building类的好朋友，可以访问到Building类中私有成员
  friend class GoodGay;

 public:
  Building();

 public:
  string m_SittingRoom;  // 客厅
 private:
  string m_BedRoom;  // 卧室
};

class GoodGay {
 public:
  GoodGay();
  void visit();  // 参观函数，访问Building中的属性

 private:
  Building* building;
};

// 类外写成员函数
Building::Building() {
  this->m_SittingRoom = "客厅";
  this->m_BedRoom = "卧室";
}
GoodGay::GoodGay() {
  building = new Building;// new在堆区创建了一个建筑物对象，并让building指向new出来的对象
}
void GoodGay::visit() {
  cout << "好基友正在访问" << building->m_SittingRoom << endl;
  cout << "好基友正在访问" << building->m_BedRoom << endl;// 当让类作为好朋友之后，类中的成员函数都可以访问Building中的私有成员了
}

int main() {
  GoodGay gg;
  gg.visit();
  return 0;
}
```

### 4.4.3 成员函数做友元

```C++
class Building {
	//告诉编译器  GoodGay类中的visit成员函数 是Building好朋友，可以访问私有成员
	friend void GoodGay::visit();

 public:
	Building();

 public:
	string m_SittingRoom; //客厅
 private:
	string m_BedRoom;//卧室
};

class GoodGay {
 public:
	GoodGay();
	void visit(); // 只让visit函数作为Building的好朋友，可以发访问Building中私有成员
	void visit2(); // 让visit2函数不可以访问Building中私有成员

 private:
	Building* building;
};

Building::Building() {
	m_SittingRoom = "客厅";
	m_BedRoom = "卧室";
}
GoodGay::GoodGay() {
	building = new Building;
}
void GoodGay::visit() {
	cout << "visit函数正在访问" << building->m_SittingRoom << endl;
	cout << "visit函数正在访问" << building->m_BedRoom << endl;
}
void GoodGay::visit2() {
	cout << "visit2正在访问" << building->m_SittingRoom << endl;
	//cout << "visit2正在访问" << building->m_BedRoom << endl;
}

int main(){
  GoodGay gg;
	gg.visit();
  gg.visit2();
	return 0;
}
```

## 4.5 运算符重载

运算符重载概念：对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

> 加号对于内置数据类型比如两个整型，编译器知道该怎么运算。但假设是两个自定义的数据类型比如两个人person进行相加该怎么运算，编译器就不知道了。所以如果想告诉编译器两个自定义数据类型如何进行相加/减/乘/除，就需要利用运算符重载这个技术

### 4.5.1 加号运算符重载

> ```cpp
> class Person {
>  public:
>   int m_A;
>   int m_B;
> }
> 
> Person p1;
> p1.m_A = 10;
> p1.m_B = 10;
> Person p2;
> p2.m_A = 10;
> p2.m_B = 10;
> 
> Person p3 = p1 + p2;// 想创建第三个人，等于前两个人进行相加运算，相当于让p1的m_A属性加上p2的m_A属性赋值给p3的m_A属性，p1的m_B属性加上p2的m_B属性赋值给p3的m_B属性。编译器实现不了
> ```
>
> 先抛开运算符重载这个新技术，通过自己写一个成员函数，实现两个对象相加属性后返回新的对象
>
> ```cpp
> Person PersonAddPerson(Person &p) {
>   Person temp;
>   temp.m_A = this->m_A + p.m_A;
>   temp.m_B = this->m_B + p.m_B;
>   return temp;
> }
> ```
>
> 这个函数的名称是程序员自己起的。编译器给起了一个通用名称`operator+`
>
> ```cpp
> // 通过成员函数重载+号
> Person operator+(Person &p) {
>   Person temp;
>   temp.m_A = this->m_A + p.m_A;
>   temp.m_B = this->m_B + p.m_B;
>   return temp;
> }
> ```
>
> `Person p3 = p1.operator+(p2);`简化为`Person p3 = p1 + p2;`

作用：实现两个自定义数据类型相加的运算

```C++
class Person {
 public:
	Person() {};
	Person(int a, int b) {
		this->m_A = a;
		this->m_B = b;
	}
	// 1.成员函数实现 + 号运算符重载
	Person operator+(const Person& p) {
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}

 public:
	int m_A;
	int m_B;
};

// 2.全局函数实现 + 号运算符重载
Person operator+(const Person& p1, const Person& p2) {
	Person temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;
}
// Person p3 = operator+(p1, p2);简化为Person p3 = p1 + p2;

// 3.运算符重载 可以发生函数重载 
Person operator+(const Person& p2, int val) {
	Person temp;
	temp.m_A = p2.m_A + val;
	temp.m_B = p2.m_B + val;
	return temp;
}

int main() {

	Person p1(10, 10);
	Person p2(20, 20);

	//成员函数方式
	Person p3 = p2 + p1;  //相当于 p2.operaor+(p1)
	cout << "mA:" << p3.m_A << " mB:" << p3.m_B << endl;

	Person p4 = p3 + 10; //相当于 operator+(p3,10)
	cout << "mA:" << p4.m_A << " mB:" << p4.m_B << endl;

	return 0;
}
```

> 在表达式`Person p3 = p2 + p1;`中，`p2` 是 `Person` 类型的对象，`p1` 也是 `Person` 类型的对象。根据 C++ 的运算符重载规则，编译器会优先选择成员函数重载，而不是调用全局函数实现 + 号运算符重载，原因如下：
>
> - **优先级**：当左侧操作数是类的实例时，编译器会优先考虑成员函数重载。这是因为成员函数可以直接访问类的成员变量，而全局函数则需要通过参数来访问。
> - **调用方式**：在 `p2 + p1` 中，`p2` 是 `Person` 类型的对象，因此编译器会尝试调用 `p2` 的成员函数 `operator+`，即 `p2.operator+(p1)`。

总结1：对于内置的数据类型的表达式的的运算符是不可能改变的

> C++ 不允许对内置数据类型（如 `int`、`float`、`char` 等）进行运算符重载。运算符的行为是由语言本身定义的，不能被用户修改

总结2：不要滥用运算符重载







### 4.5.2 左移运算符重载

作用：可以输出自定义数据类型

> ```cpp
> int a = 10;
> cout << a << endl;
> 
> Person p;
> p.m_A = 10;
> p.m_B = 10;
> cout << p << endl;
> ```
>
> 可以输出整型变量。不能直接输出p这个对象，需要重载`<<`符号，让编译器知道p该怎么输出里边的属性

```C++
class Person {
	friend ostream& operator<<(ostream& out, Person& p);

 public:
	Person(int a, int b) {
		this->m_A = a;
		this->m_B = b;
	}

	// 不会利用成员函数重载左移运算符，因为p.operator<<(cout)不是我们想要的效果，简化版本p << cout;，无法实现cout在左侧
	//void operator<<(Person& p){
	//}

private:
	int m_A;
	int m_B;
};

// 只能利用全局函数实现左移重载
// 本质operator<<(cout, p)，简化cout << p。cout是标准输出流对象，通过标准输出流这个类创建出来的对象
// ostream对象全局只能有一个，所以要用引用的方式传递进形参，不能创建一个新的出来
ostream& operator<<(ostream& out, Person& p) {
	out << "a:" << p.m_A << " b:" << p.m_B;
	return out;
}

int main() {
	Person p1(10, 20);
	cout << p1 << "hello world" << endl; //链式编程。如果operator<<函数返回的是void，那么就无法继续后面的<< "hello world" << endl了
	return 0;
}
```

> 总结：重载左移运算符配合友元可以实现输出自定义数据类型

### 4.5.3 递增运算符重载

作用：通过重载递增运算符，实现自己的整型数据

```C++
// 自定义整型
class MyInteger {
  friend ostream& operator<<(ostream& cout, MyInteger myint);

 public:
  MyInteger() {
    m_Num = 0;
  }

  // 重载前置++运算符
  MyInteger& operator++() {
    // 如果返回的是MyInteger类型，那么test01输出myint就是1。因为第一次自增后返回的新对象，并没有返回myint本身，所以第二次自增操作的新对象，myint保持不变
    // 返回引用是为了一直对一个数据进行递增操作
    m_Num++;
    return *this;
  }
  // 重载后置++运算符
  MyInteger operator++(int) {
    // int代表占位参数，可以用于区分前置和后置递增
    // 返回的是一个局部对象，在当前函数执行完之后就被释放掉了，若还要返回引用就是非法操作了
    MyInteger temp = *this;
    m_Num++;
    return temp;
  }

 private:
  int m_Num;
};

// 重载 << 运算符
ostream& operator<<(ostream& cout, MyInteger myint) {
  cout << myint.m_Num;
  return cout;
}

void test01() {
  MyInteger myint;
  cout << ++(++myint) << endl;// 2
  cout << myint << endl;// 2
}

void test02() {
  MyInteger myint;
  cout << myint++ << endl;// 0
  cout << myint << endl;// 1
  cout << myint++ << endl;// 1
  cout << myint << endl;// 2
}

int main() {
  test01();
  test02();
  return 0;
}
```

> 总结： 前置递增返回引用，后置递增返回值

### 4.5.4 赋值运算符重载

c++编译器至少给一个类添加4个函数

1. 默认构造函数(无参，函数体为空)
2. 默认析构函数(无参，函数体为空)
3. 默认拷贝构造函数，对属性进行值拷贝
4. 赋值运算符 operator=, 对属性进行值拷贝

如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题

**示例：**

```C++
class Person {
 public:
	Person(int age) {
		//将年龄数据开辟到堆区
		m_Age = new int(age);
	}
	~Person() {
		if (m_Age != NULL) {
			delete m_Age;
			m_Age = NULL;
		}
	}

	//重载赋值运算符 
	Person& operator=(Person &p) {
		if (m_Age != NULL) {// 先判断是否有属性在堆区，如果有先释放干净，然后再深拷贝
			delete m_Age;
			m_Age = NULL;
		}
    
		//编译器提供的代码是浅拷贝
		//m_Age = p.m_Age;
		//提供深拷贝 解决浅拷贝的问题
		m_Age = new int(*p.m_Age);

		//返回自身
		return *this;
	}

	//年龄的指针
	int* m_Age;
};

void test01() {
	Person p1(18);
	Person p2(20);
	Person p3(30);

	p3 = p2 = p1; //赋值操作

	cout << "p1的年龄为：" << *p1.m_Age << endl;// 18
	cout << "p2的年龄为：" << *p2.m_Age << endl;// 18
	cout << "p3的年龄为：" << *p3.m_Age << endl;// 18
}

int main() {
	test01();

	//int a = 10;
	//int b = 20;
	//int c = 30;
	//c = b = a;
	//cout << "a = " << a << endl;// 10
	//cout << "b = " << b << endl;// 10
	//cout << "c = " << c << endl;// 10

	return 0;
}
```

### 4.5.5 关系运算符重载

**作用：**重载关系运算符，可以让两个自定义类型对象进行对比操作

**示例：**

```C++
class Person {
 public:
	Person(string name, int age) {
		this->m_Name = name;
		this->m_Age = age;
	};

	bool operator==(Person& p) {
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age) {
			return true;
		} else {
			return false;
		}
	}

	bool operator!=(Person& p) {
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age) {
			return false;
		} else {
			return true;
		}
	}

	string m_Name;
	int m_Age;
};

int main() {
	Person a("孙悟空", 18);
	Person b("孙悟空", 18);

	if (a == b) {
		cout << "a和b相等" << endl;
	} else {
		cout << "a和b不相等" << endl;
	}

	if (a != b) {
		cout << "a和b不相等" << endl;
	} else {
		cout << "a和b相等" << endl;
	}
	return 0;
}
```

### 4.5.6 函数调用运算符重载

* 函数调用运算符 ()  也可以重载
* 由于重载后使用的方式非常像函数的调用，因此称为仿函数
* 仿函数没有固定写法，非常灵活

**示例：**

```C++
// 打印输出类
class MyPrint {
 public:
	void operator()(string text) {
		cout << text << endl;
	}
};
void MyPrint02(string text) {
  cout << text << endl;
}
void test01() {
	//重载的（）操作符
	MyPrint myFunc;
	myFunc("hello world");// 由于使用起来非常类似于函数调用，因此称为仿函数
  MyPrint02("hello world");
}

class MyAdd {
 public:
	int operator()(int v1, int v2) {
		return v1 + v2;
	}
};
void test02() {
	MyAdd add;
	int ret = add(10, 10);
	cout << "ret = " << ret << endl;

	//匿名函数对象调用  
	cout << "MyAdd()(100,100) = " << MyAdd()(100, 100) << endl;// MyAdd()创建一个临时的匿名对象，当前行执行完立即被释放
}

int main() {
	test01();
	test02();
	return 0;
}
```

## 4.6 继承

**继承是面向对象三大特性之一**

有些类与类之间存在特殊的关系，例如下图中：

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202411051704613.png)

我们发现，定义这些类时，下级别的成员除了拥有上一级的共性，还有自己的特性。

这个时候我们就可以考虑利用继承的技术，减少重复代码

### 4.6.1 继承的基本语法

例如我们看到很多网站中，都有公共的头部，公共的底部，甚至公共的左侧列表，只有中心内容不同

接下来我们分别利用普通写法和继承的写法来实现网页中的内容，看一下继承存在的意义以及好处

**普通实现：**

```C++
//Java页面
class Java  {
 public:
	void header() {
		cout << "首页、公开课、登录、注册...（公共头部）" << endl;
	}
	void footer() {
		cout << "帮助中心、交流合作、站内地图...(公共底部)" << endl;
	}
	void left() {
		cout << "Java,Python,C++...(公共分类列表)" << endl;
	}
	void content() {
		cout << "JAVA学科视频" << endl;
	}
};

//Python页面
class Python {
 public:
	void header() {
		cout << "首页、公开课、登录、注册...（公共头部）" << endl;
	}
	void footer() {
		cout << "帮助中心、交流合作、站内地图...(公共底部)" << endl;
	}
	void left() {
		cout << "Java,Python,C++...(公共分类列表)" << endl;
	}
	void content() {
		cout << "Python学科视频" << endl;
	}
};

//C++页面
class CPP {
 public:
	void header() {
		cout << "首页、公开课、登录、注册...（公共头部）" << endl;
	}
	void footer() {
		cout << "帮助中心、交流合作、站内地图...(公共底部)" << endl;
	}
	void left() {
		cout << "Java,Python,C++...(公共分类列表)" << endl;
	}
	void content() {
		cout << "C++学科视频" << endl;
	}
};

int main() {
  //Java页面
	cout << "Java下载视频页面如下： " << endl;
	Java ja;
	ja.header();
	ja.footer();
	ja.left();
	ja.content();
	cout << "--------------------" << endl;

	//Python页面
	cout << "Python下载视频页面如下： " << endl;
	Python py;
	py.header();
	py.footer();
	py.left();
	py.content();
	cout << "--------------------" << endl;

	//C++页面
	cout << "C++下载视频页面如下： " << endl;
	CPP cp;
	cp.header();
	cp.footer();
	cp.left();
	cp.content();
  
	return 0;
}
```

**继承实现：**

```C++
//公共页面
class BasePage {
 public:
	void header() {
		cout << "首页、公开课、登录、注册...（公共头部）" << endl;
	}
	void footer() {
		cout << "帮助中心、交流合作、站内地图...(公共底部)" << endl;
	}
	void left() {
		cout << "Java,Python,C++...(公共分类列表)" << endl;
	}
};

//Java页面
class Java : public BasePage {
 public:
	void content() {
		cout << "JAVA学科视频" << endl;
	}
};
//Python页面
class Python : public BasePage {
 public:
	void content() {
		cout << "Python学科视频" << endl;
	}
};
//C++页面
class CPP : public BasePage {
 public:
	void content() {
		cout << "C++学科视频" << endl;
	}
};

int main() {
	//Java页面
	cout << "Java下载视频页面如下： " << endl;
	Java ja;
	ja.header();
	ja.footer();
	ja.left();
	ja.content();
	cout << "--------------------" << endl;

	//Python页面
	cout << "Python下载视频页面如下： " << endl;
	Python py;
	py.header();
	py.footer();
	py.left();
	py.content();
	cout << "--------------------" << endl;

	//C++页面
	cout << "C++下载视频页面如下： " << endl;
	CPP cp;
	cp.header();
	cp.footer();
	cp.left();
	cp.content();
  
	return 0;
}
```

**总结：**

继承的好处：==可以减少重复的代码==

class A : public B; 

A 类称为子类 或 派生类

B 类称为父类 或 基类

**派生类中的成员，包含两大部分**：

一类是从基类继承过来的，一类是自己增加的成员。

从基类继承过过来的表现其共性，而新增的成员体现了其个性。

### 4.6.2 继承方式

继承的语法：`class 子类 : 继承方式 父类`

**继承方式一共有三种：**

* 公共继承
* 保护继承
* 私有继承

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202411051722534.png)

> 父类中私有的内容，子类不管用哪种继承方式都不可以访问
>
> 如果是公共继承，那么在父类中公共的属性在子类中依然是公共的属性，在父类中是保护的属性在子类中依然是保护的属性
>
> 如果是保护继承，那么父类中公共权限的内容到子类中变为了保护权限，父类中保护权限的内容也变为了保护权限
>
> 如果是私有继承，父类中公共权限的内容到子类变为了私有权限，父类中的保护权限内容到子类中也变为了私有权限

**示例：**

```C++
class Base1 {
 public: 
	int m_A;
 protected:
	int m_B;
 private:
	int m_C;
};
//公共继承
class Son1 : public Base1 {
 public:
	void func() {
		m_A = 10; //可访问 public权限
		m_B = 10; //可访问 protected权限
		//m_C = 10; //不可访问
	}
};
void myClass() {
	Son1 s1;
	s1.m_A = 100; // 其他类只能访问到公共权限
  //s1.m_B = 100; // 报错，因为m_B到子类Son1继承了之后是保护权限，类外访问不到
}

//保护继承
class Base2 {
 public:
	int m_A;
 protected:
	int m_B;
 private:
	int m_C;
};
class Son2 : protected Base2 {
 public:
	void func() {
		m_A = 100; //可访问 protected权限
		m_B = 100; //可访问 protected权限
		//m_C = 100; //不可访问
	}
};
void myClass2() {
	Son2 s;
	//s.m_A = 1000; //不可访问
}

//私有继承
class Base3 {
 public:
	int m_A;
 protected:
	int m_B;
 private:
	int m_C;
};
class Son3 : private Base3 {
 public:
	void func() {
		m_A = 100; //可访问 private权限
		m_B = 100; //可访问 private权限
		//m_C = 100; //不可访问
	}
};
void test03() {
  Son3 s1;
  //s1.m_A = 1000;// 到Son3中变为私有成员，类外访问不到
  //s1.m_B = 1000;// 到Son3中变为私有成员，类外访问不到
}
class GrandSon3 : public Son3 {
 public:
	void func() {
		//Son3是私有继承，所以继承Son3的属性在GrandSon3中都无法访问到
		//m_A = 1000;
		//m_B = 1000;
		//m_C = 1000;
	}
};
```

### 4.6.3 继承中的对象模型

**问题：**从父类继承过来的成员，哪些属于子类对象中？

**示例：**

```C++
class Base {
 public:
	int m_A;
 protected:
	int m_B;
 private:
	int m_C;
};

//公共继承
class Son : public Base {
 public:
	int m_D;
};

int main() {

	cout << "sizeof Son = " << sizeof(Son) << endl;// Son 类对象所占用的内存大小：16字节
  // 父类中所有非静态成员属性都会被子类继承下去
  // 父类中私有成员属性是被编译器给隐藏了，因此是访问不到，但是确实被继承下去了

	return 0;
}
```

> 结论： 父类中私有成员也是被子类继承下去了，只是由编译器给隐藏后访问不到

### 4.6.4 继承中构造和析构顺序

子类继承父类后，当创建子类对象，也会调用父类的构造函数

问题：父类和子类的构造和析构顺序是谁先谁后？

**示例：**

```C++
class Base  {
 public:
	Base() {
		cout << "Base构造函数!" << endl;
	}
	~Base() {
		cout << "Base析构函数!" << endl;
	}
};

class Son : public Base {
 public:
	Son() {
		cout << "Son构造函数!" << endl;
	}
	~Son() {
		cout << "Son析构函数!" << endl;
	}
};

int main() {
  Base b;// 依次打印"Base构造函数!"和"Base析构函数!"
  
  // 当你创建子类对象的时候，由于做了一个继承的操作，所以肯定会把父类中所有属性拿到手，那么也会创建出来一个父类对象，同时调用父类对象中的构造函数，以及最后销毁的同时也要把父类的析构走一遍
  // 继承中 先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反
	Son s;
	return 0;
}
```

> 总结：继承中 先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反

### 4.6.5 继承同名成员处理方式

问题：当子类与父类出现同名的成员，如何通过子类对象，访问到子类或父类中同名的数据呢？

* 访问子类同名成员   直接访问即可
* 访问父类同名成员   需要加作用域

**示例：**

```C++
class Base {
 public:
	Base() {
		m_A = 100;
	}
	void func() {
		cout << "Base - func()调用" << endl;
	}
	void func(int a) {
		cout << "Base - func(int a)调用" << endl;
	}

 public:
	int m_A;
};

class Son : public Base {
 public:
	Son() {
		m_A = 200;
	}

	void func() {
		cout << "Son - func()调用" << endl;
	}
  
 public:
	int m_A;
};

int main() {
	Son s;

	cout << "Son下的m_A = " << s.m_A << endl;// 200
	cout << "Base下的m_A = " << s.Base::m_A << endl;// 100。如果通过子类对象访问到父类中同名成员属性，需要加作用域

	s.func();// "Son - func()调用"。直接调用，调用的是子类中的同名成员
	s.Base::func();// "Base - func()调用"
	s.Base::func(10);// "Base - func(int a)调用"
  //s.func(100);// 报错
  // 如果子类中出现和父类同名的成员函数，子类会隐藏父类中所有版本的同名成员函数
	// 如果想访问父类中被隐藏的同名成员函数，需要加父类的作用域
  // 如果去掉子类的func函数，那么s.func(100);会直接调用父类的func(int a)
  
	return EXIT_SUCCESS;
}
```

总结：

1. 子类对象可以直接访问到子类中同名成员
2. 子类对象加作用域可以访问到父类同名成员
3. 当子类与父类拥有同名的成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类中同名函数

### 4.6.6 继承同名静态成员处理方式

问题：继承中同名的静态成员在子类对象上如何进行访问？

静态成员和非静态成员出现同名，处理方式一致

- 访问子类同名成员   直接访问即可
- 访问父类同名成员   需要加作用域

**示例：**

```C++
class Base {
 public:
	static void func() {
		cout << "Base - static void func()" << endl;
	}
	static void func(int a) {
		cout << "Base - static void func(int a)" << endl;
	}
	static int m_A;
};
int Base::m_A = 100;

class Son : public Base {
 public:
	static void func() {
		cout << "Son - static void func()" << endl;
	}
	static int m_A;
};
int Son::m_A = 200;

//同名成员属性
void test01() {
	// 1、通过对象访问
	Son s;
	cout << "Son  下 m_A = " << s.m_A << endl;// 200
	cout << "Base 下 m_A = " << s.Base::m_A << endl;// 100

	// 2、通过类名访问
	cout << "Son  下 m_A = " << Son::m_A << endl;// 200
	cout << "Base 下 m_A = " << Son::Base::m_A << endl;// 100。第一个::代表通过类名方式访问，第二个::代表访问父类作用域下
}

//同名成员函数
void test02() {
	// 1、通过对象访问
	Son s;
	s.func();// "Son - static void func()"
	s.Base::func();// "Base - static void func()"

  // 2、通过类名访问
	Son::func();// "Son - static void func()"
	Son::Base::func();// "Base - static void func()"
  //Son::func(100);// 报错
	//出现同名，子类会隐藏掉父类中所有同名成员函数，需要加作作用域访问
	Son::Base::func(100);// "Base - static void func(int a)"
}

int main() {
	test01();
	test02();
	return 0;
}
```

> 总结：同名静态成员处理方式和非静态处理方式一样，只不过有两种访问的方式（通过对象 和 通过类名）

### 4.6.7 多继承语法

C++允许**一个类继承多个类**

语法：` class 子类 ：继承方式 父类1 ， 继承方式 父类2...`

多继承可能会引发父类中有同名成员出现，需要加作用域区分

**C++实际开发中不建议用多继承**

**示例：**

```C++
class Base1 {
 public:
	Base1() {
		m_A = 100;
	}
 public:
	int m_A;
};

class Base2 {
 public:
	Base2() {
		m_A = 200;  // 开始是m_B 不会出问题，但是改为mA就会出现不明确
	}
 public:
	int m_A;
};

// 语法：class 子类 ：继承方式 父类1, 继承方式 父类2 
class Son : public Base2, public Base1 {
 public:
	Son() {
		m_C = 300;
		m_D = 400;
	}
 public:
	int m_C;
	int m_D;
};

int main() {

	Son s;
	cout << "sizeof Son = " << sizeof(s) << endl;// 16
  //cout << s.m_A << endl;// 报错：不明确。二义性出现了
	cout << s.Base1::m_A << endl;// 100
	cout << s.Base2::m_A << endl;// 200
  //多继承容易产生成员同名的情况
  //当父类中出现同名成员，需要加类名作用域区分调用哪一个基类的成员

	return 0;
}
```

> 总结： 多继承中如果父类中出现了同名情况，子类使用时候要加作用域

### 4.6.8 菱形继承

**菱形继承概念：**

- 两个派生类继承同一个基类


- 又有某个类同时继承者两个派生类


- 这种继承被称为菱形继承，或者钻石继承

**典型的菱形继承案例：**

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202411051910954.png)

**菱形继承问题：**

1.     羊继承了动物的数据，驼同样继承了动物的数据，当草泥马使用数据时，就会产生二义性。

2.     草泥马继承自动物的数据继承了两份，其实我们应该清楚，这份数据我们只需要一份就可以。

**示例：**

```C++
class Animal {
 public:
	int m_Age;
};

// 利用虚继承解决菱形继承的问题。继承前加virtual关键字后，变为虚继承
// 此时公共的父类Animal称为虚基类
class Sheep : virtual public Animal {};
class Tuo   : virtual public Animal {};
class SheepTuo : public Sheep, public Tuo {};

int main() {
	SheepTuo st;
  
	st.Sheep::m_Age = 100;
	st.Tuo::m_Age = 200;
  // 当菱形继承，两个父类拥有相同数据，需要加以作用域区分
  // 这份数据我们知道只有一份就可以，菱形继承导致数据有两份，资源浪费
  
  cout << "st.Sheep::m_Age = " << st.Sheep::m_Age << endl;// 200
	cout << "st.Tuo::m_Age = " << st.Tuo::m_Age << endl;// 200。当发生虚继承之后，这份数据就只有一个了
  cout << "st.m_Age = " << st.m_Age << endl;// 200。不会出现不明确的情况了，因为这个数据只有一个
  
	return 0;
}
```

> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202411051932727.png)
>
> 右边是加virtual关键字前，数据有2份，一份是从Sheep过来的，一份是从Tuo过来的
>
> 左边是加virtual关键字后，m_Age只有一份了，而从Sheep和Tuo继承下来的是vbptr（virtual base pointer，虚基类指针），它会指向vbtable（virtual base table，虚基类表）
>
> 也就是说Sheep会指向它的虚基类表，即下面的`SheepTuo::$vbtable@Sheep@:`。这个表中记录了一个偏移量8。给Sheep的0加上8之后等于8，找到m_Age。指针通过偏移量也能找到m_Age，所以m_Age只有一份
>
> 同样当子类Tuo继承Animal的时候，Tuo也有它的vbptr指向它的虚基类表`SheepTuo::$vbtable@Tuo@:`，偏移量是4。Tuo的4+4也得8
>
> 其实Sheep和Tuo只是分别继承了一个指针

总结：

* 菱形继承带来的主要问题是子类继承两份相同的数据，导致资源浪费以及毫无意义
* 利用虚继承可以解决菱形继承问题

## 4.7 多态

### 4.7.1 多态的基本概念

**多态是C++面向对象三大特性之一**

多态分为两类

* 静态多态: 函数重载 和 运算符重载属于静态多态，复用函数名
* 动态多态: 派生类和虚函数实现运行时多态

静态多态和动态多态区别：

* 静态多态的函数地址早绑定  -  编译阶段确定函数地址
* 动态多态的函数地址晚绑定  -  运行阶段确定函数地址

下面通过案例进行讲解多态

> ```cpp
> class Animal {
>   public:
> 	void speak() {
> 		cout << "动物在说话" << endl;
> 	}
> };
> class Cat : public Animal {
>   public:
> 	void speak() {
> 		cout << "小猫在说话" << endl;
> 	}
> };
> class Dog : public Animal {
>   public:
> 	void speak() {
> 		cout << "小狗在说话" << endl;
> 	}
> };
> 
> // 执行说话的函数
> void doSpeak(Animal& animal) {
>   // animal.speak();地址早绑定，在编译阶段确定函数地址
> 	animal.speak();// 打印"动物在说话"。因为现在是一个Animal对象，它调用speak函数不管传猫传狗，都会走Animal里边的speak函数
> }
> 
> int main() {
> 
> 	Cat cat;
> 	doSpeak(cat);// Animal& animal = cat;父类引用在指向一个子类对象。C++中允许父子之间的类型转换，不需要做强制类型转换
> 
> 	Dog dog;
> 	doSpeak(dog);
> 
> 	return 0;
> }
> ```
>
> 如果想执行让猫说话，那么doSpeak函数的地址就不能提前绑定，需要在运行阶段进行绑定，地址晚绑定。在父类speak函数前加一个关键字virtual

```C++
class Animal {
 public:
	//speak函数就是虚函数
	//函数前面加上virtual关键字，变成虚函数，那么编译器在编译的时候就不能确定函数调用了。
	virtual void speak() {
		cout << "动物在说话" << endl;
	}
};
class Cat : public Animal {
 public:
	void speak() {
		cout << "小猫在说话" << endl;
	}
};
class Dog : public Animal {
 public:
	void speak() {
		cout << "小狗在说话" << endl;
	}
};
//我们希望传入什么对象，那么就调用什么对象的函数
//如果函数地址在编译阶段就能确定，那么静态联编
//如果函数地址在运行阶段才能确定，就是动态联编

void doSpeak(Animal& animal) {
	animal.speak();// speak函数的地址不能提前确定出来，要看animal是什么对象。当运行的时候，speak函数发现传的是猫，那就走猫说话的代码，传的是狗就走狗的函数代码。speak属于多种形态了，由于传入的对象不同走不同的函数，确定的不同函数地址
}
/*动态多态满足条件： 
  1、有继承关系
  2、子类重写父类中的虚函数。重写：函数返回值类型、函数名、参数列表完全相同
  动态多态使用：
  父类指针或引用指向子类对象，就可以实现动态的多态了。当去调虚函数speak的时候，就会产生地址晚绑定，运行时候再绑定地址了*/

int main() {

	Cat cat;
	doSpeak(cat);// 小猫在说话

	Dog dog;
	doSpeak(dog);// 小狗在说话

	return 0;
}
```

总结：

多态满足条件

* 有继承关系
* 子类重写父类中的虚函数

多态使用条件

* 父类指针或引用指向子类对象

重写：函数返回值类型  函数名 参数列表 完全一致称为重写

> 补充：多态的原理剖析
>
> ```cpp
> class Animal {
>   public:
> 	void speak() {
> 		cout << "动物在说话" << endl;
> 	}
> };
> 
> int main() {
>     cout << "sizeof Animal = " << sizeof(Animal) << endl;// 1。Animal类只有一个非静态的成员函数，不属于类的对象上，Animal类似于一个空类，空类的大小是1字节
> 	return 0;
> }
> ```
>
> ```cpp
> class Animal {
>   public:
> 	virtual void speak() {
> 		cout << "动物在说话" << endl;
> 	}
> };
> class Cat : public Animal {
>   public:
> };
> 
> int main() {
>     cout << "sizeof Animal = " << sizeof(Animal) << endl;// 4。这4个字节是一个指针vfptr（virtual function pointer，虚函数指针），指向一个虚函数表vftable（virtual function table），表内部记录虚函数的入口地址
> 	return 0;
> }
> ```
>
> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202411061426671.png)
>
> 如果没有发生重写的情况，就是一个继承，把父类中所有的内容全都拿过来一份，包括指针。子类中也有一个vfptr并且指向了一个子类的虚函数表，记录的也是一个虚函数
>
> 当子类重写父类的虚函数：
>
> ```cpp
> class Animal {
>  public:
> 	virtual void speak() {
> 		cout << "动物在说话" << endl;
> 	}
> };
> class Cat : public Animal {
>  public:
>   void speak() {
> 		cout << "小猫在说话" << endl;
> 	}
> };
> 
> int main() {
>   cout << "sizeof Animal = " << sizeof(Animal) << endl;
> 	return 0;
> }
> ```
>
> 子类中的虚函数表内部会替换成子类的函数地址`&Cat::speak`.。父类虚函数表中的函数入口地址依然是有的，子类只是替换子类的虚函数表
>
> ```cpp
> // 当父类的指针或者引用指向子类对象时，发生多态
> Animal& animal = cat;
> animal.speak();
> ```
>
> 当animal去调用speak函数的时候，由于指向的是一个Cat对象，所以会从Cat的虚函数表中去找speak函数，相当于在运行阶段发生了动态多态，最后调用Cat的说话。如果animal指向Dog对象，那就从Dog的虚函数表中调用Dog的说话

### 4.7.2 多态案例一-计算器类

案例描述：

分别利用普通写法和多态技术，设计实现两个操作数进行运算的计算器类

多态的优点：

* 代码组织结构清晰
* 可读性强
* 利于前期和后期的扩展以及维护

**示例：**

```C++
//普通实现
class Calculator {
 public:
	int getResult(string oper) {
		if (oper == "+") {
			return m_Num1 + m_Num2;
		}
		else if (oper == "-") {
			return m_Num1 - m_Num2;
		}
		else if (oper == "*") {
			return m_Num1 * m_Num2;
		}
		// 如果要提供新的运算，需要修改getResult函数体源码
    // 在真实开发中提倡开闭原则：对扩展进行开放，对修改进行关闭
	}
 public:
	int m_Num1;
	int m_Num2;
};

int main() {
	//普通实现测试
	Calculator c;
	c.m_Num1 = 10;
	c.m_Num2 = 10;
	cout << c.m_Num1 << " + " << c.m_Num2 << " = " << c.getResult("+") << endl;
	cout << c.m_Num1 << " - " << c.m_Num2 << " = " << c.getResult("-") << endl;
	cout << c.m_Num1 << " * " << c.m_Num2 << " = " << c.getResult("*") << endl;
  
	return 0;
}
```

```cpp
//多态实现
//实现计算器抽象类：什么功能都不写，只是把getResult函数抽象出来
//多态优点：代码组织结构清晰，可读性强。利于前期和后期的扩展以及维护，比如现在想写一个除法，不用修改原来写的内容，只需要再往后扩展一个计算器
class AbstractCalculator {
 public :
	virtual int getResult() {
		return 0;
	}

	int m_Num1;
	int m_Num2;
};

//加法计算器
class AddCalculator : public AbstractCalculator {
 public:
	int getResult() {
		return m_Num1 + m_Num2;
	}
};

//减法计算器
class SubCalculator : public AbstractCalculator {
 public:
	int getResult() {
		return m_Num1 - m_Num2;
	}
};

//乘法计算器
class MulCalculator : public AbstractCalculator {
 public:
	int getResult() {
		return m_Num1 * m_Num2;
	}
};

int main() {
	//创建加法计算器
	AbstractCalculator* abc = new AddCalculator;// 多态使用条件：父类指针或者引用指向子类对象
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << " + " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;  //用完了记得销毁

	//创建减法计算器
	abc = new SubCalculator;// 重新让指针指向一个减法的计算器。刚才销毁掉指针是把堆区的数据释放了，但指针的类型没有变，还是父类的指针
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << " - " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;  

	//创建乘法计算器
	abc = new MulCalculator;
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << " * " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;
  
  return 0；
}
```

> 总结：C++开发提倡利用多态设计程序架构，因为多态优点很多

### 4.7.3 纯虚函数和抽象类

在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容

因此可以将虚函数改为**纯虚函数**

纯虚函数语法：`virtual 返回值类型 函数名 （参数列表）= 0 ;`

当类中有了==纯==虚函数，这个类也称为==抽象类==

**抽象类特点**：

 * 无法实例化对象
 * 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

**示例：**

```C++
class Base {
 public:
	//纯虚函数
	//类中只要有一个纯虚函数就称为抽象类
	//抽象类无法实例化对象
	//子类必须重写父类中的纯虚函数，否则也属于抽象类
	virtual void func() = 0;
};

class Son : public Base {
 public:
	virtual void func() {
		cout << "func调用" << endl;
	};
};

int main() {

  //Base b;// 报错：不允许使用抽象类类型"Base"的对象。抽象类无法实例化对象
	Base* base = NULL;
	//base = new Base; // 报错：不允许使用抽象类类型"Base"的对象
	base = new Son;
	base->func();
	delete base;//记得销毁

	return 0;
}
```

> 补充：`delete base;`出现了一个警告信息：`delete called on 'Base' that is abstract but has non-virtual destructor`。这个警告的意思是你在删除一个抽象类的指针（`Base*`），而这个抽象类没有虚析构函数。这会导致潜在的内存泄漏或未定义行为，因为在删除基类指针时，派生类的析构函数不会被调用
>
> 为了避免这个问题，你需要在基类 `Base` 中添加一个虚析构函数。这样，当你通过基类指针删除派生类对象时，派生类的析构函数会被正确调用，从而确保资源的正确释放。
>
> 以下是修改后的代码：
>
> ```cpp
> class Base {
>  public:
>   virtual void func() = 0;
> 
>   // 添加虚析构函数
>   virtual ~Base() {
>     // 可以在这里添加清理代码
>   }
> };
> 
> class Son : public Base {
>  public:
>   virtual void func() override {
>     cout << "func调用" << endl;
>   }
> };
> 
> int main() {
>   Base* base = nullptr;
>   base = new Son;
>   base->func();
>   delete base;  // 现在可以安全地删除
> 
>   return 0;
> }
> ```
>
> **解释**
>
> 1. **虚析构函数**：
>
>    在 `Base` 类中添加了 `virtual ~Base() {}`，这使得 `Base` 类成为一个具有虚析构函数的类。这样，当你删除一个指向 `Son` 的 `Base` 指针时，`Son` 的析构函数会被调用，确保所有资源都被正确释放。
>
> 2. **使用 `override` 关键字**：
>
>    在 `Son` 类中，使用 `override` 关键字来明确表示 `func` 函数是重写基类的虚函数。这有助于提高代码的可读性和安全性。

### 4.7.4 多态案例二-制作饮品

**案例描述：**

制作饮品的大致流程为：煮水 -  冲泡 - 倒入杯中 - 加入辅料

利用多态技术实现本案例，提供抽象制作饮品基类，提供子类制作咖啡和茶叶

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202411061538992.png)

**示例：**

```C++
//抽象制作饮品
class AbstractDrinking {
 public:
	//烧水
	virtual void Boil() = 0;
	//冲泡
	virtual void Brew() = 0;
	//倒入杯中
	virtual void PourInCup() = 0;
	//加入辅料
	virtual void PutSomething() = 0;
	//规定流程
	void makeDrink() {
		Boil();
		Brew();
		PourInCup();
		PutSomething();
	}
};

//制作咖啡
class Coffee : public AbstractDrinking {
 public:
	//烧水
	virtual void Boil() {
		cout << "煮农夫山泉!" << endl;
	}
	//冲泡
	virtual void Brew() {
		cout << "冲泡咖啡!" << endl;
	}
	//倒入杯中
	virtual void PourInCup() {
		cout << "将咖啡倒入杯中!" << endl;
	}
	//加入辅料
	virtual void PutSomething() {
		cout << "加入牛奶!" << endl;
	}
};

//制作茶水
class Tea : public AbstractDrinking {
 public:
	//烧水
	virtual void Boil() {
		cout << "煮自来水!" << endl;
	}
	//冲泡
	virtual void Brew() {
		cout << "冲泡茶叶!" << endl;
	}
	//倒入杯中
	virtual void PourInCup() {
		cout << "将茶水倒入杯中!" << endl;
	}
	//加入辅料
	virtual void PutSomething() {
		cout << "加入枸杞!" << endl;
	}
};

//业务函数
void doWork(AbstractDrinking* drink) {
	drink->makeDrink();// 接口都一样是makeDrink，由于传入的对象不一样，会调用不同子类中的函数。这就属于多态，一个接口有多种形态
	delete drink;
}

int main() {

	doWork(new Coffee);
	cout << "--------------" << endl;
	doWork(new Tea);

	return 0;
}
```

### 4.7.5 虚析构和纯虚析构

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

解决方式：将父类中的析构函数改为**虚析构**或者**纯虚析构**

虚析构和纯虚析构共性：

* 可以解决父类指针释放子类对象
* 都需要有具体的函数实现

虚析构和纯虚析构区别：

* 如果是==纯==虚析构，该类属于抽象类，无法实例化对象

  > 如果类里只写了虚析构，没有纯虚函数的情况下，那么还是可以实例化对象的

虚析构语法：

`virtual ~类名(){}`

纯虚析构语法：

` virtual ~类名() = 0;`

`类名::~类名(){}`

**示例：**

```cpp
class Animal {
 public:
	Animal() {// 创建子类对象的时候先调用父类
		cout << "Animal 构造函数调用！" << endl;
	}
	virtual void speak() = 0;

	～Animal() {
    cout << "Animal析构函数调用" << endl;
  }
};

class Cat : public Animal {
 public:
	Cat(string name) {
		cout << "Cat构造函数调用！" << endl;
		m_Name = new string(name);// 在堆区创建了一个属性，应该在Cat的析构函数把堆区创建的姓名释放掉
	}
	virtual void speak() {
		cout << *m_Name <<  "小猫在说话!" << endl;
	}
	~Cat() {
		cout << "Cat析构函数调用!" << endl;
		if (this->m_Name != NULL) {
			delete m_Name;
			m_Name = NULL;
		}
	}

 public:
	string* m_Name;
};

int main() {

	Animal* animal = new Cat("Tom");
	animal->speak();
	delete animal;// 父类指针在析构的时候，不会调用子类析构函数，导致子类如果有堆区属性，出现内存泄漏

	return 0;
}
```

以上代码依次打印："Animal 构造函数调用！"、"Cat构造函数调用！"、"小猫在说话!"、"Animal析构函数调用"。Cat的析构函数没有调，说明堆区的m_Name数据没有释放干净，导致了内存泄漏

问题产生原因是用父类的指针指向子类对象，所以`delete animal`delete父类指针的时候并不会走子类的析构代码

解决方法就是把父类的析构改为虚析构，就会走子类的析构代码了

```C++
class Animal {
 public:
	Animal() {
		cout << "Animal 构造函数调用！" << endl;
	}
	virtual void Speak() = 0;

	//析构函数加上virtual关键字，变成虚析构函数。利用虚析构可以解决父类指针释放子类对象时不干净的问题。加了下列虚构函数后，依次打印："Animal 构造函数调用！"、"Cat构造函数调用！"、"小猫在说话!"、"Cat析构函数调用!"、"Animal虚析构函数调用！"
	//virtual ~Animal()
	//{
	//	cout << "Animal虚析构函数调用！" << endl;
	//}
  
	virtual ~Animal() = 0;// 纯虚析构。只写这行代码，不写Animal::~Animal()代码段时会报错。因为这行代码写完后只有一个声明，没有代码实现。虚析构也好，纯虚析构也好，必须需要声明也需要实现，因为假设父类中也有数据开辟到堆区了，实现体就有用了，可以把父类中的堆区数据delete释放干净
};
Animal::~Animal() {
	cout << "Animal 纯虚析构函数调用！" << endl;
}
//和包含普通纯虚函数的类一样，包含了纯虚析构函数的类也是一个抽象类。不能够被实例化。

class Cat : public Animal {
 public:
	Cat(string name) {
		cout << "Cat构造函数调用！" << endl;
		m_Name = new string(name)
	}
	virtual void Speak() {
		cout << *m_Name <<  "小猫在说话!" << endl;
	}
	~Cat() {
		cout << "Cat析构函数调用!" << endl;
		if (this->m_Name != NULL) {
			delete m_Name;
			m_Name = NULL;
		}
	}

 public:
	string* m_Name;
};

int main() {

	Animal* animal = new Cat("Tom");
	animal->Speak();

	//通过父类指针去释放，会导致子类对象可能清理不干净，造成内存泄漏
	//怎么解决？给基类增加一个虚析构函数
	//虚析构函数就是用来解决通过父类指针释放子类对象
	delete animal;

	return 0;
}
```

总结：

1. 虚析构或纯虚析构就是用来解决通过父类指针释放子类对象

2. 如果子类中没有堆区数据，可以不写为虚析构或纯虚析构

3. 拥有纯虚析构函数的类也属于抽象类

### 4.7.6 多态案例三-电脑组装

**案例描述：**

电脑主要组成部件为 CPU（用于计算），显卡（用于显示），内存条（用于存储）

将每个零件封装出抽象基类，并且提供不同的厂商生产不同的零件，例如Intel厂商和Lenovo厂商

创建电脑类提供让电脑工作的函数，并且调用每个零件工作的接口

测试时组装三台不同的电脑进行工作

> ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202411061653169.png)

**示例：**

```C++
#include <iostream>
using namespace std;

// 抽象不同零件类
// 抽象CPU类
class CPU {
 public:
  // 抽象的计算函数
  virtual void calculate() = 0;
};
// 抽象显卡类
class VideoCard {
 public:
  // 抽象的显示函数
  virtual void display() = 0;
};
// 抽象内存条类
class Memory {
 public:
  // 抽象的存储函数
  virtual void storage() = 0;
};

// 电脑类
class Computer {
 public:
  Computer(CPU* cpu, VideoCard* vc, Memory* mem) {
    m_cpu = cpu;
    m_vc = vc;
    m_mem = mem;
  }
  // 提供工作的函数
  void work() {
    // 让零件工作起来，调用接口
    m_cpu->calculate();
    m_vc->display();
    m_mem->storage();
  }
  // 提供析构函数，释放3个电脑零件
  ~Computer() {
    if (m_cpu != NULL) {
      delete m_cpu;
      m_cpu = NULL;
    }
    if (m_vc != NULL) {
      delete m_vc;
      m_vc = NULL;
    }
    if (m_mem != NULL) {
      delete m_mem;
      m_mem = NULL;
    }
  }

 private:
  CPU* m_cpu;       // CPU的零件指针
  VideoCard* m_vc;  // 显卡零件指针
  Memory* m_mem;    // 内存条零件指针
};

// 具体厂商
// Intel厂商
class IntelCPU : public CPU {
 public:
  virtual void calculate() {
    cout << "Intel的CPU开始计算了！" << endl;
  }
};
class IntelVideoCard : public VideoCard {
 public:
  virtual void display() {
    cout << "Intel的显卡开始显示了！" << endl;
  }
};
class IntelMemory : public Memory {
 public:
  virtual void storage() {
    cout << "Intel的内存条开始存储了！" << endl;
  }
};
// Lenovo厂商
class LenovoCPU : public CPU {
 public:
  virtual void calculate() {
    cout << "Lenovo的CPU开始计算了！" << endl;
  }
};
class LenovoVideoCard : public VideoCard {
 public:
  virtual void display() {
    cout << "Lenovo的显卡开始显示了！" << endl;
  }
};
class LenovoMemory : public Memory {
 public:
  virtual void storage() {
    cout << "Lenovo的内存条开始存储了！" << endl;
  }
};

int main() {
  // 第一台电脑零件
  CPU* intelCPU = new IntelCPU;  // 用父类的指针来指向子类对象，利用多态
  VideoCard* intelCard = new IntelVideoCard;
  Memory* intelMem = new IntelMemory;
  cout << "第一台电脑开始工作：" << endl;
  // 创建第一台电脑
  Computer* computer1 = new Computer(intelCPU, intelCard, intelMem);
  computer1->work();
  delete computer1;  // 在释放电脑的时候，会走电脑的析构函数，可以在Computer的析构函数中把上面3个零件指针释放掉

  cout << "------------------------" << endl;
  cout << "第二台电脑开始工作：" << endl;
  // 第二台电脑组装
  Computer* computer2 = new Computer(new LenovoCPU, new LenovoVideoCard, new LenovoMemory);
  computer2->work();
  delete computer2;

  cout << "------------------------" << endl;
  cout << "第三台电脑开始工作：" << endl;
  // 第三台电脑组装
  Computer* computer3 = new Computer(new LenovoCPU, new IntelVideoCard, new LenovoMemory);
  computer3->work();
  delete computer3;

  return 0;
}
```

# 5 文件操作

程序运行时产生的数据都属于临时数据，程序一旦运行结束都会被释放

通过**文件可以将数据持久化**

C++中对文件操作需要包含头文件 ==&lt; fstream &gt;==

> C++对于文件操作也是基于面向对象的技术，它提供了一个叫文件流的管理的一个类，在使用文件操作的时候首先要包含头文件&lt;fstream&gt;文件流

文件类型分为两种：

1. **文本文件**     -  文件以文本的**ASCII码**形式存储在计算机中

   > 文本文件去写的文件的数据，双击之后可以用记事本或工具打开，能看到里边的内容，内容以明文的形式存在，能看懂。所以通常如果没有特殊需求，用文本文件就可以了
2. **二进制文件** -  文件以文本的**二进制**形式存储在计算机中，用户一般不能直接读懂它们

操作文件的三大类:

1. ofstream：写操作
2. ifstream： 读操作
3. fstream ： 读写操作

## 5.1 文本文件

### 5.1.1 写文件

写文件步骤如下：

1. 包含头文件

   \#include <fstream\>

2. 创建流对象

   ofstream ofs;

   > 通过ofstream输出流这个类创建出来一个对象，通过这个对象可以跟文件打交道，往里边写内容

3. 打开文件

   ofs.open("文件路径",打开方式);

4. 写数据

   ofs << "写入的数据";

   > 有一个cout左移运算符往屏幕上输出东西，cout叫输出流对象。ofs代表的就是文件的输出流对象，往文件中输出内容

5. 关闭文件

   ofs.close();


文件打开方式：

| 打开方式    | 解释                       |
| ----------- | -------------------------- |
| ios::in     | 为读文件而打开文件         |
| ios::out    | 为写文件而打开文件         |
| ios::ate    | 初始位置：文件尾           |
| ios::app    | 追加方式写文件             |
| ios::trunc  | 如果文件存在先删除，再创建 |
| ios::binary | 二进制方式                 |

**注意：** 文件打开方式可以配合使用，利用|操作符

**例如：**用二进制方式写文件 `ios::binary | ios:: out`

**示例：**

```C++
#include <iostream>
using namespace std;
#include <fstream>  // 1、包含头文件fstream

int main() {
  // 2、创建流对象
  ofstream ofs;

  // 3、指定打开方式
  ofs.open("test.txt", ios::out);
  // 文件的存放位置取决于运行程序的当前工作目录
  // 当前工作目录 是指程序运行时的目录。通常情况下，这个目录是在命令行中执行程序时所在的目录。

  // 4、写内容
  ofs << "姓名：张三" << endl;
  ofs << "姓别：男" << endl;
  ofs << "年龄：18" << endl;

  // 5、关闭文件
  ofs.close();
}
```

总结：

* 文件操作必须包含头文件 fstream
* 读文件可以利用 ofstream  ，或者fstream类
* 打开文件时候需要指定操作文件的路径，以及打开方式
* 利用<<可以向文件中写数据
* 操作完毕，要关闭文件

### 5.1.2 读文件

读文件与写文件步骤相似，但是读取方式相对于比较多

读文件步骤如下：

1. 包含头文件   

   \#include <fstream\>

2. 创建流对象  

   ifstream ifs;

3. 打开文件并判断文件是否打开成功

   ifs.open("文件路径",打开方式);

4. 读数据

   四种方式读取

5. 关闭文件

   ifs.close();

**示例：**

```C++
#include <iostream>
using namespace std;
#include <fstream>  // 1、包含头文件

int main() {
  // 2、创建流对象
  ifstream ifs;

  // 3、打开文件，并且判断是否打开成功
  ifs.open("test.txt", ios::in);
  if (!ifs.is_open()) {
    cout << "文件打开失败" << endl;
    return 0;
  }

  // 4、读数据
  // 第一种：字符数组初始化全为0，把文件中的数据全都放在字符数组中
  char buf[1024] = {0};
  while (ifs >> buf) {  // 利用ifs右移运算符放到buf数组中，当数据全部读入之后while循环也会退出去。内部机制就是一行一行读，读到头了会返回一个假标志
    cout << buf << endl;
  }
  // 第二种：用ifs的成员函数getline一行一行获取
  char buf2[1024] = {0};
  while (ifs.getline(buf2, sizeof(buf2))) {
    // getline要两个参数。第一个char*，把数据放到哪个地方，数组名就指向第一个元素的地址；第二个count，意思是最多读多少个字节数，准备了多大空间，也可以直接写1024，因为一个char占1个字节
    cout << buf2 << endl;
  }
  // 第三种：用全局的函数getline把所有数据放到C++的字符串中
  string buf3;
  while (getline(ifs, buf3)) {
    // 全局的getline要2个函数，第一个basic_istream基础的输入流对象，第二个读取到哪个字符串中
    cout << buf3 << endl;
  }
  // 第四种：把文件中所有数据一个一个字符全读出来，读完后放在字符里边。不推荐，因为一个一个字符读肯定没有一行一行读的速度快
  char c;
  while ((c = ifs.get()) != EOF) {
    // get函数每一次就读一个字符，把读到的字符放到字符c里边。最后判断是不是读到文件尾了，如果没有读到文件尾EOF(end of file)就一直读
    cout << c;
  }

  // 5、关闭文件
  ifs.close();

  return 0;
}
```

总结：

- 读文件可以利用 ifstream  ，或者fstream类
- 利用is_open函数可以判断文件是否打开成功
- close 关闭文件 

## 5.2 二进制文件

以二进制的方式对文件进行读写操作

打开方式要指定为 ==ios::binary==

### 5.2.1 写文件

二进制方式写文件主要利用流对象调用成员函数write

函数原型 ：`ostream& write(const char* buffer,int len);`

参数解释：字符指针buffer指向要写入的数据的地址。len是要写多少数据（单位：字节）

> 二进制操作文件就比较强大了，它不单单可以操纵内置的数据类型像int / double / float，也可以操纵自定义的数据类型（比如类）写到文件里边

**示例：**

```C++
using namespace std;
#include <fstream>//1、包含头文件
#include <string>

class Person {
 public:
	char m_Name[64];// 用C语言的字符数组代表字符串
	int m_Age;
};

int main() {

	/*2、创建输出流对象
  ofstream ofs;
  3、打开文件
	ofs.open("person.txt", ios::out | ios::binary);*/
	ofstream ofs("person.txt", ios::out | ios::binary);// ofstream类有构造函数，可以在创建对象的同时就指定路径以及打开方式

	//4、写文件
  Person p = {"张三", 18};
	ofs.write((const char*)&p, sizeof(p));// &p把数据的地址取到，并且强转成const char*。因为 write 方法的参数类型是 const char*，它期望接收一个指向字符数组的指针。const char* 是一个指向常量字符的指针，它表示指向字符数组的指针，且这些字符的内容不能被修改。使用 const 关键字可以确保指针指向的数据在使用过程中不会被意外修改。

	//5、关闭文件
	ofs.close();

	return 0;
}
```

总结：

* 文件输出流对象 可以通过write函数，以二进制方式写数据

### 5.2.2 读文件

二进制方式读文件主要利用流对象调用成员函数read

函数原型：`istream& read(char* buffer, int len);`

参数解释：字符指针buffer指向把数据读到的地址。len是准备的空间有多少（字节）

示例：

```C++
#include <fstream>// 1、包含头文件
#include <iostream>
#include <string>
using namespace std;

class Person {
 public:
	char m_Name[64];
	int m_Age;
};

int main() {

  // 2、创建流对象 & 3、打开文件
	ifstream ifs("person.txt", ios::in | ios::binary);
	if (!ifs.is_open()) {// 判断文件是否打开成功
		cout << "文件打开失败" << endl;
    return 0;
	}

  // 4、读文件
	Person p;
	ifs.read((char*)&p, sizeof(p));
	cout << "姓名： " << p.m_Name << " 年龄： " << p.m_Age << endl;
  
  // 5、关闭文件
  ifs.close();
  
	return 0;
}
```

- 文件输入流对象 可以通过read函数，以二进制方式读数据
