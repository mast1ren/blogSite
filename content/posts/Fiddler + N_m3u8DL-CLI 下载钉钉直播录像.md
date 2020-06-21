---
title: "Fiddler + N_m3u8DL-CLI 下载钉钉直播录像"
date: 2020-06-21T14:53:17+08:00
draft: false
tags: ["Fiddler","m3u8","钉钉","抓包"]
categories: ["正经事"]
series: ["踩坑"]
description: "绕过权限下载钉钉直播录像"
---

## 前言

最近在准备最优化的考试，需要反复看钉钉上的直播录像，但是钉钉自带的播放器功能实在太拉胯了，老师又不给开放下载权限，只能采取这种抓包的方式来下载。

## 准备工具

### Fiddler

Fiddler 是一款常用的抓包工具：[官网](https://www.telerik.com/fiddler)

安装 Fiddler 之后，需要在 `Tool/Options/HTTPS` 中勾选 `Capture HTTPS CONNECTs`、`Decrypt HTTPS traffic` 和 `Ignore server certificate errors (unsafe)` 三个选项，中间会弹出窗口安装证书，同意就行。

点击 `Find` 搜索 `.m3u8` 右键选取高亮条目，选择 `Copy` 下的 `Just Url` 复制流媒体源。

> 需要注意的是，在第一次查找过后，如果还需进行下一个流媒体的查找，需要点击 `Replay` 和 `Go` 中间的选项选择 `Remove all` 来清楚记录，否则查找到的还会是第一次查找的内容。

### N_m3u8DL-CLI

N_m3u8DL-CLI 是开源在 Github 上的一款 m3u8 下载器：[开源地址](https://github.com/nilaoda/N_m3u8DL-CLI)

压缩包中包含两个可运行文件：
`N_m3u3DL-CLI_version.exe` 和 `N-m3u8DL-CLI-SimpleG.exe`

其中第一个是下载需要的主要程序，第二个是一个图形化的配置界面。

如果图一个省事，直接在第一个程序中粘贴已经抓到的流媒体源就可以下载，默认下载地址是程序目录下的 `Downloads` 文件夹中。

如果需要进行细化的配置，可以在第一个程序中使用命令行选项或者直接在第二个程序中进行。

在第二个程序中将流地址粘贴在 `M3U8地址` 一项中点击 `GO(S)` 就可以开始下载到指定的文件夹中。

默认的下载格式应该是 `mp4` 文件。

