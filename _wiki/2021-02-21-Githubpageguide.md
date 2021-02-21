---
layout: wiki
title: 基于 Jekyll 搭建 GithubPage
categories: [cate1, cate2]
description: 搭建 Githubpage
keywords: keyword1, keyword2
---

> 基于 jekyll 与 Githubpage 搭建博客网站

### 准备工作

#### 基本前提条件：

1. 注册/拥有 Github 账号
3. 基于 git 操作比较复杂，推荐使用 GithubDesktop <a href="https://desktop.github.com/">前往下载</a>

#### 环境准备：

1. 安装 Ruby 环境。<a href="https://rubyinstaller.org/">前往下载</a> （苹果机使用 Homebrew 安装）

2. Ruby 安装完成后，找到其 bin 目录下的 Ruby.exe 双击，在打开的命令窗口中输入`gem install jekyll bundler`

   等待一段时间。

____

### 开始搭建

#### 在 Github 中创建一个仓库

仓库名最好为：xxx.github.io 的格式，这样，创建完成后，会自动生成基本的 Githubpage。

#### 将远程仓库 Clone 到本地：(使用 GithubDesktop)

打开 GithubDesktop，然后按照指引登录之后，

<img src="https://i.loli.net/2021/02/21/R6BLaW9HOQgXPSD.png" alt="GithubDesktop" style="zoom: 80%;" />

点击 Clone repository。然后选择刚刚创建的后缀为 .github.io 的仓库 clone 到本地。

#### 寻找一个 Jekyll 模板

到这里，其实已经结束了，网站地址即为 **xxx.github.io**，接下来只需要在本地仓库写好 web 文件，在上传即可。but 如我等小白的话，可以在网上找一个模板，然后稍加修改上传。

推荐网址:

>http://jekyllthemes.org/
>https://jekyllthemes.io/
>http://themes.jekyllrc.org/

当然，也可以自行在其他网站查找下载。

#### 生成网站雏形

下载模板解压，将整体复制到本地仓库，然后上传即可（同类型文件替换掉）。
<img src="https://i.loli.net/2021/02/21/ywlbVFrPJBDvORK.png" alt="GithubDesktop1" style="zoom:50%;" />

> 复制到本地仓库后，点击 Commit，然后 Fetch 变为 Push，点击 Push 等待上传即可。

____

#### 修改模板

认识重要文件:

> CNAME : 网站解析
> _config.yml：网站主题配置文件。

将模板中的个人信息，修改为自己的就好了。

#### 其他

* 自定义域名

  找到 Github Page 仓库，点击设置，然后找到下图部分
  <img src="https://i.loli.net/2021/02/21/9twmDsVi4a3c1H5.png" alt="Github" style="zoom:50%;" />

在 Custom domain 一栏填写已购买的域名，然后点击 save。再在域名解析中加入相关条目即可（这里不赘述）。

* 启用 Https

  接上图，勾选 Enforce Htpps 即可。等候一段时间。

### 注意事项

限于 Jekyll 模板，文章格式如下：

文件名
`年-月-日-标题.markdown`

文章开头含有以下信息

`---`
`layout: post`
`title: Blog`
`---`

> 可以添加其他信息：如分类等等。

____

### 参考资料

> 1. <a href="https://www.zhihu.com/question/20962496">少数派-知乎-如何在 Github 上写博客</a>

最后，本文是在我搭建博客之后写的，emm 遂无法验证此篇文章，难免有疏漏。。但流程确实大致如上。如有问题，还请评论区留言。

-QAQ-