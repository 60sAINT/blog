---
title: 使用VS Code进行C++函数分文件编写
cover: assets/cppConfig.png
categories:
- [C++]
tags:
  - C++
  - VSCode
  - 环境
---

# 下载安装：C/C++ Project Generator扩展

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410131350485.png)

下载安装完成后，按ctrl+shift+p打开命令面板，输入create C++ project，按回车后可以选择保存工程的文件夹

创建好会后生成几个目录：

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410131351250.png)

.vscode：里面放一些配置文件之类的，如launch.json、setting.json、tasks.json

include：存放头文件

lib：存放项目所依赖的外部库文件

output：存放生成的可执行文件

src：存放源码文件

还有个Makefile文件

# 实例验证

在include文件夹中新建一个.h文件：swap.h，声明了一个函数，功能为交换两个数的值

```cpp
#include <iostream>
using namespace std;

// 函数的声明
void swap(int a, int b);
```

在src文件夹中新建swap.cpp、main.cpp

swap.cpp如下：写函数具体的实现逻辑

```cpp
#include "swap.h"// 需要包含swap头文件

// 函数的定义
void swap(int a, int b) {
  int temp = a;
  a = b;
  b = temp;

  cout << "a = " << a << endl;
  cout << "b = " << b << endl;
}
```

main.cpp如下：

```cpp
#include "swap.h"// 需要包含swap头文件

int main() {
  int a = 10;
  int b = 20;
  swap(a, b);
  return 0;
}
```

**运行：**

在终端输入`make`命令编译后可以直接执行：

output/main

./output/main

.\output/main

output\main

./output\main

.\output\main

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202410131356791.png)

当然也可以直接按F5运行

==但记住不要使用右键 Run Code 运行，好像是会识别不了头文件，会报错==