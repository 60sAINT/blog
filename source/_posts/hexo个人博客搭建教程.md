---
title: Hexo个人博客搭建教程
cover: assets/hexo.png
tags:
  - Hexo
  - GitHub

---

# 准备工具

1. 安装Node.js

   ```bash
   brew install node
   ```

2. 安装Hexo

   一旦下载了Node.js，就能用npm（Node包管理器）全局安装Hexo：

   ```bash
   npm install -g hexo-cli
   ```

3. 验证Hexo是否安装成功

   ```bash
   hexo -v
   ```

   如果安装成功，则显示一系列版本号

# 生成ssh Keys

Git bash输入

```bash
ssh-keygen -t rsa -C "邮件地址"
```

1. 敲4次Enter⌨️

2. 访达 -> shift+command+G -> 输入~/.ssh

   即：访达 -> 前往 -> 前往文件夹 -> 输入~/.ssh

3. 打开里面的id_rsa.pub，全选复制代码

4. 打开github

5. 进入用户设置，找到SSH and GPG keys

6. 新建一个New SSH key，标题任意，粘贴刚刚复制的代码，点击Add SSH key创建

7. 打开git bash验证是否已经成功添加

   ```bash
   ssh -T git@github.com
   ```

   输入yes

   看到`Hi 60sAINT! You've successfully authenticated, but GitHub does not provide shell access.`则成功

# 本地部署

1. 选择一个合适的位置创建一个文件夹，用于放置博客文件。control + 单击文件夹 -> “新建位于文件夹位置的终端窗口”
2. 输入`hexo init`进行初始化。如果不行就在前面加npx -> `npx hexo init`
3. `hexo install`安装
4. `hexo g`生成
5. `hexo s`本地部署，得到一个链接，打开就可以看到博客已经成功在本地部署了

> 下一步就可以选择喜欢的主题在本地继续配置了
>
> 我的爱用：
>
> https://shoka.lostyu.me/computer-science/note/theme-shoka-doc/
>
> https://docs.kaitaku.xyz/guide/config.html#algolia-%E6%90%9C%E7%B4%A2

# 上线博客

1. 打开博客文件夹中的config文件，配置下列代码：

   ```yml
   deploy:
     type: git
     repository: https://github.com/60sAINT/60sAINT.github.io.git
     branch: main
   ```

2. git bash -> `hexo algolia` 上传索引

3. `hexo g`生成

4. 安装Hexo Git部署插件：

   ```bash
   npm install hexo-deployer-git --save
   ```

5. `hexo d`上传

   > 第一次上传要进行身份验证
   >
   > GitHub 已经不再支持使用密码进行身份验证，它要求用户使用更安全的身份验证方式。
   >
   > 1. ==使用 SSH 密钥==
   >
   >    1. **生成 SSH 密钥**（如果您还没有的话）：
   >       打开终端并输入以下命令：
   >
   >       ```bash
   >       ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   >       ```
   >
   >       按照提示操作，默认情况下会在 `~/.ssh/id_rsa` 生成密钥。
   >
   >    2. **将公钥添加到 GitHub**：
   >
   >       - 复制公钥：
   >
   >         ```bash
   >         cat ~/.ssh/id_rsa.pub
   >         ```
   >
   >       - 登录到 GitHub，进入 **Settings** -> **SSH and GPG keys** -> **New SSH key**，将公钥粘贴进去。
   >
   >    3. **更改 Hexo 配置**：
   >       在您的 Hexo 项目的 `_config.yml` 中，将 GitHub 的 URL 更改为 SSH 格式：
   >
   >       ```yml
   >       deploy:
   >         type: git
   >         repo: git@github.com:60sAINT/60sAINT.github.io.git
   >         branch: main
   >       ```
   >
   > 2. ==使用 Personal Access Token==
   >
   >    如果更愿意使用 HTTPS，可以使用 GitHub 的 Personal Access Token 代替密码：
   >
   >    1. **在 Hexo 中使用 Token**：
   >       在您执行 `hexo d` 时，输入用户名为 GitHub 用户名，密码为刚刚生成的 token。
   >    2. **在 Hexo 中使用 Token**：
   >       在您执行 `hexo d` 时，输入用户名为 GitHub 用户名，密码为刚刚生成的 token。
