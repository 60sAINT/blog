---
title: PicGo + Github搭建个人免费图床
cover: assets/picGo.png
tags:
  - PicGo
  - GitHub
---

# 使用Github搭建图床

1. 新建仓库

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20240930170046.png)

   !![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301704990.png)

2. 点击右上角用户头像 => settings

3. 生成token令牌，往下拉，直到左侧到底，选择Developer settings

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20240930170517.png)

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301705330.png)

4. 密码验证

5. 可以给令牌(token)做个Note(标记)，然后选择令牌(token)截止时间。不建议选永久，因为不安全。基本是该图床用到多久就选多久。

   选择 repo 权限，然后拉到底部，选择创建就行了。

   ![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301706904.png)

   创建完毕之后，生成的Token是你的账户下的github服务器的令牌

# PicGo整合Github图床

PicGo是一款优秀的[图床工具](https://so.csdn.net/so/search?q=图床工具&spm=1001.2101.3001.7020)，能够自动把本地图片上传至网络，并转换成可访问的链接。

## 下载并安装PicGo

下载地址：https://github.com/Molunerfinn/picgo/releases

根据自己的操作系统（Win/Linux/Mac）来下载安装包

Mac系统选择后缀为.dmg的进行安装

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301708256.png)

> **Mac系统安装PicGo时打开报错：文件已损坏，您应该讲它移到废纸篓**
>
> 解决办法：
>
> - 打开终端输入以下内容，“为安装路径，默认是以下路径”
>
>   ```bash
>   sudo xattr -d com.apple.quarantine "/Applications/PicGo.app"
>   ```
>
> - 按照提示输入电脑锁屏密码回车即可

## 设置图床

图床设置 -> Github

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301711644.png)

## 整合jsDelivr

想要知道jsDelivr的作用，首先就需要了解CDN是什么

CDN的全称是Content Delivery Network，即内容分发网络。CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。

由于Github的资源在国内加载速度比较慢，因此需要使用CDN加速来优化网站打开速度，jsDelivr + Github便是免费且好用的CDN，非常适合博客网站使用。

进行图床配置：

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/20240924163148.png)

:::info

具体配置介绍

;;;id1 设定仓库名

用户名/仓库名

;;;



;;;id1 设定分支名

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301713979.png)

;;;



;;;id1 设定Token

上面刚刚在Github上获取的token

;;;



;;;id1 设定存储路径

需要放到仓库中的哪个文件夹下

- 如果直接放到仓库的根目录下就不需要填写这一栏
- 如果需要放到某个目录下，直接写目录名就行，不需要在目录名前加 / 

> 建议在路径后面统一都加个 ’ / '，否则PicGo会在test后再拼接上本地的文件名然后一起作为远程仓库存储图片的图片名
>
> eg：test/

- 当有多级目录时，也是直接写路径。

  eg：test/test1/test2/

- 当填写的目录不存在时，PicGo会自动帮你在Github上创建目录，这个不用担心！

;;;



;;;id1 设定自定义域名

此时就需要结合jsDelivr来加速了

打开jsDelivr官网，了解它的使用方法：https://www.jsdelivr.com

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301714667.png)

```http
# https://cdn.jsdelivr.net/gh/：固定的前缀，相当于替换掉了Github地址中的https://github.com/
# user：Github上的用户名
# repo：仓库名
# @version：版本号（这里我们可以不管）
# file：文件名（这里我们也不需要加上，因为上传完图片后，它会自动将上传的图片的名字作为存储的文件名）
https://cdn.jsdelivr.net/gh/user/repo@version/file
```

例如我的自定义域名就为：https://cdn.jsdelivr.net/gh/60sAINT/images@latest

> 这里值得注意的是，如果需要指定上传到哪个分支，此时需要在自定义域名后面使用@ + 分支名，如果是仓库默认的分支，可以省略指定分支这一步。
>
> eg：我需要上传到test分支上，此时自定义域名就变成了：https://cdn.jsdelivr.net/gh/60sAINT/images@latest
>

;;;

:::

## 测试

配置完成后，切换到刚刚配置好的图床，然后手动上传图片试试：可以点击’点击上传’，也可以通过拖拽的方式进行上传

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301716029.png)

然后，我们能够在相册中看到我们已经上传的图片，可以查看、复制已经上传的图片的URL，同时也可以将上传的图片删除。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301716169.png)

## 附录

可以在PicGo设置中开启 时间戳重命名 ，这样同时上传相同的图片就不会被覆盖了。

![](https://cdn.jsdelivr.net/gh/60sAINT/images@latest/202409301717945.png)

