---
title: "Notion创建个人博客之托管Vercel"
date: 2023-03-12T22:17:42+08:00
# draft: true
categories : [
    "hugo",
]
---

## 前言

当今个人博客盛行，人们都想要有一个属于自己的个人博客，但是很多人苦于没有编程基础或不想使用代码来制作一个个人博客，但好在现在有一种方式可以不用敲一行代码就可以制作出一个属于自己的个人博客，只需要有一个Notion账号，Github账号以及Vercel账号就可以实现了。那么废话就不多说了，开始本次的教程吧！

## 开发流程

### 克隆项目

首先我们需要有一个github账号，如果没有也可以先打开[github](https://www.github.com)页面再进行注册登录。

登陆之后，我们在左上角输入NotionNext搜索，也可以点击[此链接](https://github.com/tangly1024/NotionNext)直接跳转到NotionNext，然后点击右上角的Fork按钮，在下拉菜单中选择Create Fork，这样就会拷贝一份代码到你的github中。

![图片](/img/img6/image1.png)

![图片](/img/img6/image2.png)

这份代码就是我们将Notion搭建为个人博客网站的关键，这个`NotionNext`调用了Notion的API来展示Notion的页面，同时也提供了一些主题和特效来定制化我们自己的个人网站效果。

对于这份代码仓库，我们基本可以不用做什么改动，只需要对`blog.config.js`这份文件进行修改，因为这份文件是所有主题特效的详细配置文件。

### 托管Vercel平台

首先我们依旧需要一个Vercel账号，如果没有Vercel的账号，可以直接在浏览器中输入https://vercel.com/，也可以直接点击[Vercel](https://vercel.com/)，进行登录，温馨提示：建议使用github账号注册，这样方便我们从github仓库中导入`NotionNext`。

登录之后我们需要做的第一件事就是新建项目，点击`New Project`，

![图片](/img/img6/image3.png)

导入git仓库，可以全部导入，也可以指定导入的仓库。

![图片](/img/img6/image4.png)

在仓库列表中选择`NotionNext`并点击`Import`。

![图片](/img/img6/image5.png)

Import之后，我们基本上没有什么需要修改的了，只需要我们在`Environment variables`中添加一条记录即可。Name：`NOTION_PAGE_ID` Values：`34889e026fc44f7eb074c3847a052020`。

![图片](/img/img6/image6.png)

这里的值是我们从Notion里发布出去的链接中的中间部分，也就是这一部分

![图片](/img/img6/image7.png)

弄好之后点击Deploy进行部署，等待几分钟，就可以部署成功了

![图片](/img/img6/image8.png)

然后点击Vist，可能会出现访问不上的问题，但是没关系，我们可以申请域名来绑定个人网站，这样我们就可以访问了。

### 简易博客配置

进入github仓库找到我们刚刚克隆下来的`NotionNext`，进入仓库找到`blog.config.js`文件进行修改，特别注意的是，我们在GItHub上的修改是不需要到Vercel上再次部署的，Vercel会监听github上的修改，自动自行部署的。

![图片](/img/img6/image9.png)

点击blog.config.js文件

![图片](/img/img6/image10.png)

点击右上角的笔的图标，这样就可以进行修改了，主要看个人喜好，以下是我个人配置。

```
修改主题
THEME: process.env.NEXT_PUBLIC_THEME || 'hexo', // 主题， 支持 ['next','hexo',"fukasawa','medium','example'] @see https://preview.tangly1024.com
修改个人信息
  AUTHOR: process.env.NEXT_PUBLIC_AUTHOR || 'Chen', // 您的昵称 例如 tangly1024
  BIO: process.env.NEXT_PUBLIC_BIO || '一个迷茫的摆烂人', // 作者简介
  LINK: process.env.NEXT_PUBLIC_LINK || 'https://tangly1024.com', // 网站地址
  修改特效是否开启
    // 鼠标点击烟花特效
  FIREWORKS: process.env.NEXT_PUBLIC_FIREWORKS || true, // 开关
  // 烟花色彩，感谢 https://github.com/Vixcity 提交的色彩
  FIREWORKS_COLOR: ['255, 20, 97', '24, 255, 146', '90, 135, 255', '251, 243, 140'],

  // 樱花飘落特效
  SAKURA: process.env.NEXT_PUBLIC_SAKURA || true, // 开关
  是否开启音乐插件
    MUSIC_PLAYER: process.env.NEXT_PUBLIC_MUSIC_PLAYER || true, // 是否使用音乐播放插件
```

修改完之后滑到最下端，点击`Commit changes`，保存修改。

![图片](/img/img6/image11.png)

等待几分钟，让Vercel自动部署网站，之后我们可以访问网站查看效果。

![图片](/img/img6/image12.png)

我们可以发现修改已经生效了。

<本文完>

> 本文至此就结束了，如果还有不懂的可以看[码农在新加坡](https://www.leftpocket.cn/post/notion/vercel/)的教程。