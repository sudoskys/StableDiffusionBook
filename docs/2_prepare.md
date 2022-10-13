

# 准备

本文档目的是为刚入门接触使用Stable Diffusion绘画的小白一个简易的指南。


## 在社区礼貌提问

[如何提问才是正确的？](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md)

如果你确认有错误的内容或者需要补充的重要内容，可以发 Issue 或 Pr 这个仓库，帮助完成文档，帮到更多人。

欢迎交流，不欢迎伸手党。问问题前请先自己确认文档内没有解释，且你应该明白任何人或社区成员在没有利益关系的情况下，没有义务为你解答问题。


## 我可以参加 Party 吗？

首先，很不幸地，因为需要用到 `CUDA` 加速，所以只有**英伟达显卡**支持良好。（AMD 可以用但速度不行，~当然没显卡也可以用 CPU 花几百倍时间生成~）


**Linux + AMD卡** 请读 [AMD安装指南](https://rentry.org/ayymd-stable-diffustion-v1_4-guide) 和 [AMD安装WebUi指北](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)


[对于支持AMD Gpu方案相关讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1046)


## 目前的五种方案

| 名称             | 需求                      | 效果                                                                  |
|------------------|---------------------------|-----------------------------------------------------------------------|
| SdWebui(4GB Leak模型) | 2GB显存起步               | 4GB显存默认出图速度 10s,单次最大出 920x920                            |
| SdWebui(7GB Leak模型) | 8GB显存                   | 和4GB效果相差不大，但是数据中带有最后一次训练的权重，所以耗费较多显存 |
| Naifu(4GB Leak模型)   | 最低8GB显存&8GB显存       | 和官方接近                                                            |
| Naifu(7GB Leak模型)   | 最低8GB显存(向上浮动10GB) | 和官方接近                                                            |
| 官方后端         | 80GB&服务器系统           | 和官方99.9相似                                                        |


NAI 原版网页UI+后端部署需要准备一台拥有12GB以上显存的 *Linux系统* 的GPU服务器。个人使用 4GB 精简 Leak 模型更加划算。NAI官方使用的是全量模型，但是这需要大量的显存，~你需要淘宝售价 7W 的 A100 显卡~

SdWebui 是一个可以使用 模型 生产图片的**框架**。

NAI 是一个在线**服务**。



## 新闻


!!! tip
    目前stable-diffusion-webui迭代非常快，每天都会有大量更新，因此建议每天都拉取最新代码。

以下是新闻和测试内容。

-------
- 10/13 更新

automatic1111 的 repo 从 r/stablediffusion 的固定指南中删除

-------
- 10/12 更新

StabilityAI 从现在开始只发布 SFW 模型，from [Here](https://www.reddit.com/r/StableDiffusion/comments/y2dink/qa_with_emad_mostaque_formatted_transcript_with/is32y1d/)

-------
- 10/11 更新
新版本 Webui 增加了 Eta noise seed delta ，设为 31337 后会贴近NAI官方

现在可以训练 hypernetworks

-------
- 10/10 更新
测试发现,新版本 Webui 优化显存占用(20xx—>10xx)，关闭浏览器和TG的硬件加速后，即使是4GB的  RTX2050 也可以启动 --medvram 模式！而且很快.

谈话
https://github.com/brycedrennan/imaginAIry/blob/master/docs/emad-qa-2020-10-10.md

--------
- 背景故事

NovelAI 是一个使用 AI 生成故事文本和**通过描述文字生成图片**的服务，而 Stable Diffusion 是由 Stability AI 发布的透过文字等生成图片的模型。

开发者 AUTOMATIC 是 Stable-Diffusion-Webui 的主要开发者：此项目可以用于在使用 Stable Diffusion 等模型时调整参数，极大地方便了尤其是没有计算机背景或 AI/ML 背景的模型用户。

前几日 NovelAI [称其部分软件和源码泄露](https://old.reddit.com/r/NovelAi/comments/xydjc6/)。在模型泄露后， AUTOMATIC 在 Webui 项目中添加了对 **Hypernetwork** 模型的支持，使得此项目可以和泄露模型共用。

此 Reddit 贴称， Stability AI 创始人 Emad Mostaque 谴责 AUTOMATIC 此行为，并称后者窃取了代码； 

AUTOMATIC 则称自己没有窃取代码，并解释说他编写的代码是基于很久以前已经完成的研究和开发，并且是开源的。有问题的函数于 2021 年 12 月 21 日在 [此处](https://github.com/CompVis/latent-diffusion/commit/e66308c7f2e64cb581c6d27ab6fbeb846828253b) 发布，并称反倒是 NovelAI 使用了自己的代码(https://imgur.com/a/Z2QsOEw)。


Stable Diffusion 社群管理员后又要求 AUTOMATIC 移除项目中的 Hypernetwork 支持，称 NovelAI 核心开发者认为相关代码必然与泄露源码有联系；但被以代码原创且 Hypernetwork **并非泄露模型独创**的理由回绝。
 
而后， AUTOMATIC 被从 Stable Diffusion 的（Slack?）社群服务器中封禁。

来自 https://rentry.org/sd-tldr




## 声明

本文档不托管任何模型及相关源代码，本网站托管方不提供模型的下载，不直接分发任何不合法模型文件。不鼓励使用开源项目盈利的行为。不赞成使用泄漏模型进行商业竞争。

如果本文档外链的内容中有不合适的内容，与本文档无关。
