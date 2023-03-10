---
title: "从零开始搭建个人博客之使用hugo创建个人站点"
date: 2023-03-10T17:23:13+08:00
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
2.  通过hugo架构提供的语法将Markdown文件编译成html文件，html文件就是在浏览器中可以展示的文件
3.  把编译出来的html文件发布到服务器或者网站托管平台

我们可以通过上面的三步生成一个只属于自己的个人博客，是不是非常的简单呢，那么，跟着我一起来使用hugo搭建一个属于自己的个人博客吧。
## 具体流程
### 安装hugo
首先说明我是在Linux系统下安装Hugo
```
sudo apt install hugo
hugo version
```
![图片](/img/hugo%20version.png)
### 创建站点
```
hugo new site qichenxiaoni
cd qichenxiaoni
ls
```
![图片](/img/ls.png)

**简单的介绍一下目录中的部分文件**
**config.toml** 是配置文件，在里面可以定义博客地址，构建配置，标题，导航栏等等。
**themes**是主题目录，可以下载喜欢的主题，然后配置在**config.toml**里面。
**content**是博客文章的目录。
**static**是博客的静态资源，比如图片
**public**是编译后的目标文件的目录。
==注：需要特别注意的是当前目录是你的根目录，里面存放的是你的源文件，也就是markdown文件、模板文件以及配置文件和图片，但是最终显示在网页上的是目标文件，也就是markdown文件编译成的html文件，在这个html文件里面存在着你这个页面中使用的css和js代码。
### 设置主题
在我们的博客正式运行之前，我们需要给博客设置一个主题，官方主题网站中有很多的主题文件。可以[点击此链接跳转](https://themes.gohugo.io/)，在这里我使用的主题文件是[Even](https://github.com/olOwOlo/hugo-theme-even)
#### 安装主题
需要特别注意的是，这时候我们是要在根目录下运行这行指令，也是要在站点目录下运行，例如我的站点就是qichenxiaoni，因此我要在qichenxiaoni这个路径下执行指令
```
git clone https://github.com/olOwOlo/hugo-theme-even /themes/even
如果你没有安装git命令可以先去安装git
sudo apt install git
```
如果这时候你发现 git 的速度很慢或者是显示连接超时，可以在 git 前面加上一个 sudo，（此方法用于 Linux）如果还是不行，可以选择手动下载主题，然后复制到根目录下的 themes 目录中，不过可能会报‘WARN 2021/02/03 10:56:17 found no layout file for "HTML" for kind "home": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.’，这时候我们需要重新 git clone 一下主题。
#### 配置主题
当我们下载好主题之后，在主题的 exampleSite 目录下一般都会一个 config.toml 配置文件，打开这个文件将里面的内容都复制到你根目录下的 config.toml 文件中，然后根据里面的注释进行修改，下面是我做的简单修改，可以参考一下。
```
languageCode = "zh-cn"
defaultContentLanguage = "zh-cn"                             # en / zh-cn / ... (This field determines which i18n file to use)
title = "小陈的博客"

[author]                  # essential                     # 必需
  name = "Chen"

[sitemap]                 # essential                     # 必需
  changefreq = "weekly"
  priority = 0.5
  filename = "sitemap.xml"

[[menu.main]]             # config your menu              # 配置目录
  name = "主页"
  weight = 10
  identifier = "home"
  url = "/"
[[menu.main]]
  name = "归档"
  weight = 20
  identifier = "archives"
  url = "/post/"
==[[menu.main]]
  name = "标签"
  weight = 30==
  identifier = "tags"
  url = "/tags/"
[[menu.main]]
  name = "分类"
  weight = 40
  identifier = "categories"
  url = "/categories/"

logoTitle = "小陈的博客"        # default: the title value    # 默认值: 上面设置的title值
```
这样我们就完成了基本的配置，后续如果还需要更改，可以按照文件里面的注释进行修改。
#### 创建第一篇博客
发布博客之前需要我们有一篇博客，因此我们需要创建出一个博客页面
```
hugo new post/first-post.md
```
==当我们生成了新的 md 文件时，它会自动生成一些行，我们可以看到它的 draft 选项后面接的是 true，当它是 true 的时候是默认不渲染的，如果需要渲染的话，我们可以将 true 改成 false，这样就可以渲染了，但是我们在这里直接将 draft 选项注释掉，然后加上 markdown 语法就 OK 了。后续可以直接自己直接在编辑器里面。content/post/目录下创建 md 文件来写博客，比较方便。也可以在 post 里面创建多层目录，方便归类。==
~[图片](/qichenxiaoni/static/img/first-post.png)
**温馨提示**：
	如果我们里面要插入图片的话，我们需要在 static 目录下创建存放图片的文件夹，一般命名为 image 或 img。
#### 发布博客
```
hugo server
```
![图片](/img/hugo%20server.png)
如果我们执行 `hugo server` 没有报错的话，就说明发布成功，我们就可以在浏览器中输入 `localshot:1313` 来查看我们的第一篇博客啦。
![](/img/%E5%8F%91%E5%B8%83%E6%88%90%E5%8A%9F.png)
至此，个人博客就初步的搭建出来了，你也可以自己去尝试一下通过 config.toml 中的注释来修改一下主题的配置，例如将页面下面的图标改成自己需要的，不用担心需要重新启动 hugo，因为 hugo 是立即生效的。

下一篇具体讲述如何将hugo博客托管到github pages中，为什么要托管到github pages的原因。

<全文完>

> 本文参考教程：[码农在新加坡的个人博客](https://leftpocket.cn/post/hugo/hugo_creation/)