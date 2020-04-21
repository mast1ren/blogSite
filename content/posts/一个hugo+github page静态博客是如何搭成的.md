---
title: 一个hugo+github pages静态博客是如何搭成的
date: 2020-04-20T18:25:03+08:00
draft: false
tags: ["blog","hugo"]
categories: ["学习"]
series: ["博客搭建", "踩坑"]
---

# 一个hugo+github pages静态博客是如何搭成的

## 写在前面

------

**注意！这篇文章并不是一个教程，只是把我在搭建博客的过程和中间遇到的问题记录一下。**

------

其实之前也搭过一个静态博客，搭了两次，使用hexo框架。本来是想在博客上写点题解，记录一下自己在刷题和学习过程中遇到的坑和一些生活上面的感想，于是有了Hello-World-again这篇文章~~并不算~~。后来18年寒假进了技术部之后没了压力和方向，在加上本身懒到一种极致~~没有课绝对不会出宿舍门~~，刷题啥的也就慢慢落了下来，后来一直到现在没写过一道题，在原来博客里更新的几篇题解也全都转到了CSDN上，之后就没再管过博客的事情。

直到昨天，想用github上的一个项目开发一个东西，想了想用qt写了个图形界面，然后在实现功能过程中疯狂吃瘪，甚至一些之前踩过的坑都全忘了，又踩了一遍。于是我痛定思痛，决定以后及时更新博客，把自己踩过的坑全记下来防止再踩一遍。

于是我想起了被遗忘的博客，然后测试了一下还能不能用，然后就有了下面的情况

![image-20200420185908446](https://masterenlu.github.io/blogSite/img/image-20200420185908446.png)

进博客看了看自己仅有的两篇博~~shui~~客~~wen~~，决定从头开始搭一个新的博客，顺便把这篇作为新博客的开端，这才有了现在这篇文章。

## 关于hugo

为什么不再使用hexo作为新博客的框架了呢？一是因为hugo使用的go语言以后可能会学，现在整一下环境完事。第二个就是感觉我跟hexo水土不服（并不），这次换一个试试。

## hugo与go的安装

hugo按照[官方文档](https://gohugo.io/getting-started/installing)走就可以。

这里我看到最后才发现可以不下载chocolatey工具，直接下载hugo开源的发布版，解压缩后把添加<code>hugo/bin</code>到<code>path</code>环境变量就行了。

go的安装直接按照[官方文档](https://golang.org/doc/install)走，下载一个安装包一步到位。

安装完成之后如果要验证的话需要重启命令行，使用 `hugo help` 验证hugo的安装，`go` 来验证go的安装

## 选择主题并生成网页

然后创建要部署博客的文件夹，`hugo new site floderName` 就可以看到新部署的文件夹。

然后在[官网](https://themes.gohugo.io)上选择一个主题下载下来，解压缩到 `theme` 文件夹内，然后把目录里的配置文件复制到博客根目录。

这里主题配置文件可以是 `toml、yaml` 和 `yml` 三种格式

如果是 `toml` 格式可以直接复制内容到根目录的 `config.toml` 文件中，否则需要复制原配置文件里的内容到主题的配置文件，然后把原配置文件删除。注意配置内容格式，我选择的主题使用的是 `yml` 文件。

在新文件夹内执行 `hugo new posts/bulabula.md` 就可以在文件夹内的 `content/posts` 里看见生成的md文件。

执行 <code>hugo -D</code> 来部署静态网页，之后 <code>hugo server</code> 可以打开本地服务器预览效果。

![image-20200420221834299](https://masterenlu.github.io/blogSite/img/image-20200420221834299.png)

## 利用github仓库和github pages

首先需要在github上新建一个没有 `readme` 和 `lisence` 的空仓库，把博客的文件夹初始化并 `push` 到仓库中

这个时候按照[官方文档](https://gohugo.io/hosting-and-deployment/hosting-on-github/)来走，官方给了三个选择：

> [Deployment of Project Pages from `/docs` folder on `master` branch](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-of-project-pages-from-docs-folder-on-master-branch)
>
> [Deployment of Project Pages From Your `gh-pages` branch](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-of-project-pages-from-your-gh-pages-branch)
>
> [Deployment of Project Pages from Your `master` Branch](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-of-project-pages-from-your-master-branch)

第一个将在 `master` 分支下的 `/docs` 子文件夹部署网站，第二个是新建 `gh-pages` 分支来部署网站，第三个就是使用新仓库来部署网站。

第二种和第三种比较接近，当提交更改时需要提交两次，相比之下第一种只需要提交一次就可以。这里按照顺序选择了第一种方法~~并不是懒~~。

第一种方法需要更改 `hugo` 渲染网页的目录，默认是 `public` 目录，需要我们手动将 `config` 中的发布目录更改成 `docs`

```yml
publishDir: docs
```

* 如果是 `toml` 文件格式为

```toml
publishDir = "docs"
```

更改完文件之后重新在本地部署一次，确认有 `docs` 目录了之后直接一波git三连

```bash
$ git add .
$ git commit -m ""
$ git push -u origin master
```

完了之后打开仓库中的 `Settings` 往下找到 `Github Pages`，把 `Source` 一项改成 `master branch /docs floder`

![image-20200420225945814](https://masterenlu.github.io/blogSite/img/image-20200420225945814.png)

那么 `public` 文件夹实际上就不再需要了，为了避免麻烦，直接在 `.gitignore` 里加上 `\public` 以在 `push` 的时候不再更新 `public` 文件夹。

接下来要等一段时间，今天就先到这了，溜了，都11丶了，该摸了。