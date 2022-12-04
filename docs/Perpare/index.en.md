# Preparation

The purpose of this document is to summarize the results of the community discussion and to create a self-help guide for people who are new to drawing with Stable Diffusion, starting with WebUi and learning about Ai drawing.

## Asking polite questions in the community

[How To Ask Questions The Smart Way](https://github.com/selfteaching/How-To-Ask-Questions-The-Smart-Way)

If you identify something wrong or something important to add, you can send **Issue** or submit **Pr** to this repository to help complete the documentation and help more people.

Communication is welcome, but no nonsense is welcome. Please make sure yourself that there is no explanation in the documentation before asking a question, and you should understand that no one or community member has an obligation to answer your question without having a stake in it.

If you see a section you don't recognize, chances are your version is too low.

## Can I play this?

First, unfortunately, because it requires `CUDA` acceleration, only **Nvidia graphics cards** are well supported. (AMD works but is significantly slower than Nvidia graphics, ~ of course without a graphics card it can take hundreds of times longer to generate with a CPU ~)

**Linux + AMD** Please Read [AMD Guide](https://rentry.org/ayymd-stable-diffustion-v1_4-guide) 和 [AMD install WebUi Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)

[Discussion about installing on Amd](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1046)

!!! danger "GPU Warranty Policy"
    In mainland China, the warranty policy for damage caused by graphics cards used for deep learning is the same as the warranty for damage caused by mining digital cryptocurrencies.
    
    Overuse will lead to GPU damage

[Stable Diffusion performance test reports](https://docs.google.com/spreadsheets/d/1Zlv4UFiciSgmJZncCujuXKHwc4BcxbjbSBg71-SdeNk/edit#gid=0)

## Model selection

To generate images you need a model + UI, you need to download a model first before you can use it. The tutorial test uses a third party model from China, not a real model.

**Anime carving**

- Copyright Avoidance

**Realistic portrayal**

- [Stable Diffusion 2.0 model](https://stability.ai/blog/stable-diffusion-v2-release)

- [Stable Diffusion 1.5 model](https://huggingface.co/runwayml/stable-diffusion-v1-5)

- [Stable Diffusion 1.4 model with small stable volume](https://huggingface.co/CompVis/stable-diffusion-v1-4)

### Data set accounting

Stable Diffusion based on [LAION](https://laion.ai/).

NAI based on [Danbooru](danbooru.donmai.us/).

## Options

| Name | Requirements | Effects |
|------------------|---------------------------|-----------------------------------------------------------------------|
| SdWebui(4GB Anime model) | Minimum 3GB video memory | 4GB video memory generates 512x512 size images in 17s |
| SdWebui(7GB Anime model) | 8GB minimum | Not much different from 4GB, but the data has the last training weights, so it consumes more memory |
| Naifu(4GB Anime model) | Minimum 8GB video memory & 8GB video memory | Similar results |
| Naifu(7GB Anime model) | Minimum 8GB video memory (10GB floating up) | Similar results|
| Naifu Original Backend | 16GBfp16/24GBfp32 & Server System | Similar results 99.9% |
| [NovelAi](https://novelai.net/) | No requirement, as long as you can open a browser | Save your breath|


Naifu deployments require a GPU server with 12GB or more of video memory on a *Linux system*.

The official model used by Novel is the full model, but it requires a lot of video memory, so you need a 7W A100 graphics card from Taobao.

SDWebUi is a **framework** that can use models to produce images.

NAI is an online **service**.
