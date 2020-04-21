---
title: 一个hugo+github pages静态博客是如何搭成的（二）
date: 2020-04-21T13:54:18+08:00
draft: true
tags: ["blog","hugo"]
categories: ["学习"]
series: ["博客搭建", "踩坑"]
---

# 一个hugo+github pages静态博客是如何搭成的（二）



## 遇到的第一个坑

按理说按照上面的步骤建好github pages之后直接访问域名就可以看到网站了，今天早上打开之后发现是这个样子的

![image-20200421093612212](https://masterenlu.github.io/blogSite/img/image-20200421093612212.png)

然后在我一顿操作之后突然就好了，至今不清楚到底是因为啥。

## 插入图片的问题

在写这篇文章的时候，插入了几个图片。由于我不想使用图床，直接把要插入的图片放在 `static` 文件夹里，然后按照官方文档给的格式使用相对路径插入图片。

> By default, the `static/` directory in the site project is used for all **static files** (e.g. stylesheets, JavaScript, images). The static files are served on the site root path (eg. if you have the file `static/image.png` you can access it using `http://{server-url}/image.png`, to include it in a document you can use `![Example image](/image.png) )`.

那么问题来了，使用这种相对路径插入的时候本地是识别不出来的，md文件在 `content\posts` 文件夹，图片在 `static` 文件夹，所以在本地查看的时候需要按照完整的相对路径查看，但是在发布到page上时又要更改过来，很麻烦。

而且使用官方的相对路径，要求**发布的网页必须在 `public` 文件夹里**，`docs` 文件夹不生效，原因未知。

最后还是选择了图床的模式，把仓库当作图床，采用 `url =  baseURL/img/image` 的模式

## 主题的问题

上面提到过需要更改根目录下的配置文件，但是在配置文件内引用的资源文件没有被更改。需要进入主题文件夹内的 `static` 文件夹内更改资源文件为自己想要的，比如更改头像。