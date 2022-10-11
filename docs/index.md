# AiDraw

[Stable Diffusion](https://github.com/AUTOMATIC1111/stable-diffusion-webui) web UI是一个基于 Gradio 库的 Stable Diffusion 浏览器界面。

[NovelAI](https://novelai.net/) 是一项每月订阅服务，用于 AI 辅助创作、讲故事、虚拟陪伴，或者只是供您想象的 GPT 驱动的沙盒。

本文档围绕以上项目展开叙述....


!!! tip "术语"
    NV 指 NovelAI

    SD 指 Stable Diffusion web 

    魔杖 指 Stable Diffusion web 

    魔法 指 调参方法
    
    令牌 指 参数

## 提问的艺术

问问题先自己查（至少遇到英文要翻译一下吧！），任何人或社区成员没有义务为你解答，你又不给钱。

[如何提问才是正确的？](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md)

**如果你确认有错误的内容或者需要补充的重要内容，可以发 Issue 或 Pr 这个仓库，帮助完成文档，帮到更多人。**

欢迎交流，不欢迎伸手党。


## 我可以参加 Party 吗？

首先，很不幸地，因为需要用到 CUDA ，所以只有**英伟达显卡**才可用。（当然也可以用 CPU 花超长时间算）

**Linux + AMD卡** 请读 [这里](https://rentry.org/ayymd-stable-diffustion-v1_4-guide) 和 [WebUiWiki对AMD安装的教程](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)

[对于支持AMD Gpu相关讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1046)


**WebUi对机器的要求**

N卡显存 > 2GB ，4GB 或 6GB 是最低正常体验，均要求内存大于 4GB

**Nv对机器的要求**

NV 原版网页UI+后端部署需要准备一台拥有12GB以上显存的 *Linux系统* 的GPU服务器。

NV 精简版 **naifu** 至少需要8GB内存，8GB显存才能出图。


## Stable Diffusion 和 NV 关系？我该怎么办？

因为泄漏事件，NV使用数千万Danbooru（图站）图片训练的模型被泄漏了两次，而后来 WebUI ——一个通用的生成框架对模型所需要的Hypernetwork 进行了支持，所以可以 使用 WebUi（简称SD）装载 NV 的模型。


!!! info "**事件报告**"
    泄露 Part 1 —— 包含生产模型，程序 —— 53.66 GB，其中模型相关需要下载有 7GB 和 4GB （7GB删除除EMA之外内容）两种。


    泄露 Part 2 —— 包含历史测试代码和模型，程序 —— 124.54 GB，模型没有改动。

    使用 4GB 精简更加划算。NV使用的是 全量，但是这需要大量的显存。



## 新闻


!!! tip
    目前stable-diffusion-webui迭代非常快，每天都会有大量更新，因此建议每天都拉取最新代码。

以下是新闻和测试内容。

-------

- 10/11 更新
新版本 Webui 增加了 Eta noise seed delta ，设为 31337 后会贴近NV官方

-------

- 10/10 更新
测试发现,新版本 Webui 优化显存占用(20xx—>10xx)，关闭浏览器和TG的硬件加速后，即使是4GB的  RTX2050 也可以启动 --medvram 模式！而且很快.


--------


- 背景故事

NovelAI 是一个使用 AI 生成故事文本和**通过描述文字生成图片**的服务，而 Stable Diffusion 是由 Stability AI 发布的透过文字等生成图片的模型。

开发者 AUTOMATIC 是 Stable-Diffusion-Webui 的主要开发者：此项目可以用于在使用 Stable Diffusion 等模型时调整参数，极大地方便了尤其是没有计算机背景或 AI/ML 背景的模型用户。

前几日 NovelAI [称其部分软件和源码泄露](https://old.reddit.com/r/NovelAi/comments/xydjc6/)。在模型泄露后， AUTOMATIC 在 Webui 项目中添加了对 **Hypernetwork** 模型的支持，使得此项目可以和泄露模型共用。

此 Reddit 贴称， Stability AI 创始人 Emad Mostaque 谴责 AUTOMATIC 此行为，并称后者窃取了代码； AUTOMATIC 则称自己没有窃取代码，并称反倒是 NovelAI 使用了自己的代码。

Stable Diffusion 社群管理员后又要求 AUTOMATIC 移除项目中的 Hypernetwork 支持，称 NovelAI 核心开发者认为相关代码必然与泄露源码有联系；但被以代码原创且 Hypernetwork **并非泄露模型独创**的理由回绝。
 
而后， AUTOMATIC 被从 Stable Diffusion 的（Slack?）社群服务器中封禁。
