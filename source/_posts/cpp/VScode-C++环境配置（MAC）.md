---
title: VScode C++环境配置（MAC）
cover: assets/cppConfig.png
categories:
- [C++]
tags:
  - C++
  - VSCode
  - 环境
---

# 安装 C/C++ 插件

点击**扩展**，然后在**商店[文本框](https://zhida.zhihu.com/search?content_id=236277832&content_type=Article&match_order=1&q=文本框&zhida_source=entity)中输入 C/C++**，再选择 C/C++ 插件，点击**安装**。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301737368.png)

# 配置运行环境

## 打开终端

在命令行中输入 **clang --version**，如果之前**没有安装 clang** 的话，就会出现下图的对话框，点击安装，如下图。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301741336.png)

安装完成之后，验证是否成功，在命令行中输入 clang --version，显示版本号，代表成功了。

## 编写测试代码

新建 test 文件夹，并在 vscode 中打开，添加test.cpp文件。输入一段可以运行的简易代码：

```cpp
#include <iostream>
using namespace std;

int main() {
  cout << "Come on" << endl;
  return 0;
}
```

## 配置编译器

按下 **command + shift + P** 调出面板，输入**C/C++**，选择**编辑配置(UI)**，如下图示。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301822632.png)

此时，项目根目录下会出现一个**.vscode 文件夹**

在 **C/C++ 配置**界面下的**编译器路径**，选择适合自己的，如果是 **C 语言**，则**选择 clang**，**C++ 则选择 clang++**，由于作者是用 C++，所以选择 clang++，如下图示。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301825520.png)

在当前界面下，将 **IntelliSense 模式**，设置成 **clang-x64(legacy)**，将 **C** 标准设置为 **c17，C++** 标准设置为**c++17**，如下图示。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301826786.png)

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301836659.png)

此时，.vscode 文件夹 **c_cpp_properties.json** 文件内容如下：

```json
{
  "configurations": [
    {
      "name": "Mac",
      "includePath": ["${workspaceFolder}/**"],
      "defines": [],
      "macFrameworkPath": [
        "/Library/Developer/CommandLineTools/SDKs/MacOSX14.5.sdk/System/Library/Frameworks"
      ],
      "compilerPath": "/usr/bin/clang++",
      "cStandard": "c17",
      "cppStandard": "c++17",
      "intelliSenseMode": "clang-x64"
    }
  ],
  "version": 4
}
```

## 配置构建任务

1. 打开命令面板（`Command + Shift + P`），输入 `Tasks: Configure Task`，然后选择 `Create tasks.json file from template`。

2. 选择 `Others` 选项。

3. 在生成的 `tasks.json` 文件中，添加以下内容：

   ```json
   {
     "version": "2.0.0",
     "tasks": [
       {
         "type": "cppbuild",
         "label": "C/C++: clang++ 生成活动文件",
         "command": "/usr/bin/clang++",
         "args": [
           "-fcolor-diagnostics",
           "-fansi-escape-codes",
           "-g",
           "${file}",
           "-o",
           "${fileDirname}/${fileBasenameNoExtension}"
         ],
         "options": {
           "cwd": "${fileDirname}"
         },
         "problemMatcher": ["$gcc"],
         "group": {
           "kind": "build",
           "isDefault": true
         },
         "detail": "编译器: /usr/bin/clang++"
       }
     ]
   }
   ```

4. 再次打开命令面板（`Command + Shift + P`），输入 `Tasks: Configure Default Build Task`，应该能看到刚刚添加的 C/C++ 任务

## 配置调试设置

1. 打开命令面板，输入 `Debug: Open launch.json`。

2. 如果没有看到相关选项，可以选择 `C++ (GDB/LLDB)`。

3. 将生成的 `launch.json` 文件修改为如下内容（根据您的文件名和路径进行调整）：

   ```json
   {
     "version": "0.2.0",
     "configurations": [
       {
         "type": "lldb",
         "request": "launch",
         "name": "C++ debug",
         "program": "${fileDirname}/${fileBasenameNoExtension}",
         "args": [],
         "cwd": "${workspaceFolder}",
         "preLaunchTask": "C/C++: g++ 生成活动文件"
       }
     ]
   }
   ```

4. 现在，应该能够在 `test.cpp` 文件中按 `Command + Shift + P`，输入 `debug`，并看到 `C/C++: clang++ 生成和调试活动文件` 选项

# 验证可行性

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409302039066.png)

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409302040652.png)

- **`test` 文件**：这是编译的 C++ 程序的可执行文件。
- **`test.dSYM` 文件**：这是 macOS 系统下用于调试的符号文件，包含调试信息，帮助调试器更好地理解程序的结构和状态。

至此，C++环境已经全部配置完成

