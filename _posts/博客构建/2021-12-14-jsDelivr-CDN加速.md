---
title: jsDelivr-CDN加速服务
description: 使用CDN加速解决图片加载失败问题
categories:
 - 博客构建
tags:
---

## 1 问题

之前构建图床的方案中，使用的是`github` + `PicGo`的，该方案存在的问题是：国内网络**加载图片速度缓慢或者图片加载失败**。

于是我考虑使用**阿里云OSS服务**构建图床，但是经过了解之后，OSS服务收取的费用主要包括两部分：
* 存储费用
* 流量费用

这两部分都可以**采用包月或者按量收费**。

存储费用比较容易解决，9元钱可以使用40GB一年；但是**流量费用比较难解决**，因为我的图床需求是用来构建个人博客，所以流量在可预期的一段时间不会太大，因此流量费用采用包月方案必然不划算，但是如果采用按量收费的方式，可能需要考虑安全性。

关于安全性的问题，我看到比较简单的方法是加入**防盗链**，基本原理就是在请求资源时，判断这个网址是不是在白名单中的；然后又看到了很多站长吐槽阿里云OSS流量被爆刷的经历，触目惊心，于是我开始寻找另外一种解决方案。

幸运的是，我发现了一种名为`jsDelivr`的开源CDN加速服务，于是开启了白嫖之路。

## 2 什么是jsDelivr

[jsDelivr](https://www.jsdelivr.com)由ProspectOne维护的公共库，使用的融合CDN技术，由Cloudflare、Fastly、StackPath、QUANTIL等CDN供应商提供了**全球超过750个CDN节点**。

最重要的是，jsDelivr在**中国大陆也拥有超过数百个节点**，因为jsDelivr拥有正规的ICP备案，解决了中国大陆的访问速度优化，实现真正的全球极速低延迟体验。jsDelivr是**免费的、不限制带宽**的，可以加速`NPM`、`Github`内的文件。

本文采用的方法就是将静态文件资源放到Github的仓库内，再使用jsDelivr进行加速访问，达到完全零成本优化访问速度。相当于一个高速访问的图床！ 

## 3 使用jsDelivr加速Github静态资源

首先在Github创建仓库，本文创建名为[https://github.com/qqqjoe/imageRepo](https://github.com/qqqjoe/imageRepo)的仓库，主要提供了**三种访问方式**，可以根据需求选择合适的方案。

### 3.1 通过releases访问

* 在仓库中releases一个发布
<img src="https://cdn.jsdelivr.net/gh/qqqjoe/imageRepo@latest/202112141911818.png"/>

* 通过`jsDelivr`加速链接为：`https://cdn.jsdelivr.net/gh/user/repo@version/file`

* 优点：可以进行**版本区分**
* 缺点：需要手动发布版本

### 3.2 直接访问

* 通过`jsDelivr`加速链接为：`https://cdn.jsdelivr.net/gh/user/repo/file`

* 不推荐使用这种办法，因为暂不清除其缓存更新机制

### 3.3 访问最新文件-**本文采用的办法**

* 通过`jsDelivr`加速链接为：`https://cdn.jsdelivr.net/gh/user/repo@latest/file`

* 优点：不需要发布release，可以直接读取
* 缺点：缓存更新时机比较随机



