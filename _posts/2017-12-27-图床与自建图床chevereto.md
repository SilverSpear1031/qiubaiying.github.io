---
layout:     post
title:      图床与自建图床chevereto
subtitle:   
date:       2017-12-27
author:     SilverSpear1031
header-img: img/post-bg-acg-3.jpg
catalog: true
tags:
    - 图床-Image hosting service
    - chevereto
---

# 图床-Image hosting service

图床，即Image hosting service，这是来自wiki的定义：
>[An image hosting service allows individuals to upload images to an Internet website. The image host will then store the image onto its server, and show the individual different types of code to allow others to view that image.](https://en.wikipedia.org/wiki/Image_hosting_service)

如果初次接触这个名词，其实可以简单的将其理解为网络相册，他们都具有如下特点：
- 可以上传图片到服务器
- 已上传的图片可以被查看

对于很多人来说，将图片通过微博，QQ说说发送到互联网上展示已经足够，但是试想当你开始撰写自己的博客，而你的博文里需要插入一些图片，注意这些图片是存储在你本地电脑里的，那么你如何插入它们？
是传到微博上，然后复制链接到博文里，还是google上传image搜索到相应的互联网链接（甚至大多数情况下互联网上没有你所存储的图片）？

所以我们需要更专业的管理图片的方式，百度“图床”你会发现有很多专业网站为我们提供了图床服务，但一定要注意，他们的大部分都是需要收费的，甚至有号称免费，一周后删光了你的图告诉你要收费的（如果我没有被某站删光了图，也没有这篇文章了，好气呀）

让我说，百度到的一些图床是极不靠谱的，甚至不如很多论坛里的个人贡献的图床，不仅收费不合理且没法打保票说它哪天不会跑路

但我会推荐使用以下方式：
- Mac上的图床利器：iPic
直接在App Store上下载，直接拖动图片到 P 图标上，或者选中图片按快捷键 ⌘+U，就能请示上传，上传成功就能直接粘贴图片的URL。默认的微博图床免费，其他自定义的收费图床目前是￥58 每年。
- Chrome的插件：[新浪微博图床](https://chrome.google.com/webstore/detail/%E6%96%B0%E6%B5%AA%E5%BE%AE%E5%8D%9A%E5%9B%BE%E5%BA%8A/fdfdnfpdplfbbnemmmoklbfjbhecpnhf?hl=zh-CN)

可以发现其中使用微博图床的方式，就是方便你将图片上传至新浪并获取链接

# 自建图床chevereto

如果你有多余的VPS，或者带宽足够，可以考虑拿来搭个图床。我个人就是使用的Vultr的5刀每月的服务器，每月1T的流量如果只用来科学上网有点暴殄天物。还是多折腾下吧！

[Chevereto is an image hosting script that allows you to create a beautiful and full-featured image hosting website on your own server. It's your hosting and your rules, say goodbye to closures and restrictions.](https://github.com/Chevereto/Chevereto-Free)

点击上面的文字可以跳转到chevereto的github页面，如果只是个人使用，免费版应该是足够的，而且搭建完成后可以在后台升级到收费版。（百度了下似乎也有盗版的存在，但是不推荐，如果因为使用盗版被通知服务器运营商封了你的服务器就得不偿失了）

下面是搭建流程：

## 1. 环境配置
在chevereto的github页面上可以了解其服务器环境的基本要求：
- Apache / NGiNX web server
- PHP 5.5.0 (standard libraries)
- MySQL 5.0 (ALL PRIVILEGES)

我推荐直接一套LAMP走起，[LAMP一键安装包](https://lamp.sh/install.html)
LAMP的默认网站根目录： /data/www/default

## 2. git拉项目
```
cd /data/www/default
git clone https://github.com/Chevereto/Chevereto-Free.git
```
到这一步，chevereto就部署完成，以我为例，图床链接为[http://45.77.14.203/Chevereto-Free/](http://45.77.14.203/Chevereto-Free/)

## 3. 配置chevereto
第一次打开网站，可能会提示：Chevereto can’t create the app/settings.php file. You must manually create this file.

这里我们只需要在app/下创建一个setting.php的空白文件即可。

之后刷新网站就会出现配置引导界面。

也有可能提示读写权限不够，只需要去修改提示的文件夹的权限即可。（chevereto的debug做的已经相当完善）

如果出现其他问题，且网站提示隐藏了错误，那是因为chevereto的debug level默认为1，需要去提示app/settings.php中的debug level，详情可见[https://chevereto.com/docs/debug](https://chevereto.com/docs/debug)

如果一切正常，可见下图，不过一般是英文
数据库即之前配置LAMP时的选择，如果使用的MariaDB也是一样的（MySQL和它是完全兼容的）
![20160819175029.png](http://45.77.14.203/Chevereto-Free/images/2017/12/27/20160819175029.png)

注意网站模式，如果是个人模式，网站是只能指定一个账户使用的，社区模式类似论坛。
但这些包括是否开启注册，在网站后台即dashboard（仪表盘）都是可以设置的。
![20160819175319.png](http://45.77.14.203/Chevereto-Free/images/2017/12/27/20160819175319.png)

## 4. 上传图片
chevereto不仅可以创建相册管理图片，也可以设置是否公开
`最重要的是，他的Embed codes功能，可以非常方便的获取各种格式的URL`
![20160819175467.jpg](http://45.77.14.203/Chevereto-Free/images/2017/12/27/20160819175467.jpg)

## 5. 图片压缩
如果你博客上所有的图片未经压缩，动辄几MB的页面肯定是不流畅的，放在图床上也很臃肿
这里推荐一个压缩图片的网站：[https://tinypng.com/](https://tinypng.com/)
主要还是免费方便，而且压缩效果不凡
![201608191754676789.jpg](http://45.77.14.203/Chevereto-Free/images/2017/12/27/201608191754676789.jpg)

## 6.总结
其实一开始我使用了docker hub上的chevereto和Mysql的安装脚本，但是docker --link始终不起作用，很是苦恼（说白了还是docker一知半解），可能是mysql镜像没有在监听服务器3306端口的原因，但不明白为什么没有监听

如果日后了解了docker镜像之间端口的开放原理，会来更新这篇文章的
