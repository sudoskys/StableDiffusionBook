---
glightbox: false
---

# StableDiffusionBook

<!--
Copyright (C)  2022  StableDiffusionBook.

    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts.
    A copy of the license is included in the section entitled " GNU
    Free Documentation License ".
-->

致力于信息公开和社区连结，秉持开放共建的原则进行社区文档建设。任何人都可以参与到此文档的编写。

### 快速介绍

Stable Diffusion 是一种生成式 AI
模型，可以根据文本描述和图像产生独特逼真的图像，广泛应用于界面原型设计、广告创意设计、海报设计等领域。本项目主要介绍 [Stable Diffusion WebUi (SDWebUi)](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
，一个基于 Stable Diffusion 的图形用户界面图像生成框架。

其他相关工具如 [ComfyUI](https://github.com/comfyanonymous/ComfyUI) 也是重要的生态组成部分。本文档包含了许多技术文档，虽然有些可能已过时，但还是提供了有价值的技术背景。

### Stable Diffusion 和 Novelai 的关系

Novelai 曾泄漏其利用网络作品集（danbooru）训练的二代 Stable Diffusion 模型，后被 Stable Diffusion WebUi 支持。
现在各种模型绝大部分都基于NAI被泄漏的模型训练得来，但因为缺乏资金和技术限制，故画风大多油腻，风格趋同，不服指令。

后 Novelai 收集数据，开发出基于 SDXL 的第三代模型，风格多变且非常听话，在插画和动漫方面遥遥领先于大部分模型。

借由泄漏事件，引领了文生图领域的技术蓬勃发展，现在各大公司普遍使用特有风格模型生产素材，用于视觉设计等领域。

!!! info "**关于此事件的详细数据**"

    泄露事件 Part 1：包含生产模型和程序，大小53.66 GB，相关模型有两种分别为7GB和4GB。

    泄露事件 Part 2：包含历史测试代码和模型，大小124.54 GB，相关模型与 Part 1 相同。

### 针对不同用户的推荐资源

| 用户类型   | 推荐资源                                                                                                                                                            |
|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 设计师/美工 | [@秋葉aaaki 的视频教程](https://space.bilibili.com/12566101)、[一键启动器](https://www.bilibili.com/video/BV1iM4y1y7oA)、[蓝图节点编辑器](https://github.com/comfyanonymous/ComfyUI) |
| 二次元宅   | [Novelai](https://novelai.net/)                                                                                                                                 |

如果您是老手，可以继续阅读以下归档文件。

### 文档结构

[工作台速查传送门](https://draw.dianas.cyou/paint/)

| 文档部分     | 内容                                              |
|----------|-------------------------------------------------|
| **起步于此** | 介绍术语和新闻内容，有新动态会更新到新闻页面。                         |
| **开始之前** | 入门要求和基本方案的介绍。                                   |
| **安装配置** | WebUi 的安装和运行指南，及运行时常见错误处理。                      |
| **绘画调试** | CPU 和 NAI Leak 内容解释、应用程序配置、图片生成错误解决方法和应用程序使用技巧。 |
| **绘画指南** | 提示词书写和实战指南。                                     |
| **训练模型** | 风格化模型的使用和训练方法。                                  |
| **参考资料** | 速查链接页面，备份所有有用链接。                                |

### 访问信息

| 地址                                                                                     | 说明                |
|----------------------------------------------------------------------------------------|-------------------|
| [https://draw.dianas.cyou/](https://draw.dianas.cyou/)                                 | 主地址               |
| [https://stable-diffusion-book.vercel.app/](https://stable-diffusion-book.vercel.app/) | Vercel 版本 (大陆不可用) |

若无法访问图片或外链资源，请尝试更换网络。如果源站地址返回状态码 404/300/302/502，请提交 issue 通知我们移除此内容。

## 链接到社区

欢迎补充更多的链接。

### 创作者

| Name             | Link                                                                                     |
|------------------|------------------------------------------------------------------------------------------|
| Bilibili@秋葉aaaki | 系统性 [教材](https://space.bilibili.com/12566101)                                            |
| Bilibili@Nenly同学 | 系统性 [教材](https://space.bilibili.com/1814756990/channel/collectiondetail?sid=1285674)     |
| Bilibili@大江户战士   | [教程合集](https://space.bilibili.com/55123/channel/collectiondetail?sid=1162295)            |
| Bilibili@十岁中单林美丽 | 适合实战工作流的 [教材](https://space.bilibili.com/11620/channel/collectiondetail?sid=1234588)     |
| Bilibili@六天K     | Lora  [训练图标素材](https://space.bilibili.com/44572877/channel/collectiondetail?sid=1317059) |

### 即时讨论

| 群组名称                                   | 链接                                                                                                                                                                                             | 平台       |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Telegram  海外 Ai 绘画中文社群                 | [Telegram](https://t.me/StableDiffusion_CN) <img src="https://img.shields.io/badge/Telegram-Group-blue" alt="Telegram">                                                                        | Telegram |
| Discord 海外 Ai 绘画中文社群                   | [Discord](https://discord.gg/vhsArSSA6K) <img src="https://img.shields.io/discord/1033769426216046622?color=blue&label=Discord_Ai%E7%BB%98%E7%94%BB%E4%B8%AD%E6%96%87%E7%BB%84" alt="Discord"> | Discord  |
| Discord Ai 场外绘画社区                      | [Discord](https://discord.gg/AghgzqYUMA)                                                                                                                                                       | Discord  |
| 秋葉的甜品店 @ QQ 频道                         | [QQ 频道](https://pd.qq.com/s/9d8ffanis)                                                                                                                                                         | QQ 频道    |
| 秋葉的 Discord 频道                         | [Discord 频道](https://www.bilibili.com/opus/791435921645895680)                                                                                                                                 | Discord  |
| 并联计划组                                  | [GoogleDocs](https://docs.google.com/spreadsheets/d/1TA_xInjgS_dKdd68EL9NamCgulTBQHQpsVdCQpf3OxU/edit#gid=45042168)                                                                            | -        |
| StableDiffusion Discord English forums | [Discord 英文社群](https://discord.gg/stablediffusion) <img src="https://img.shields.io/discord/1002292111942635562?color=blue&label=Discord_stablediffusion" alt="Discord">                       | Discord  |

### 论坛链接

[StableDiffusionWebui_GitHub_discussions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions)

| 中文论坛     | 链接                                           |
|----------|----------------------------------------------|
| Nga 中文社区 | https://nga.178.com/thread.php?stid=33844263 |

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
