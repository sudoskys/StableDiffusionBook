# AiDraw

<!--
Copyright (C)  2022  StableDiffusionBook.
    Permission is granted to copy, distribute and/or modify this document
    under the terms of the GNU Free Documentation License, Version 1.3
    or any later version published by the Free Software Foundation;
    with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts.
    A copy of the license is included in the section entitled "GNU
    Free Documentation License".
-->

本页交代写给新人的话和术语解释。


## 项目

本文档主要涉及以下两个项目。


[Stable Diffusion WebUi](https://github.com/AUTOMATIC1111/stable-diffusion-webui)简称 SDWebUi，web UI是一个基于 Gradio 库的 Stable Diffusion 浏览器界面。

[NovelAI](https://novelai.net/)简称 NAI， 是一项每月订阅服务，用于 AI 辅助创作、讲故事、虚拟陪伴，或者只是供您想象的 GPT 驱动的沙盒。

Stable diffusion本来是面向三次元的，而 NAI 是二次元特化版本。


## 分流地址

- Vercel 域名(国内不可用)
[https://stable-diffusion-book.vercel.app/](https://stable-diffusion-book.vercel.app/) 短链为(https://b23.ink/aibook)

- 别名(国内可用)
[https://draw.dianas.cyou/](https://draw.dianas.cyou/)

>图片托管在Github,如果不能访问图片，请切换网络环境。


## Stable Diffusion 和 NAI 关系？我该怎么办？

因为泄漏事件，NAI 使用数千万 Danbooru（图站）图片训练的模型被泄漏了两次，而后来 **Stable Diffusion WebUi(一个通用的生成框架)** 对模型进行了支持，所以可以 使用 WebUi（简称 SDWebUi）装载 NAI 的模型。


!!! info "**事件报告**"

    泄露 Part 1 —— 包含生产模型，程序 —— 53.66 GB，其中需要下载的相关模型有 7GB 和 4GB 两种。

    泄露 Part 2 —— 包含历史测试代码和模型，程序 —— 124.54 GB，其中需要下载的相关模型与 Part1 相同。


## 社区

海外

<img src="https://img.shields.io/github/stars/sudoskys/StableDiffusionBook.svg" alt="Stars">

<a href="https://github.com/sudoskys/StableDiffusionBook/issues"><img src="https://img.shields.io/github/issues/sudoskys/StableDiffusionBook" alt="Issues"></a>



## 名词


### ENSD


在设置页中的 ENSD 是 eta 噪声种子增量。

它改变了你的种子，执行一些 eta/sigma 的操作。

NAI使用31337


### CLIP

```
CLIP is a very advanced neural network that transforms your prompt text into a numerical representation. Neural networks work very well with this numerical representation and that's why devs of SD chose CLIP as one of 3 models involved in stable diffusion's method of producing images. As CLIP is a neural network, it means that it has a lot of layers. Your prompt is digitized in a simple way, and then fed through layers. You get numerical representation of the prompt after the 1st layer, you feed that into the second layer, you feed the result of that into third, etc, until you get to the last layer, and that's the output of CLIP that is used in stable diffusion. This is the slider value of 1. But you can stop early, and use the output of the next to last layer - that's slider value of 2. The earlier you stop, the less layers of neural network have worked on the prompt.

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#ignore-last-layers-of-clip-model
```

要让 AI 作画，先要让程序“听懂”你的指令，比如

“a big cherry tree above a lake with flying petals in the sky.”

对一个相对复杂场景的文本描述，AI 需要能“理解”并匹配到对应的画面，大部分项目依赖的都是一个叫 CLIP 的模型。

CLIP 在生成模型的潜在空间进行搜索，从而找到与给定的文字描述相匹配的潜在图像。

它非常现代且高效。


### CUDA

配合 CUDA 技术，显卡可以模拟成一颗 PhysX 物理加速芯片。目前，全系列的 GeForce 8 显示核心都支持 CUDA。

使用 CUDA 技术，GPU 可以用来进行通用处理（不仅仅是图形）；这种方法被称为 GPGPU。与 CPU 不同的是，GPU 以较慢速度并发大量线程，而非快速执行单一线程。以 GeForce 8800 GTX 为例，其核心拥有 128 个内处理器。利用 CUDA 技术，就可以将那些内处理器做为线程处理器，以解决数据密集的计算。



