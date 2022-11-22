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

[Stable diffusion performance test reports](https://docs.google.com/spreadsheets/d/1Zlv4UFiciSgmJZncCujuXKHwc4BcxbjbSBg71-SdeNk/edit#gid=0)

## Current five options

| Name | Requirements | Effectiveness |
|------------------|---------------------------|-----------------------------------------------------------------------|
| SdWebui(4GB Leak model) | At least 3GB VRAM | 4GB VRAM generates 512x512 size image in 17s |
| SdWebui(7GB Leak model) | At least 8GB VRAM | Not much difference from 4GB, but the data has the last training weights, so it consumes more VRAM |
| Naifu(4GB Leak model) | Minimum 8GB VRAM & 8GB VRAM | Close to official |
| Naifu(7GB Leak model) | Minimum 8GB VRAM (10GB upward) | Close to official |
| Official Backend | 16GBfp16/24GBfp32 & Server System | 99.9 similar to official |

The original NAI web UI + backend deployment requires a GPU server with 12GB or more of VRAM on a *Linux system*. NAI officially uses the full model, but it requires a lot of VRAM, so you'll need a A100 graphics card(10000$) from Amazon.

SDWebUi is a **framework** that can use models to produce images.

NAI is an online **service**.

## Don't want to generate anime pictures?

If you want to get models that don't generate anime girls, please see [here](https://space.bilibili.com/250989068/channel/collectiondetail?sid=660352).

By the way post a [recently released 1.5 model](https://huggingface.co/runwayml/stable-diffusion-v1-5) and [stable small size 1.4 model](https://huggingface.co/CompVis/stable-diffusion-v1-4)
