---
title: 基于Hexo框架搭建个人博客网站并部署在GitHub上

date: 2023-04-02 22:04:59
updated: 2023-04-03 15:09:09
tags: 
- Hexo
- GitHub
categories: 
- web_dev
index_img: /images/index_img/index_img_0.png
banner_img: /images/banner_img/banner_img_0.png
typora-root-url: ./基于Hexo框架搭建个人博客网站并部署在GitHub上
---

摘要：记录第一次搭建个人博客的全过程，学会了简单的网页部署，搭建了一个十分舒服的知识管理系统，Nice！技术栈包括Hexo基本框架，Fluid主题，GitHub.io云端部署等

<!--more-->

---

> 引言：我为什么要开始写博客？

不是所有的程序员都写博客，但是优秀的程序员都有写博客的习惯。因为这一门职业必备两项基本素质：**持续学习**和**开源精神**，写博客的过程正是践行了这两点。坚持每日学习，吸收前沿知识和最新信息，通过写博客的方式进行自我总结和思考，并开源到互联网上供他人参考与交流。此外，在写博客和运营网站的过程中，还能提高自己的语言表达能力、问题理解与解释能力、设计与艺术能力等。总之，每天抽一点时间做做总结，每周再反思归纳归纳，对一个人技术水平的提升是大有裨益的。

这周末花了两天时间，总算是搭建好了自己的博客系统，趁热打铁赶紧总结一波，作为本站的第一篇文章。我将先复盘搭建博客系统的全部配置细节，再总结日后使用的流水线操作方法。

---

# 一、博客系统配置

我的需求一开始是想做一个知识汇总用的笔记软件，因为一直用的Notion，非常好用，但是怕毕业了没有学校的邮箱（可以免费用），而购买它的服务又太贵了（每个月5刀），而且总觉得自己的东西放在别人的平台上不太放心，万一哪天公司跑路了东西就都没了，而且人家还是个美国公司（虽然老板是华裔）。因此选择了博客这个平台来做知识管理，将每天的随笔、笔记什么的保留在互联网上，而且公开可见，万一做的好的话还能增加流量出个名什么的hhh（主要还是自己总结用啦）。

调研了一下现有的搭建博客方案，一种方案是直接用现有平台的（CSDN、博客园、掘金、简书、知乎等等），比较方便，容易被搜索引擎搜到，但是功能较单一，扩展性差，风格固化，最不能忍受的是还有广告牛皮癣！而另一种方案是本地建站再部署到云端，DIY成分更多，但是难度也大，需要对全栈有个基本了解。以后我的目标是用方案二，但是现在初步入门还是先快速搭建出一个Demo较好（劝退警告），因此我选择的方案是介于两者之间的：**本地采用md写作，再用博客生成框架直接转译成网站源码，再通过GitHub部署到云端。**

三步流程的技术选型：

- **本地markdown写作**：使用**Typora**，永久购买89元。（看到一句话被激励到，立刻购买了：你愿意花大几百去买游戏，却不愿意花几十块买个好用的生产力工具？）
- **本地建站框架**：其实最佳的建站方法是自己写网站代码，灵活度高，但是对新手不友好。因此这部分选择目前主流的“纯文本md转译为HTML的博客建站框架”，GitHub官方推荐的是Jekyll框架，尝试了下因为不方便修改风格而且教程较老就放弃了，目前选择的是**Hexo**框架，搭配开源主题插件**Fluid**，美&爽！

- **云端部署**：采用**GitHubPages**功能。GitHub界面美观且功能强大，还提供了免费的网站部署（虽然资源受限，但是做个博客绰绰有余了）。

本地环境：基于Windows11系统，在LEGION2022-R7000笔电上安装，默认已配置好VPN（随时会用）

## 1. 本地环境配置

本地配置的目标是完成本地建站功能，至于部署到云端再到怎么能成功访问就是GitHub帮我们做好的事情了。因此需要配置用于本地建站的**Node.js**和用于GitHub部署的**git**。有了这个基础，再去安装博客转译框架**Hexo**。

1. 安装[Node.js](https://nodejs.org/en)
2. 安装[Git](https://git-scm.com/)
3. 安装[Hexo](https://hexo.io/zh-cn/docs/#%E5%AE%89%E8%A3%85)

安装完成使用-v命令查看版本测试安装是否成功。注意查看Hexo版本必须在安装Hexo的目录下看（我的是MyBlog博客根目录），否则不显示Hexo的版本只显示hexo-cli的版本。

> [Hexo官方教程（安装）](https://hexo.io/zh-cn/docs/#%E5%AE%89%E8%A3%85)
>
> [Hexo博客详细教程（一）| 建立本地站点 - 腾讯云开发者社区 ](https://cloud.tencent.com/developer/article/1662732)

## 2. 本地建站与项目文件说明

### 建站流程

#### 1. 初始化站点

1. 第一次建站，选择一个文件夹作为站点根目录，在终端打开，执行以下命令新建文件

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

2. 此时直接运行`hexo s`即可在本地部署demo网站，启动server，打开本地浏览器链接即可访问

3. 之后每一次执行修改，重新生成站点页面时都需要先运行`hexo clean`清除历史，再运行`hexo g`生成HTML页面

> [Hexo官方教程（建站）](https://hexo.io/zh-cn/docs/setup)

#### 2. 修改配置文件

1. 修改根目录下的_config.yml文件，这是站点全局配置文件。使用vscode打开，参考官方教程进行配置。

2. 修改默认模板文件post.md，添加Front-matter字段。（添加标签、分类等便于管理）

> [Hexo官方教程（配置）](https://hexo.io/zh-cn/docs/configuration)
>
> [Front-matter | Hexo](https://hexo.io/zh-cn/docs/front-matter)

#### 3. 更换主题

默认的主题不好看，下载安装Fluid主题。参考官方文档进行配置。

我用npm install方法将hexo-theme-fluid安装在node_module库中，并将其中的配置文件复制到根目录下_config.fluid.yml，在这里配置和主题相关的事项。

主题文件夹里的source和根目录下的source在编译后会合并，因此保存图片优先放在根目录下的source的images文件夹里。

> [开始使用 | Hexo Fluid 用户手册 (fluid-dev.com)](https://hexo.fluid-dev.com/docs/start/#主题简介)
>
> [配置指南 | Hexo Fluid 用户手册 (fluid-dev.com)](https://hexo.fluid-dev.com/docs/guide/)

### 项目文件组织说明

项目文件组织如下：大部分都是配置文件，核心放md文档的文件只有source

- .github：git配置文件
- themes：网站主题，类似布局模板。没下载一个主题就会将主题文件夹放在该文件夹里，核心是主题的_config.yml配置文件，里面一般定义页面的标题、链接啥的
- source：存放每个单独博文的md文件、md文件对应的图片文件夹，新建page后还能按category、tag管理（和主题相关，有的主题可能没有）
- node_modules：node.js的lib库
- scaffolds：md文件的模板文件，新建post、page、draft会自动复制里面的模板。模板里只定义了yml头字段，部分自动写好（title、date），其余在写文章时需自行填写。每次新建md文件默认拷贝post.md，如果有什么需要通用的修改（关于Front-matter或者全文通用模板）可以直接修改该文件。
- public：`hexo g`后生成的网站文件，该文件夹可以单独拿出来，当作网站部署的源码
- .deploy_git：`hexo d`后生成的git部署文件，用于和GitHub同步
- _config.yml：核心文件，全局配置
- _config.fluid.yml：核心文件，主题相关全局配置（我的主题名为fluid）
- .npmignore/package.json/package-lock.json/db.json：其余配置文件

![项目文件组织](image-20230403114238355.png)

## 3. 远程部署到GitHub的两种方式

- 初次使用：在GitHub上建库，尾缀为.github.io
- 安装git部署插件：`npm install hexo-deployer-git --save`



### 1. 简易部署

具体流程理解为：先在本地生成页面文件，存在public文件夹下，再将public文件夹同步到GitHub库的main分支上，并自动触发action的pages build and deployment流程

1. 将全局配置文件_config.yml配置如下：repo地址设置为ssh连接，因为之前设置了本地连GitHub用的ssh密钥，这样比较稳定

```
deploy:
  type: git
  repo: git@github.com:CZX-Yui/CZX-Yui.github.io.git
  branch: main
```

2. 每次更新完后，在根目录下先`hexo clean`，再部署站点：`hexo d` 

3. 刷新GitHub可以看到已经被同步，再打开个人博客网站刷新即可更新（GitHub部署较慢要等几分钟）

> [Hexo博客教程（三）| Github、Coding 部署Hexo站点详解 - 腾讯云开发者社区](https://cloud.tencent.com/developer/article/1662782)



### 2. 开辟新分支部署备份库（推荐）

简易部署只将编译后的网页文件同步在GitHub库上，现在希望将源文件也同步在该库中。新建一个分支source来专门同步源文件。再利用action功能，每次push到source后触发，执行文件编译并部署。这种模式的好处在于可以在线修改文档再直接GitHub网站上提交，当手边的机器没有node、hexo环境时非常好用。

1. 在GitHub该库中新建分支source，并设置为默认分支

2. 在本地站点根目录初始化git仓库，创立source分支，删除public文件、主题文件夹里的.git文件，再将本地目录与远程仓库关联（SSH关联）

   ```
   git init
   git checkout -b source
   hexo clean
   git remote add origin git@github.com:CZX-Yui/CZX-Yui.github.io.git
   ```

3. 新建.gitignore，配置如下：

   ```
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/
   ```

4. 在站点根目录下新建`.github/workflows/pages.yml`，添加内容如下，解释写在注释里

   ```
   name: Pages
   
   on:                 # 触发条件：push到分支source的时候
     push:
       branches:
         - source # default branch
   
   jobs:               # 触发后执行任务
     pages:			# 任务名
       runs-on: ubuntu-latest
       permissions:
         contents: write
       steps:   									# 任务步骤，每行-打头表示一个任务
         - uses: actions/checkout@v2
         - name: Use Node.js 16.x
           uses: actions/setup-node@v2
           with:
             node-version: "16.18.0"				# 改成和本地环境一致
         - name: Cache NPM dependencies
           uses: actions/cache@v2
           with:
             path: node_modules
             key: ${{ runner.OS }}-npm-cache
             restore-keys: |
               ${{ runner.OS }}-npm-cache
         - name: Install Dependencies
           run: npm install
         - name: Build
           run: npm run build
         - name: Deploy
           uses: peaceiris/actions-gh-pages@v3
           with:
             github_token: ${{ secrets.GITHUB_TOKEN }}
             publish_branch: main						# 部署到main节点，该节点同步页面文件
             publish_dir: ./public						# 部署源文件的目录
   
   ```

5. 推送三连：add、commit、push

   ```
   git add .
   git commit -m 'hexo source post'
   git push origin source
   ```

> [在 GitHub Pages 上部署 Hexo | Hexo](https://hexo.io/zh-cn/docs/github-pages.html)
>
> [使用git分支保存hexo博客源码到github ](https://www.jianshu.com/p/8814ce1da7a4)
>
> [GitHub Actions 的工作流语法 - GitHub 文档](https://docs.github.com/zh/actions/using-workflows/workflow-syntax-for-github-actions)





# 二、新建博客流水线

1. 进入**本地站点根目录**`D:\Documents\Github.io\MyBlog` ，在终端中打开

2. 新建博客： `hexo new <title>` ，进入source->_posts，用Typora打开对应的md文件

3. 愉快地编写markdown。

   1. 设置index和banner图片，拷贝到source/images两个对应文件夹中。重命名尾部数字，更改到md文档的Front-matter部分的两个字段。

   2. 插入图片时：

      - 本地图片：直接copy进来，修改路径只保留图片名（若要在Typora里显示就改“格式--图像--设置图像根目录”）

      - 网络图片：复制图片链接，写一段插入图片的md语法

   3. 记得编写文章头部的摘要，100字以内

4. （可选）本地发布测试：在终端执行 `hexo clean ; hexo g ; hexo s`

5. ~~（简易部署）部署到GitHub上：在终端执行`hexo clean ; hexo d`~~

6. （备份部署）最好先在本地测试没问题了，再将源文件同步到GitHub，自动执行部署

   ```
   git add .
   git commit -m "xxxx"
   git push origin source
   ```

   



# 三、其余细节问题

### 1. 新建博客时命名注意

- 不要出现“+”号，否则会自动识别为“-”号，造成后续导入图片时与默认“typora-root-url”命名不符合。尽量命名不要出现特殊符号

### 2. Typora图片插入

- 本地插入图片时默认图片地址为“**/xxx.jpg**”，在官方[hexo-renderer-marked](https://github.com/hexojs/hexo-renderer-marked)插件中存在bug，插件希望图片地址前面没有反斜杠“/”，因此不能识别到该地址出现bug。直接的解法是在每次插入图片后手动删掉反斜杠，但是太麻烦了。我尝试直接去node_modules源码里面修改，成功：hexo-renderer-marked/lib/**renderer.js**文件中，删掉下面标黄的部分，即允许文件名从“/”开头。


![hexo插入图片的bug](hexo插入图片的bug.png)

- 图片中含有中文出错：在Typora偏好设置->图像，不要勾选插入时自动转译图像URL，否则会将中文转译成ASCII码，在Hexo插件转译路径时不匹配出错。
- （TODO）图片进行缩放出错：缩放后默认格式变化，Hexo插件识别不了，这个暂时没有解决
- 0406发现新BUG：用同步GitHub源文件再自动部署的方法，GitHub的在线环境没有修改该BUG，还是会出现图片路径转译出错的问题。即上述解法只适用于本地调试好、再hexo d部署的方式，而且上述解法中默认地址为“xxx.jpg”是因为在typora里设置图片的根目录为md同名文件夹了，每次都要设置太麻烦。所以最优解法还是每次插入图片后都改一下图片路径，只保留图片名称。若要在Typora里显示，就改图片根目录

> [资源文件夹 | Hexo](https://hexo.io/zh-cn/docs/asset-folders#使用-Markdown-嵌入图片)
>
> 类似问题：[md img render part improvement of hexo-render-marked · Issue #216 ·](https://github.com/hexojs/hexo-renderer-marked/issues/216)
>
> [typora + hexo博客中插入图片 | yinyoupoet的博客](https://yinyoupoet.github.io/2019/09/03/hexo博客中插入图片/)



### 3. 部署github报错

1. 上传出错

   - 报错内容：`err: Error: Spawn failed`

   - 解决方法：将原来的部署配置改成ssh连接


> [Hexo部署出现错误err: Error: Spawn failed解决方式](https://blog.csdn.net/weixin_41256398/article/details/117994899)
>
> [hexojs/hexo-deployer-git: Git deployer plugin for Hexo. (github.com)](https://github.com/hexojs/hexo-deployer-git)

2. 部署出错

   - 通过F12排查问题，CSS文件问题，文件路径的问题
   - 在站点根目录的_config.yml文件中加入一行：`root: /`


> [hexo+Github搭建博客，能访问但无法加载css文件_StarryaSky的博客-CSDN博客](https://blog.csdn.net/StarryaSky/article/details/83378011)

### 4. 设置帖子的摘要并限制字数

- Hexo官方文档建议在Front-matter处添加字段“excerpt”，并在其后添加摘要描述，这种方法不太优雅不够灵活。

- Hexo官方还提供了一种[配置](https://hexo.io/zh-cn/docs/tag-plugins#%E6%96%87%E7%AB%A0%E6%91%98%E8%A6%81%E5%92%8C%E6%88%AA%E6%96%AD)，在文章开头使用 `<!-- more -->`，那么 `<!-- more -->` 之前的文字将会被视为摘要。首页中将只出现这部分文字，同时这部分文字也会出现在正文之中。测试发现大概可以写100个字以内能完全展示。

- 这部分已经内置到post.md中，以后直接copy即可

### 5. 每个帖子背景图片配置

- 参考[Fluid文档关于文章页](https://hexo.fluid-dev.com/docs/guide/#%E6%96%87%E7%AB%A0%E5%9C%A8%E9%A6%96%E9%A1%B5%E7%9A%84%E5%B0%81%E9%9D%A2%E5%9B%BE)的描述，可以在md文章Front-matter处添加字段`index_img`显示文章在首页的小图和`banner_img`显示文章在详情页的背景大图。

- 如果不指定这两个值，会采用_config.fluid.yml中配置的默认值

- 配置路径标准：不一定要绑定每一篇文章，可以随机一些。全部放在source/images下，分别建立两个文件夹，一个存index图片，一个存banner图片。图片命名以数字结尾。图片格式为png。在post.md模板中定义为：(注意图片名称x替换为数字)

  ```
  index_img: /images/index_img/index_img_x.png
  banner_img: /images/banner_img/banner_img_x.png
  ```

### 6. hexo new page的作用是什么？

TODO。。。

### 7. 增加文章在线编辑功能

- 需要先完成备份部署的配置。希望实现的流程是每次写完本地md，直接推送源码到git，使用git action自动部署，就不用本地做`hexo d`的操作了。
- （TODO）还是GitHub action的问题，yml配置的问题

### 8. 新环境下拉取GitHub并部署环境

- 新建文件夹，在该目录下启动Git Bash
- 直接拉取个人账户下的github.io项目，source分支



# 四、总结

#### 技术栈

- [GitHub Pages](https://pages.github.com/)

- [Hexo](https://hexo.io/zh-cn/)

- [Fluid](https://hexo.fluid-dev.com/docs/)

#### 心得

- 灵活配置好md文章中Front-matter的部分，这是每篇文章将如何在页面中展示的“控制台”。以及_config.yml文件，这是全局样式配置的“控制台”。

- 搭建过程中遇到问题主要还是参考官方文档的说明，相比于到处找帖子而且说的都不一样，官方文档会及时更新，有啥bug都会及时修正。因为这个框架也算是各个玩家们的作品，少有盈利组织在专门运维，有点bug很正常。

- 常常遇到本地建站测试没问题，一部署到GitHub就会出现问题，这时开源打开F12debug，一般是链接出错。
