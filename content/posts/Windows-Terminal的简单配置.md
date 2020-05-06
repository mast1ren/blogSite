---
title: "Windows Terminal的简单配置"
date: 2020-04-22T09:42:49+08:00
draft: true
tags: ["Windows-Terminal","注册表","Windows","terminal"]
categories: ["找乐子"]
series: ["踩坑","工具配置"]
description: "Windows Terminal的安装和右键菜单配置"
---

## 前言

由于电脑里的命令行有丶多~~其实就三个~~，就想着有没有一个工具能够集合一下，正好巨硬的 Windows Terminal 现在正在预览阶段，所以就直接用上了。

~其实就是闲着没事干，找点乐子~

## 关于Windows Terminal

Windows Terminal是微软官方开发并开源一个终端应用

开源地址：https://github.com/microsoft/terminal

> Windows Terminal is a new, modern, feature-rich, productive terminal application for command-line users. It includes many of the features most frequently requested by the Windows command-line community including support for tabs, rich text, globalization, configurability, theming & styling, and more.
> The Terminal will also need to meet our goals and measures to ensure it remains fast and efficient, and doesn't consume vast amounts of memory or power.

简单来说就是一个更现代的多功能命令行工具，包含多种命令行功能，同时支持选项卡、主题等个性化配置。

## Windows Terminal的安装

官方给了三种安装方式

1. 使用 Microsoft Store 搜索 Windows Terminal 并安装，这种方法可以及时获取最新版本的 Windows Terminal，这也是巨硬推荐的一种安装方式
  
   > ⚠ 注意：这种方法需要你的 Windows 系统在 Win10 1903 (build 18362) 以上才能使用
   
2. 通过 Github 安装，前面说过 Windows Terminal 是一个开源工具，在 Github 上可以找到开源仓库。
   如果无法在 MS 商店里安装，可以在开源仓库的 [Releases page](https://github.com/microsoft/terminal/releases) 下载并手动安装。
   
   >⚠ 注意：使用这种方式需要确保已经安装 [C++ Runtime v14 framework package for Desktop Bridge](https://www.microsoft.com/en-us/download/details.aspx?id=53175)
   
3. 通过 Chocolatey 工具安装
   ```
   choco install microsoft-windows-terminal
   ```
   以安装WT
   ```
   choco upgrade microsoft-windows-terminal
   ```
   以更新WT

当然还可以下载源码，自己编译，不过有一说一，能自己编译安装的也不会看这个
滑稽叹气.jpg

## 简单配置

安装完成之后打开WT，可以看到主界面

![image-20200422103049084](https://masterenlu.github.io/blogSite/img/image-20200422103049084.png)

~~WOW, amazing~~

点击 ➕ 可以打开新的页面，或者可以选择 ⌵ 选择打开其他类型终端。这里点击 ⌵ 可以看到有一个 `Settings` 选项，这个就是WT的用户配置选项，可以使用 `Ctrl + ,` 快捷键来快速打开。另外可以使用 `Alt +` 点击 `Settings` 来打开默认的配置文件 `default.json`，一般来说 `default.json` 文件是不可以更改的，需要更改的话可以参照 `default.json` 里的选项来在 `profiles.json` 配置。

在 `profiles.json` 文件的 `profiles: list:` 就是WT可以打开的 shell 或命令行
`guid` 用来标识要打开的 shell 或命令行，可以在 `powershell` 里使用 `[Guid]::NewGuid()` 命令来生成新的 `guid`
`name` 是shell或命令行的名称，可以随便取
`commandline` 是要执行的命令
`hidden` 项可以选择关闭某个选项，默认为 `false`

再回到头部，可以看到一个 `defaultProfile`，这一项就是选择打开 WT 时的默认窗口，通过选择相应窗口的 `guid` 来更改。

在 `profiles: defaults:` 里可以更改一些默认属性，比如

```json
"fontFace": "字体"
"cursorShape": "光标类型"
"useAcrylic": true/false //使用亚克力背景
"acrylicOpacity": //透明度
"startingDirectory": "." //默认打开路径
```

除此之外还可以在 `schemes` 里设置主题，在 `keybindings` 里设置快捷键绑定，具体可以参照[官方文档](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)

## 增加git-bash窗口

由于 `cmd` 和 `powershell` 已经自带，我要做的只有增加一个 `git-bash` 窗口~~😏️~~

按照上面的，只需要在 `profiles: list:` 增加一个新的选项，使用 `powershell` 生成一个新的 `guid`，并且在 `commandline` 选项填上 `git-bash` 的路径，如果想要添加图标的话，可以在下面新增一个 `icon` 选项并选择图片

完成之后保存文件就可以实时更新。

## 更改注册表实现右键WT菜单

官方并没有给出一个教程

> ##### [Add a "Open Windows Terminal Here" to File Explorer](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md#add-a-open-windows-terminal-here-to-file-explorer)
> Not currently supported "out of the box". See issue [#1060](https://github.com/microsoft/terminal/issues/1060)

但是我们可以通过更改注册表来实现这个右键菜单功能

> 2020 5 6
> 现在可以在 Github 上看一看 kerol2r20 大佬的  [Windows-terminal-context-menu](https://github.com/kerol2r20/Windows-terminal-context-menu) 项目

### 用户模式

首先打开注册表编辑器，找到路径 `计算机\HKEY_CLASSES_ROOT\Directory\Background\shell`，新建一个 `wt` 项，更改默认值数据为 `Windows Terminal here` 或者其他想在右键菜单里显示的内容，然后新建一个 `command` 项，将其默认值数据更改为 `C:\Users\your_username\AppData\Local\Microsoft\WindowsApps\wt.exe`

> ⚠ 注意：使用debug版本编译出来的应用程序是 `wtd.exe`

这个时候右键菜单里就出现了 `Windows Terminal here` 选项，如果想要按住 `shift` 点击右键出现的话需要在 `wt` 里新建一个**字符串值**，将其命名为 `Extended`。如果想要在选项旁边增加一个图标，需要在 `wt` 里新建一个可扩充字符串值 `Icon` ，将数据为 `你想要的图标的路径`

### 管理员模式选项

还是在 `计算机\HKEY_CLASSES_ROOT\Directory\Background\shell`，新建一个 `runas` 项，更改默认值数据为 `Windows Terminal here（管理员）` 或者其他想显示的内容，其余操作跟普通用户一样。

使用 `Windows Terminal here（管理员）` 选项打开的 WT 将会获得管理员权限。

### 打开路径

如果发现点击 `Windows Terminal here` 之后终端的路径依然是用户路径，就需要更改配置文件

上面提到过 `startingDirectory` 这个配置项，这个就是打开的路径

我们将其配置成

```json
"startingDirectory": "."
```

保存即时生效。

#### 新的思路

在新的[官方文档](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingCommandlineArguments.md#open-windows-terminal-in-the-current-directory)中出现了这样一项

> ##### Open Windows Terminal in the current directory
> ```
> wt -d .
> ```
> This will launch a new Windows Terminal window in the current working directory. It will use your default profile, but instead of using the `startingDirectory` property from that it will use the current path. This is especially useful for launching the Windows Terminal in a directory you currently have open in an `explorer.exe` window.

简单来说就是可以使用 wt 的命令行来实现打开 terminal 并且使用当前路径，我们可以在上面更改的注册表的 `command` 的默认值的数据更改为 `C:\Users\your_username\AppData\Local\Microsoft\WindowsApps\wt.exe -d .` 来实现这个功能。

而且按照官方文档的说法来看，`-d` 命令的优先权是大于 `startingDirectory` 的，可能是因为在启动的时候先按照配置文件运行程序，再执行命令的原因。也许可以通过这一点来找点乐子。

### 隐藏 `powershell here` 或 `cmd here`

在拥有了 `Windows Terminal here` 之后，`powershell here` 或 `cmd here` 就显得很多余，可以通过一些方法来去掉，依然是通过更改注册表。

依然是在 `计算机\HKEY_CLASSES_ROOT\Directory\Background\shell` 路径中，更改 `cmd` 或 `Powershell` 的 `ShowBasedOnVelocityId` 值，将其**重命名**为 `HideBasedOnVelocityId`

如果重命名失败，说明权限不够，需要更改为管理员用户或者提升用户管理员权限，具体不再多说。

## 后记

这篇文章其实早该写的，最早在19年就使用过 Windows Terminal 这个工具，不过后来重装系统之后就没下回来。

直到今年清明节，因为禁娱的原因，难得的能够闲下来，就想着找些事情做，然后就在论坛上（也许）看到了有过 WT 的帖子。安装配置花了一上午，还没来得及写些东西~~其实是根本没想写~~，就被高中同学叫出去踏青。回来之后也没再管过。

正好前两天把博客重新搭了起来，就想着有空把这个坑给填上，今天终于在水课期间把这篇文章写完，算是拔了一个 flag。