---
date: "2022-03-30T21:49:57-07:00"
title: 如何使用R blogdown快速搭建个人网站
---

# R Blogdown 搭建个人网站详细教程

[Blogdown](https://bookdown.org/yihui/blogdown/)是Yihui Xie开发的一款R包，便于配合R markdown开发网站，可直接显示R markdown的运行结果，适用于R programmer。

如果你是一个编程小白，花20 mins阅读一下markdown语法，
跟着这套教程，你也可以轻松搭建自己的网址。

使用Blogdown进行网站搭建的过程有三步:

- [x] Blogdown搭建本地网页
- [x] Github托管项目
- [x] 使用netlify部署网站

以下步骤可以帮助你快速搭建网站，详细细节后续可以自己摸索。

## 1. Blogdown搭建本地网页

### 1.1 安装

首先，R console运行以下语句安装blogdown包。

```r
install.packages("blogdown")
```

然后，创建一个blogdown project。

操作路径： 点击 File - New Project - New Directory - Website using blowdown - 填写文件夹名称，存放路径，选择主题 - Create Project

<img src="/img/image-20220331192638723.png" alt="image-20220331192638723" style="zoom: 50%;" align="center" />

<img src="/img/image-20220331192832390.png" alt="image-20220331192832390" style="zoom: 50%;" align="center"/>

点击创建项目后，R会从github下载默认主题Hugo-lithium，如果从github下载的速度过慢需要挂vpn。

<img src="/img/image-20220331193821484.png" alt="image-20220331193821484" style="zoom:50%;" />

然后运行：

```r
blogdown::serve_site()
```

Viewer中显示出网页内容则说明安装顺利，本地的网页demo已出~可以具体查看Files中的文件内容进行对应的修改。

### 1.2 报错处理

若运行`serve_site()`失败，尝试使用

```r
remotes::install_github('rstudio/blogdown')
```

但我在这里依然运行失败了，报错信息:

`blogdown::serve_site() Error in servr::server_config(..., baseurl = baseurl, hosturl = function(host) { :    unused argument (hosturl = function(host) {     if (g == "hugo" && host == "127.0.0.1") "localhost" else host`

报错原因是[servr文件没有被自动安装成功](https://stackoverflow.com/questions/65245723/why-is-my-served-site-not-rendering-with-this-error)，运行

```r
remotes::install_github('yihui/servr')
```

然后重启R，创建blogdown项目后运行即可成功。

## 1.2 内容修改

### 文本内容

如果你了解标记语言[markdown](https://www.markdownguide.org/cheat-sheet/)/rmarkdown，便很容易在项目文件中找到对应的文件进行更改。

以blogdown的默认主题hugo-lithnium为例，其中一篇推送内容在
`content/post/2016-12-30-hello-markdown/index.md`中。修改其中的文字内容即可。

在编写自己的网页内容时，需要插入图片，我在这里遇到了一些坑，这里[解决图片无法显示的问题](https://github.com/rstudio/blogdown/issues/45)。Yihui推荐将图片存放在`static`文件夹中, 按hugo的惯例，我们在`static`文件夹下新建一个`img`文件夹，将图片放入`img`文件夹中，使用语句`<img src="/img/image1.png"  style="zoom:25%;" />`嵌入图片，
注意不要遗漏第一个`/`。


### 个性化

暂时先不对已有主题做太大的改动，但你一定想给自己的网页增加一点个性化的内容，最重要的就是网页的logo啦!

网页的布局在`config.yaml`文件中可以修改，其中logo信息在:
```r
  logo:
    alt: Logo
    height: 50
    url: 个人头像.jpeg
    width: 50
```
中，如果你不清楚在什么路径替换图片文件，可以电脑搜索该默认主题的logo文件路径，然后再该路径下替换你的logo。

## 2: Github托管项目

第二步，我们需要把整个项目的文件夹放到Github上。

如果你不熟悉Github，你可以下载一个[桌面版Github](https://desktop.github.com/)，然后进行下面的操作。

1. 注册并登陆Github
2. 新建一个repository

<img src="/img/github1.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

设置仓库名`Repository name`，勾选`Public`，并创建仓库`Create repository`。

<img src="/img/github2.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

然后，复制你的仓库链接，如`https://github.com/Tatiana9707/test111.git`到
github桌面版 - File - Clone Repository 

<img src="/img/github3.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

<img src="/img/github4.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

`Local path`会根据你的url链接自动生成，不需要管。点击`Clone`。


然后，很简单的，将你blogdown文件夹下的文件复制到本地的Local path ，如`/Users/xiayiwang/Documents/GitHub/test111`文件夹下。

<img src="/img/github5.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

随后，Github桌面版的最左边的Changes栏会识别到该local repository的变化(新增了文件)，填写`Summary(required)`如创建项目，点击`Commit to main`，稍等片刻本地文件就会上传你的github上啦。


去网页版github看一下情况，

<img src="/img/github6.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

上传成功~

## 3：Netify部署

到达[Netify官网](https://www.netlify.com/)，使用你的github账号即可轻松注册。然后我们要做将github项目部署到netlify的操作，这相当简单。

<img src="/img/netlify1.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

点击从已有项目新增一个网址，使用github授权，找到你托管刚才blogdown项目的库，点击`Deploy site`，一些参数使用默认的即可。

<img src="/img/netlify2.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

<img src="/img/netlify2.jpg" alt="image-20220331192832390" style="zoom: 75%;" />

这里显示出了你的网页地址~ 点击`Site settings`可以更改你的域名。

至此完工~



