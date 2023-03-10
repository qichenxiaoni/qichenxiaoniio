---
title: "从零开始搭建个人博客之hugo部署Vercel"
date: 2023-03-10T21:40:24+08:00
# draft: true
categories : [
    "hugo",
]
---

## 前言

[Hugo](https://gohugo.io/)是一款快速，现代化且高度可配置的静态网站生成器。它是一个基于命令行的工具，用于将Markdown等文本格式转换为静态网站。Hugo使用Go语言编写，因此它非常快。Hugo的主要优点是构建速度非常快，因为它是一个静态网站生成器，没有必要运行数据库或其他服务。

使用Hugo创建个人博客的主要优势在于它非常快速、易于配置，并且生成的网站可以直接部署到任何Web服务器上，而不需要运行数据库或其他服务。此外，Hugo提供了大量的主题和插件，可以轻松地自定义您的个人博客。它还支持各种文本格式，包括Markdown，AsciiDoc和Org-mode，以及多种语言，包括中文。

## 整体流程

1.  写Markdown文件；
2.  通过hugo架构提供的语法将Markdown文件编译成html文件，html文件就是在浏览器中可以展示的文件;
3.  把编译出来的html文件发布到服务器或者网站托管平台;

我们可以通过上面的三步生成一个只属于自己的个人博客，是不是非常的简单呢，那么，跟着我一起来使用hugo搭建一个属于自己的个人博客吧。

## 具体流程

在上一篇文章中，我们已经将hugo博客部署到Github Pages中了，这一篇博客中我们来将博客部署到Vercel平台，这时可能会有人感到疑惑了，为什么弄到了GItHub Pages中还要再弄到Vercel中，这是因为github pages在国内有时候会访问不上，并且如果你想要让百度这一类的搜索引擎将你的博客收录的话，必须再弄一个托管平台，因为Github禁止百度抓取网页，那么什么是Vercel，Vercel的优势又是什么呢。

[Vercel](https://vercel.com/)是一个创建高性能网站和应用程序的现代平台。它支持许多编程语言和框架，并提供了许多工具和服务来简化Web开发过程。Vercel的主要优势是快速部署和构建，自动缩放，安全性高，集成了很多有用的功能，如自动SSL和CDN。

使用Vercel可以快速部署您的Hugo博客并获得许多有用的功能。例如，它提供了自动SSL，自动缩放和全球CDN，可以让您的网站访问速度更快，并提供更好的安全性。此外，您还可以获得有用的分析和监控工具，以了解您的网站的性能和使用情况。并且Vercel还支持一键导入Github、GItlab，十分的方便。

### 部署到Vercel

#### 目标代码托管到Vercel

首先第一步，注册一个Vercel账号，这里我们直接使用github账号登录，因为这样方便导入仓库

第二步，新建一个项目

![图片](/img/img3/image1.png)

导入git仓库，可以选择导入所有仓库

![图片](/img/img3/image2.png)

选择目标文件仓库，点击Import

![图片](/img/img3/image3.png)

FRAMEWORK PRESET 选Other (因为目标文件是没有框架的)，查看无误之后点击Deploy

![图片](/img/img3/image4.png)

点击Deploy之后需要等待一下，因为它会自动检查有没有存在错误，如果没有存在错误，你就可以看到彩带烟花！

![图片](/img/img3/image5.png)

至此，我们就完成了将目标代码托管到Vercel的操作，当然我们也可以将源代码也托管给Vercel一份，操作也是十分的简单。

#### 源文件托管vercel

源码托管需要注意几点：

源代码需要发布到Github上。

public目录不能发布。（添加到 .gitignore上）

主题如果是从github下载下来的，子目录里面会有 `.git` 文件，发布代码的话，内层目录是不能有`.git`的，它会识别成另一个git仓库并忽略上传，只上传一个软链接文件。所以需要删除themes主题下面的`.git`文件，否则 Vercel 无法从 github上拉取到主题，编译会失败。

前面的步骤是一样的，同样的选择创建新项目，选择自己需要导入的项目仓库。

![图片](/img/img3/image6.png)

但是里面的设置存在差异，需要我们更改一些地方。

![图片](/img/img3/image7.png)

如果在构建当中出现问题，解决方法如下：

当我们看到这个的时候，

![图片](/img/img3/image8.png)

就说明我们的主题是没有发布到自己的Github中的，只是一个软链接，所以需要我们去themes目录，如果还是没有变动需要提交，重命名even提交，然后再改回来，就可以正常提交了。

```
到根目录（/Document/qichenxiaoni）
rm -rf .git
进入themes文件夹 
mv even even1 
git status 
git add * 
git commit -m "mv" 
git push 

mv even1 even 
git status 
git add * 
git commit -m "mv" git push
```

如果出现以下问题，是因为public文件已经存在，Vercel没有办法编译成目标文件，需要我们将public删除并忽略public文件提交到Github中。

![图片](/img/img3/image9.png)

```
mv public public2 
git add public public2 
git commit -m "mv" 
git push 

echo 'public'>>.gitignore 
mv public2 public 
git add public2 
git commit -m "gitignore" 
git push
```

如果出现`Error：Command "hugo" exited with 255`的话，需要我们将BUILD COMMAND将hugo改成hugo -D --gc

![图片](/img/img3/image10.png)

进入代码托管的页面，如果左边的小窗口显示正常，就说明发布成功，则点击Visit

![图片](/img/img3/image11.png)

如果地址栏显示如这样，就说明发布成功，同样，我们可以看到Vercel给我们自动分配了一个域名。

![图片](/img/img3/image12.png)

至此我们就完成将文件托管到Vercel的全部操作。

<本文完>

> 本文参考教程：[码农在新加坡的个人博客](https://www.leftpocket.cn/post/hugo/hugo_vercel/)
