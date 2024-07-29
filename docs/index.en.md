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

Our mission is to build community documentation based on the principles of openness and collaboration. Anyone can
contribute to the creation of this document.

### Quick Introduction

Stable Diffusion is a generative AI model that can produce unique and realistic images based on textual descriptions and
images. It is widely used in interface prototyping, advertising creative design, poster design, and other fields. This
project mainly introduces [Stable Diffusion WebUi (SDWebUi)](https://github.com/AUTOMATIC1111/stable-diffusion-webui), a
graphical user interface (GUI) image generation framework based on Stable Diffusion.

Other related tools, such as [ComfyUI](https://github.com/comfyanonymous/ComfyUI), are also important parts of this
ecosystem. This document contains many technical pieces, some of which may be outdated but still provide valuable
technical background.

### Relationship Between Stable Diffusion and NovelAI

NovelAI once leaked its second-generation Stable Diffusion model, which was trained using the danbooru online works
collection, and was later supported by Stable Diffusion WebUi. Now, most models are trained based on the leaked NAI
models, but due to lack of funds and unity, the art styles are mostly greasy and similar, often disobeying commands.

Later, NovelAI collected data and developed the third-generation model based on SDXL, which features diverse and
obedient styles, leading most models in the field of illustration and anime.

The leak event spurred vigorous development in the text-to-image field. Now, many companies generally use proprietary
style models to produce materials for visual design and other fields.

!!! info "**Detailed Data**"

    Leak Incident Part 1: Includes production models and programs, size: 53.66 GB. There are two related models: 7GB and 4GB.

    Leak Incident Part 2: Includes historical test codes and models, size: 124.54 GB. The related models are the same as in Part 1.

### Recommended Resources for Different Users

| User Type       | Recommended Resources                                                                                                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Designer/Artist | [@秋葉aaaki's Video Tutorials](https://space.bilibili.com/12566101), [One-Click Launcher](https://www.bilibili.com/video/BV1iM4y1y7oA), [Blueprint Node Editor](https://github.com/comfyanonymous/ComfyUI) |
| Anime Fans      | [NovelAI](https://novelai.net/)                                                                                                                                                                          |

If you are a researcher, you can continue reading the archived files below.

### Document Structure

[Workbench Quick Access Portal](https://draw.dianas.cyou/paint/)

| Document Section                   | Content                                                                                                                           |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| **Start Here**                     | Introduction to terminology and news. New updates will be posted on the news page.                                                |
| **Before Starting**                | Introduction to entry requirements and basic plans.                                                                               |
| **Installation and Configuration** | Guide to installing and running WebUi, and handling common errors during operation.                                               |
| **Drawing and Debugging**          | Explanation of CPU and NAI Leak content, application configuration, image generation error solutions, and application usage tips. |
| **Drawing Guide**                  | Guidelines for writing prompts and practical guides.                                                                              |
| **Model Training**                 | Methods for using and training stylized models.                                                                                   |
| **References**                     | Quick links page, backing up all useful links.                                                                                    |

### Access Information

| URL                                                                                    | Description                                         |
|----------------------------------------------------------------------------------------|-----------------------------------------------------|
| [https://draw.dianas.cyou/](https://draw.dianas.cyou/)                                 | Main URL                                            |
| [https://stable-diffusion-book.vercel.app/](https://stable-diffusion-book.vercel.app/) | Vercel Version (Not accessible from mainland China) |

If you cannot access images or external resources, please try changing your network. If the source site URL returns a
status code of 404/300/302/502, please submit an issue to notify us to remove this content.

## Some communities

### Chat

**CN**

- [Telegram Ai 绘画中文社群](https://t.me/StableDiffusion_CN)
  <img src="https://img.shields.io/badge/Telegram-Group-blue" alt="Telegram">

- [Discord Ai 绘画中文社群](https://discord.gg/vhsArSSA6K)
  <img src="https://img.shields.io/discord/1033769426216046622?color=blue&label=Discord_Ai%E7%BB%98%E7%94%BB%E4%B8%AD%E6%96%87%E7%BB%84" alt="Discord">

- [并联计划组](https://docs.google.com/spreadsheets/d/1TA_xInjgS_dKdd68EL9NamCgulTBQHQpsVdCQpf3OxU/edit#gid=45042168)

**EN**

- stablediffusion [Discord English](https://discord.gg/stablediffusion)
  <img src="https://img.shields.io/discord/1002292111942635562?color=blue&label=Discord_stablediffusion" alt="Discord">

### Forum

- [StableDiffusionWebui_GitHub_discussions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions)

**CN**

- [Nga](https://nga.178.com/thread.php?stid=33844263)

**EN**

- [Reddit-DreamBooth](https://www.reddit.com/r/DreamBooth/)

- [Reddit-StableDiffusion](https://www.reddit.com/r/StableDiffusion/)

- [Reddit-NovelAi](https://www.reddit.com/r/NovelAi/)

- [AIBooru](https://aibooru.online/)

- [Reddit](https://www.reddit.com/r/StableDiffusion/)

### Models

https://civitai.com/
