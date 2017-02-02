---
title: Github Pages与Hexo框架博客的搭建
date: {{ date }}
update: {{ updated }}
comments: true
categories:
  - archives
  - technology
tags:
  - Github Pages
  - Hexo
---

这篇博文的目的不是[Github Pages](https://pages.github.com/)与[Hexo框架](https://hexo.io/zh-cn/docs/index.html)框架搭建个人博客的教学，毕竟官网教学更详细更全面，而是记录本人搭建过程中了解、学习并汇总的一些东西，附带链接陈列出来，主要供第一次搭建的朋友参考学习。
<!-- more -->

## 知识点 ##

### git与github ###
**git**:查看廖雪峰的[git简介](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)。
git工具是Linus继linux开源操作系统之后又一影响世界的项目，因与linux的关系，所以本身是命令行模式，对大多数人来说是不够友好的，在实际使用中可以使用windows或iOS平台上的桌面软件，虽然这样并不能使用git的全部功能。[Github](https://www.github.com/)就是使用git的通信协议，得力于开源精神而创建的一个平台，并且linux项目也在[Github](https://www.github.com/)上托管，我们可以使用桌面软件[Source Tree](https://www.sourcetreeapp.com/)管理我们在[Github Pages](https://pages.github.com/)上的博客项目。

**学习成本**：初期算半天吧，git重操作，前期学习[廖雪峰的git教学](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)了解基本理念，之后以多用多学为主，不会咱可以google。

### Github Pages ###

[Github Pages](https://pages.github.com/)具体信息查看官网是最权威的，这里提出为何使用Github Pages搭建博客的原因：
* **版本管理**：[github](www.github.com)是目前程序员最火的一个免费的版本管理平台，所谓版本管理，主要是针对文本文档的管理，解决多人、异地、长时间维护共同项目文档所带来的问题，并因为开源精神，所以Free(免费且自由)。

* **展示**：[Github Pages](https://pages.github.com/)是给[github](www.github.com)中用户或项目的一个展示项目，并免费提供300M空间，就项目而言，提供项目介绍与使用相关，对于个人而言可以作为博客，分享知识，也有利于他人了解自己。

* **全球优良的网络**：这个平台是程序员为主的平台，有google、微软等众多一线科技公司在这发布开源项目，也有现今互联网基石linux在其平台进行管理，而且也是技术界大佬建立的公司，在全球都拥有很好的网络连接。

* **学习**：[github](www.github.com)上有很多开源的项目，因此也提供了很多开源的软件，比如[wox](https://github.com/Wox-launcher/Wox)方便windows更好的打开应用。同样，[github](www.github.com)也是程序员学习，创建开源项目的地方，可以与他人一同开发、维护开源项目，这其中必不可少的使用git版本管理工具。

* **其他领域**：[github](www.github.com)同样开始影响其他领域，可以一起创建项目翻译国外文档、写小说、写项目说明。学习并使用github或许能学习到不一样的知识。参考[gitbook](https://www.gitbook.com/)

* **搭建学习成本**：根据[Github Pages](https://pages.github.com/)创建[github](www.github.com)帐号，Github Pages项目，不到一小时。

### Hexo ###

[Github Pages](https://pages.github.com/)中介绍使用Jekyll框架，因为勘察时感觉对于个人博客而言Jekyll的主题不够华丽，而后选择了[Hexo框架](https://hexo.io/zh-cn/docs/index.html)。在使用上，需要安装Node.js，以命令行的形式使用，对于没有使用过命令行操作电脑的小伙伴还是有点不友好的。对于windows用户可以使用[cmder](https://github.com/cmderdev/cmder)代替CMD。win7以上可以使用powershell。对于什么是命令行，使用win + R组合键再输入cmd，看到的界面就是命令行界面，以输入字符的方式操作电脑。

**学习成本**：参考[Hexo命令](https://hexo.io/zh-cn/docs/commands.html),命令行常用操作也就只有一个网页，常用不到5个命令。但了解、熟悉并使用一定的自定义要看个人。全部搭建完花费4天时间。以后只要编写markdown文档和常用几个命令了。

### 静态网站 ###

**个人在该博客中对静态、动态网站的理解**：对于浏览器而言是没有多大的区别，相关的也就是使用中要与服务器发送或接收数据，对于服务器而言，静态网页差不多就是网络端存储了些网页相关的资源文件，如html、css、js、图片等，服务器主要响应文件请求与发送；而对于动态网站，我们可以参考QQ空间，每个人的空间基本结构是一样的，但是每个人每个时间点显示的信息都是不一样的，这是因为后台使用PHP、Python、Java等web服务器，在每个用户请求页面时通过与数据库交互，获取特定数据再生成特定资源文件(html、css等)发送到用户浏览器。

### Markdown ###

[Markdown](http://markdown-zh.readthedocs.io/en/latest/)官方：

>Markdown 的目标是易于阅读, 创作和编辑文章. HTML 是一种 发布 格式; Markdown 是一种 创作 格式. 因此, Markdown 处理的都是纯文本。

目前，已知在[github](www.github.com)上的项目介绍文件，就是使用Markdown，并且在项目相关的issues等一些文字发布区域，也支持Markdown的编写与预览。其他网站也有推荐使用Markdown进行回答、评论、书写，比如[简书](http://www.jianshu.com/p/d62ca374c90f)，而我们搭建的博客，就是通过使用Markdown编写成源文件，通过[Hexo框架](https://hexo.io/zh-cn/docs/index.html)与[相关主题](https://hexo.io/themes/)生成静态网页，再更新到[github](www.github.com)个人博客项目库中。并且Markdown可以通过更多插件，提供更多功能，可以预见的是，Markdown将会越加火热。

**学习成本**：半天，与git一样，初步了解后多写多练，不会或者忘记勤查文档。

## 踩过的坑 ##

### Hexo本地工程目录 ###

参考[Hexo建站](https://hexo.io/zh-cn/docs/setup.html),假设我们在HexoInit目录下使用`hexo init`命令来初始化Hexo工程，参考[Hexo配置](https://hexo.io/zh-cn/docs/configuration.html)设置配置文件，在HexoInit目录下，我们可以看到:

* node_modules目录：该目录是npm下载的一些插件，不需直接管理，如有添加、删除应尽量使用npm命令。
* public目录：当使用`hexo g`命令后，会生成静态站相关文件，先通过`hexo s`本地查看页面，以便修改，最后直接把该目录所有文件上传（commit）到[github](www.github.com)个人博客项目，然后再访问。
* scaffolds目录：该目录存放模板，模板文件是markdown文件`*.md`，初始化时已存在三个模板文件，分别对应三个`layout`。现假设我们有模板jottings.md,内容如下：


```
---
layout: post
title: {{ title }}
date: {{ date }}
update: {{ updated }}
comments: false
categories:
  - archives
  - jottings
tags:
---

```
当使用`hexo new jottings fileName`命令时，就会以jottings.md的内容生成一个fileName.md文件在source/_posts/目录下，相关配置参考[Front-matter](https://hexo.io/zh-cn/docs/front-matter.html)。
* source目录：存放使用markdown编写的源文件，当使用`hexo g`命令生成public目录文件时，Hexo和主题就会使用source目录中的*.md文件生成最后的静态网页文件，其下子目录分布与layout有关。
* themes目录：存放配置主题，相关说明见[Hexo主题说明](https://hexo.io/zh-cn/docs/themes.html)。
* _config.yml文件：这是Hexo本地工程的配置文件，相关使用说明参考[Hexo配置](https://hexo.io/zh-cn/docs/configuration.html)
* db.json文件：缓存文件，使用`hexo clean`会清除，使用`hexo g`时会生成。
* package.json文件：该本地工程npm配置文件，记录了node_modules目录安装的插件，如果存在一份配置好的package.json文件，可以直接在HexoInit目录下使用`npm install`命令读取配置文件安装相关插件。

### 备份 ###

node_modules目录内容可以根据package.json生成，public目录根据source目录生成，所以我们应备份source目录与_config.yml、package.json二个文件，另外，如果有定制化的scaffolds目录文件，也要备份，以便于今后重建工程时能快速搭建。

### 主题 ###

该博客使用的是[yelee主题](https://github.com/MOxFIVE/hexo-theme-yelee)，[更多主题选择](https://hexo.io/themes/),选择自己喜欢的主题，参考[Hexo主题说明](https://hexo.io/zh-cn/docs/themes.html)配置，一般一个完善的主题会有使用帮助，具体配置，如[yelee使用说明](http://moxfive.coding.me/yelee/)。在yelee使用说明中，可以查看主题完成显示相关，个性化相关的一些配置，也集成了一些第三方服务与评论系统。

## 相关帮助连接 ##

* [蓝灯番墙](https://github.com/getlantern/forum#%E8%93%9D%E7%81%AFlantern%E6%9C%80%E6%96%B0%E7%89%88%E6%9C%AC%E4%B8%8B%E8%BD%BD)
* [更多番墙方案](https://wsgzao.github.io/post/fq/):个人使用XX-NET
* [github](www.github.com)
* [Github Pages](https://pages.github.com/)
* [廖雪峰的git教学](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
* [Hexo官网](https://hexo.io/zh-cn/docs/index.html)
* [Hexo主题](https://hexo.io/themes/)
* [Markdown官方教程](http://markdown-zh.readthedocs.io/en/latest/)
* [cmder](https://github.com/cmderdev/cmder)
* [阮一峰博客-Github Pages搭建博客入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
* [使用GitHub和Hexo搭建免费静态Blog](https://wsgzao.github.io/post/hexo-guide/)
