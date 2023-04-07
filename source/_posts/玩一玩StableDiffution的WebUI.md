---
title: 玩一玩StableDiffution的WebUI

index_img: /images/index_img/index_img_1.png
banner_img: /images/banner_img/banner_img_1.png
date: 2023-04-05 01:25:14
updated: 2023-04-05 01:25:14
tags:
- AI绘画
- Stable_Diffusion
categories:
- AIGC
---



摘要：本地Win11下部署Stable Diffusion模型，通过WebUI控制文本生成图片

<!--more-->

---



现在是2023年4月，基于Diffusion的AI生成图片模型的效能已经远超基于GAN的生成模型，目前主流的产品包括OpenAI的DALL·E2（2022.4）、某个小组织开发的MidJourney（2022.4）、开源的Stable Diffusion（2022.7）。三者在具体实现原理、作图风格等上都略有不同。作为一个刚接触该领域的萌新，我就想找一个较成熟的产品体验一下，最好该产品能开源代码方便DIY，而DALL·E2直接闭源，MidJourney还要基于他们的社交聊天软件才能用，于是果断选择Stable Diffusion作为我入门的第一步。

初次体验先试试大佬们基于Stable Diffusion做的一个WebUI控制台。其实现逻辑是：**先下载开源的预训练模型，在本地部署，通过WebUI界面来操作。**相比于MidJourney等在线作图的方式，在本地部署一套环境可以无视生成数量的限制，自由度高，但是对本机配置较高，要体验不同风格的模型还需预先下载（模型大到以GB为单位）。

> [理解DALL·E 2， Stable Diffusion和 Midjourney的工作原理](https://zhuanlan.zhihu.com/p/589223078)



# 一、 本地环境配置

## 1. 配置Python环境

1. 安装conda：直接上miniconda

2. 创建虚拟环境，并激活进入
3. 升级pip包，更改下载地址为清华镜像站

## 2. 配置Git

> [Git - Downloading Package (git-scm.com)](https://git-scm.com/download/win)

## 3. 安装CUDA

1. 查看本机GPU驱动版本，去官网下载匹配的版本的CUDA
2. 安装后在NVIDIA控制面板查看CUDA版本

![NVIDIA系统信息](/image-20230405100324214.png)

## 4. 克隆stable diffusion WebUI库

1. 在希望克隆的文件夹下打开CMD

2. 克隆项目源码

   ```
   git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
   ```

3. 下载预训练模型（[地址](https://huggingface.co/stabilityai/stable-diffusion-2-1/blob/main/v2-1_768-ema-pruned.ckpt)），并转移到对应文件夹`models\Stable-diffusion`

4. 进入项目根目录，运行`webui-user.bat` 即可完成本地Web部署
   - 如果长时间卡在Installing gfpgan(或者installing clip，installing open_clip)这个环节，那么进入F:\stable-diffusion-webui文件夹下面，找到launch.py这个文件，在第200多行到300行的位置在“[https://github.com/xxx](https://link.zhihu.com/?target=https%3A//github.com/xxx)”的最前面，加上：`https://ghproxy.com/`，相当于让原来从github下载相关程序包变成了走国内镜像下载相关程序包，这样会增加网络的稳定性和网络速度，此时需关闭梯子。

> [从零开始，手把手教你本地部署Stable Diffusion Webui AI绘画(Win系最新版) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/613530403)



# 二、WebUI操作指南

## 1. 开启server流程：

1. 在项目根目录下`E:\Stable_Diffusion_WebUI\stable-diffusion-webui`打开CMD，进入虚拟环境`sdwebui`
2. 执行`webui-user.bat`，浏览器打开`http://127.0.0.1:7860` 。（注意关掉梯子）

（3. 完整执行`.\webui.bat --medvram --xformers`）





# 三、提示词promot

## 1. 语法







## 2. 参考其他开源模板

​	如Civitai网站。选择参考的图片，复制其promot信息到UI中

> - [Prompt Search](https://www.ptsearch.info/articles/list_best/)
> - [MeinaMix](https://civitai.com/models/7240/meinamix): 二次元风格



# 四、controlnet

Controlnet 允许通过线稿、动作识别、深度信息等对生成的图像进行控制。 





# 报错记录

- 不管什么操作都会报错：`Something went wrong Expecting value: line 1 column 1 (char 0)`

​	网络代理问题，关掉梯子，再重新启动服务器，之后就没问题了。

> [[Bug\]: Something went wrong Expecting value: line 1 column 1 (char 0) · Issue #9174 · AUTOMATIC1111/stable-diffusion-webui (github.com)](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/9174)

- 点击generate后报错：RuntimeError: Not enough memory, use lower resolution (max approx. 512x512). Need: 0.2GB free, Have:0.1GB free

​	执行`.\webui.bat --medvram`，增加`--medvram`

> [Troubleshooting · AUTOMATIC1111/stable-diffusion-webui Wiki (github.com)](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Troubleshooting#low-vram-video-cards)

- 在生成图片过程中报错：AttributeError: 'NoneType' object has no attribute 'local_data_path'

​	`modules\realesrgan_model.py`中出现bug，参考下面的issue

> [[Bug\]: AttributeError:	"NoneType' object has no attribute 'data_path' · Issue #5170 · AUTOMATIC1111/stable-diffusion-webui (github.com)](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/5170"

- 生成图片到一半就卡住，然后报错CUDA out of memory

​	GPU性能不够，将`Upscale by`设置为1，图片size最高支持到512x1024





# 参考文献

> - SD模型源码地址：[CompVis/stable-diffusion: A latent text-to-image diffusion model (github.com)](https://github.com/CompVis/stable-diffusion)
> - SD_WebUI项目地址：[AUTOMATIC1111/stable-diffusion-webui: Stable Diffusion web UI (github.com)](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
> - [如何用AI绘图画出超真实的小姐姐？](https://mp.weixin.qq.com/s?__biz=MzI2NTQ0MjY5Nw==&mid=2247484745&idx=1&sn=2c4ae77c2138d561b45de5f4002d2b77&chksm=ea9c01afddeb88b92ffbec68c7badf6e0836f43977650c0bbb3eeb04ca1adfbe3ac43a2de28d&scene=21#wechat_redirect)
