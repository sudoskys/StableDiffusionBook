---
glightbox: false
---

# AiDraw

<!--
Copyright (C)  2022  StableDiffusionBook.

    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts.
    A copy of the license is included in the section entitled " GNU
    Free Documentation License ".
-->

StableDiffusionBook 致力于信息公开和社区连结，秉持开放，共建的原则进行社区文档建设。任何人都可以参与到此文档的编写工作。

本页交代写给新人的话和术语解释。

## 项目

本文档主要涉及以下两个项目。（以后可能会更多？)

[Stable Diffusion WebUi](https://github.com/AUTOMATIC1111/stable-diffusion-webui) 简称 SDWebUi，web UI 是一个基于 Gradio
库的 Stable Diffusion
浏览器界面。该项目目前还没有 [许可证](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/24) [相关 Issue](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2059)。

[NovelAI](https://novelai.net/) 简称 NAI， 是一项每月订阅服务，用于 AI 辅助创作、讲故事、虚拟陪伴，或者只是供您想象的 GPT
驱动的沙盒。

Stable Diffusion 本来是面向三次元的，而 NAI 是二次元特化版本。

## 导航

[工作台速查传送门](https://draw.dianas.cyou/paint/)

**起步于此**

介绍了一些术语和新闻内容。有新动态会更新到新闻页面。

**开始之前**

列举了入门的要求，介绍了一些基本的方案。

**安装配置**

介绍了 WebUi 的安装和运行，还有运行时候遇到的错误处理指南。

**绘画调试**

解释了关于 GPU 和 NAI Leak 的内容。

还介绍了如何配置应用程序，解决图片生成出现的错误，介绍了应用程序本身相关的使用技巧。

**绘画指南**

介绍了如何应用和相关的实践操作，指导如何书写提示词 和 如何实战。

**训练模型**

介绍了如何 **使用** 和 训练 风格化模型，以及基本常识和训练方法。

**参考资料**

这是一个用于速查的快速链接页面，备份了所有有用的链接。

## 访问可用性

* 主地址
  [https://draw.dianas.cyou/](https://draw.dianas.cyou/)

* Vercel （中国大陆不可用）
  [https://stable-diffusion-book.vercel.app/](https://stable-diffusion-book.vercel.app/) 短链为 (https://b23.ink/aibook)

如果不能访问图片或外链资源，请尝试切换网络环境。如果源站地址返回状态码 404/300/302/502 ，可以提交 issue 通知我们移除此内容。

## Stable Diffusion 和 NAI 关系？我该怎么办？

因为泄漏事件，NAI 使用数千万 Danbooru（图站）图片训练的模型被泄漏了两次。 目前社区广泛使用 **Stable Diffusion WebUi** 生成框架
（简称 SDWebUi）装载 NAI 的模型。

!!! info "**事件报告**"

    泄露 Part 1 —— 包含生产模型，程序 —— 53.66 GB，其中需要下载的相关模型有 7GB 和 4GB 两种。

    泄露 Part 2 —— 包含历史测试代码和模型，程序 —— 124.54 GB，其中需要下载的相关模型与 Part1 相同。

## 链接到社区

欢迎补充更多的链接。

### 即时讨论

| 群组名称                                   | 链接                                                                                                                                                                                             | 平台       |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Telegram  海外Ai绘画中文社群                   | [Telegram](https://t.me/StableDiffusion_CN) <img src="https://img.shields.io/badge/Telegram-Group-blue" alt="Telegram">                                                                        | Telegram |
| Discord 海外Ai绘画中文社群                     | [Discord](https://discord.gg/vhsArSSA6K) <img src="https://img.shields.io/discord/1033769426216046622?color=blue&label=Discord_Ai%E7%BB%98%E7%94%BB%E4%B8%AD%E6%96%87%E7%BB%84" alt="Discord"> | Discord  |
| Discord Ai 绘画繁体中文社群                    | [Discord](https://discord.gg/AghgzqYUMA)                                                                                                                                                       | Discord  |
| 并联计划组                                  | [GoogleDocs](https://docs.google.com/spreadsheets/d/1TA_xInjgS_dKdd68EL9NamCgulTBQHQpsVdCQpf3OxU/edit#gid=45042168)                                                                            | -        |
| StableDiffusion Discord English forums | [Discord 英文社群](https://discord.gg/stablediffusion) <img src="https://img.shields.io/discord/1002292111942635562?color=blue&label=Discord_stablediffusion" alt="Discord">                       | Discord  |

### 论坛链接

[StableDiffusionWebui_GitHub_discussions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions)

| 中文论坛    | 链接                                           |
|---------|----------------------------------------------|
| Nga中文社区 | https://nga.178.com/thread.php?stid=33844263 |

| English forums                          | Link                                                                |
|-----------------------------------------|---------------------------------------------------------------------|
| Reddit-DreamBooth                       | https://www.reddit.com/r/DreamBooth/                                |
| Reddit-StableDiffusion                  | https://www.reddit.com/r/StableDiffusion/                           |
| StableDiffusionWebui_GitHub_discussions | https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions |
| Reddit-NovelAi                          | https://www.reddit.com/r/NovelAi/                                   |
| AIBooru                                 | https://aibooru.online/                                             |
| Reddit                                  | https://www.reddit.com/r/StableDiffusion/                           |

### 模型

| 社区名称         | 链接                     |
|--------------|------------------------|
| Hugging Face | https://huggingface.co |
| Civitai      | https://civitai.com/   |
