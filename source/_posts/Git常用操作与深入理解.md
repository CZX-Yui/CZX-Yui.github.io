---
title: Git常用操作与深入理解
typora-root-url: ./Git常用操作与深入理解/
index_img: /images/index_img/index_img_2.png
banner_img: /images/banner_img/banner_img_2.png
date: 2023-04-04 18:37:27
updated: 2023-04-04 18:37:27
tags:
categories:
- Git
---





摘要：学习Git的基本用法，发掘内部原理，理解与GitHub的关联流程，以及优雅地解决连接GitHub失败的问题

<!--more-->

# 一、Git本地操作与内部原理

- git本地操作原理

[这才是真正的Git——Git内部原理揭秘！ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/96631135)









# 二、关联远程仓库GitHub

## 1. 关联远程仓库

git和GitHub是两回事，git是本地版本控制软件，GitHub是远程版本控制仓库。通俗理解就是，git安装在本地，权力范围只有本地被初始化git的文件夹，而GitHub存在在云端（且往往是分布式，可能在存在在好几个人的电脑上），只是个任人进出的仓库。



### 先本地创建仓库，再关联到云端

- 准备好本地仓库，在某个文件夹下执行`git init`
- 准备好远程仓库，在GitHub上新建一个仓库
- 执行同步命令`git remote add origin git@github.com:CZX-Yui/xxxx.git` ，现在以及建立起本地文件夹和远程GitHub仓库的联系了
  - 远程库的名字就是`origin`，这是git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。
- 之后每次有文件更新，用`git push origin main`将本地更新同步到远程仓库
  - main是远程分支的名称

### 云端已有仓库，拉取到本地

- 找到云端仓库的SSH链接
- 在本地文件夹下执行`git clone ssh://xxxx`

> [远程仓库 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

## 2. GitHub通信方式及断网问题总结

### 省流

- 常见连不上报错（连不上server、超时等等），大概率都是因为采用HTTP协议连接远程仓库，如clone仓库时目标仓库地址是http打头、链接远程库时地址也是http打头等等
- 一种解决策略是设置代理，但不是很稳定，建议用下面的方法
- 另一种解决策略是花点时间配置SSH。养成好习惯，每次涉及到使用GitHub链接时都写成SSH形式。



### 原理分析

- 在使用过程中，除了好几个与git管理版本相关的命令让人摸不着头脑外（主要是不太理解背后的原理），最让人头疼的就是本地git连接远程GitHub仓库频频报错的问题了（感谢防火墙）。

- 为了彻底解决这个问题，还得从背后原理上搞明白。首先得明确访问远程Github仓库的几种通信协议：HTTPS和SSH。

  - HTTP/HTTPS是最基本的通信方式，方便快捷，不管收端是谁、在哪，只要能拿到发端的http链接，就能直接访问，因此也容易受到墙的限制，而SSH相比HTTP多加了一层“保护壳”，会在收发端通信的过程中自动加密和解密，这种安全传输（“隧道”）允许穿过防火墙，但是其灵活性较差，收端和发端固定只有那两个设备/地址。简单来说，举个例子，通过HTTP方式clone仓库可以在任意设备上克隆任意地址的仓库，而通过SSH方式只能在配置了密钥的两台设备间clone/push仓库。SSH的优势就在于连接稳定、push的时候不需要每次输入密码来证明我是该用户。

  - 网上一种说法“使用SSH的URL克隆的话，你必须是这个项目的拥有者“，这种表述不完全正确，对于GitHub上的公开库，你可能不是该项目的拥有者或管理员，但当你配置好本机和GitHub的SSH连接后，你依然可以通过SSH来clone所有的GitHub公开库。

> [SSH Tunnel绕过防火墙穿透内网 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/46243205#:~:text=ssh协议里面封,破防火墙的限制。)

### 配置HTTP代理（不推荐）

- 首先得配置好梯子，查看本机梯子http走的端口

- 使用下述命令配置全局代理

  ```
  git config --global http.proxy http://127.0.0.1:7890
  ```

> [一文让你了解如何为 Git 设置代理 - Eric (ericclose.github.io)](https://ericclose.github.io/git-proxy-config.html)



### 配置SSH（推荐）

- 本机查看私钥和公钥
- 将公钥复制一份到GitHub的个人账号下

> [Github配置ssh key的步骤（大白话+包含原理解释）](https://blog.csdn.net/weixin_42310154/article/details/118340458)
>
> [设置代理解决github被墙 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/481574024)





# 三、Demo实践

抽空复习试一试，顺便做一下常用命令总结



