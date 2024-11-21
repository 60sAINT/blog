---
title: PMD 基于缺陷模式的测试
date: 2023-04-14
cover: assets/pmd.png
categories:
- [软件测试与质量保证]
tags:
  - 软件测试
  - PMD
---

# 介绍

+   PMD——Java 程序代码缺陷的开源静态检查工具

# Eclipse 安装 PMD 插件

1. 在 Eclipse 的菜单中找到 “Help” --> “Eclipse MarketPlace...”

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413153813.png)

2. 打开之后，会看到如图所示界面，在 “Find” 的搜索框中输入 “PMD”，点击搜索

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413154023.png)

3. 会看到 pmd-eclipse-plugin 7.0.0，点击”Install“安装（这里我已经安装过了，所以显示”Installed“）

4. 弹出界面，点击”Confirm“

5. 跳转界面，选勾接受条款，点击”Finish“

6. 安装完成之后，提示重启 Eclipse，点击”Yes“

7. 查看 PMD。在菜单栏中”Window“--> ”Preferences“

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413155500.png)

8. 可查看到 PMD，则安装成功

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413155556.png)


# 使用 PMD 测试程序

1. 让 PMD view 在控制台中显示

   ”Windows“--> ”Show View“ --> ”Other“ --> “Violations Outline” 和 “Violations Overview” 各自 “Open” 一次

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413155914.png)

2. 完成之后，控制台如图所示

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413160438.png)

3. 右键点击项目文件，选择 “PMD” --> “Check Code”

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413161523.png)

4. 控制台即显示缺陷信息

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413161704.png)

5. 点击右上角 “最大化” 可全屏查看

   ![](https://raw.githubusercontent.com/60sAINT/images/main/20230413161844.png)


⚠️：代码没有语法错误测试才能成功
