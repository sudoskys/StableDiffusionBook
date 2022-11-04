# 准备

本文档目的是为了总结社区讨论成果，为刚入门接触使用Stable Diffusion绘画的小白编写一个自助指南，从 WebUi 入手接触 Ai绘画，并学习相关知识。


## 在社区礼貌提问

[如何提问才是正确的？](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md)

如果你确认有错误的内容或者需要补充的重要内容，可以发 **Issue** 或 提交 **Pr** 到此仓库，帮助完成文档，帮到更多人。

欢迎交流，不欢迎伸手党。问问题前请先自己确认文档内没有解释，且你应该明白任何人或社区成员在没有利益关系的情况下，没有义务为你解答问题。

如果看到不认识的章节，有可能是你的版本太低了。


## 我可以参加 Party 吗？

首先，很不幸地，因为需要用到 `CUDA` 加速，所以只有**英伟达显卡**支持良好。（AMD 可以用但速度明显慢于英伟达显卡，~当然没显卡也可以用 CPU 花几百倍时间生成~）


**Linux + AMD卡** 请读 [AMD安装指南](https://rentry.org/ayymd-stable-diffustion-v1_4-guide) 和 [AMD安装WebUi指北](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)


[对于支持AMD GPU方案相关讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1046)

!!! danger "显卡保修"
    显卡厂家对于深度学习卡的保修政策等同于矿卡
    过度玩耍(比如连续3天出图)，显卡会有坏掉的风险

[各种显卡的稳定扩散性能测试报告](https://docs.google.com/spreadsheets/d/1Zlv4UFiciSgmJZncCujuXKHwc4BcxbjbSBg71-SdeNk/edit#gid=0)


## 目前的五种方案

| 名称             | 需求                      | 效果                                                                  |
|------------------|---------------------------|-----------------------------------------------------------------------|
| SdWebui(4GB Leak模型) | 最低3GB显存               | 4GB显存被512x512吃满，默认出图速度 17s                            |
| SdWebui(7GB Leak模型) | 最低8GB显存                   | 和4GB效果相差不大，但是数据中带有最后一次训练的权重，所以耗费较多显存 |
| Naifu(4GB Leak模型)   | 最低8GB显存&8GB显存       | 和官方接近                                                            |
| Naifu(7GB Leak模型)   | 最低8GB显存(向上浮动10GB) | 和官方接近                                                            |
| 官方后端         | 16GBfp16/24GBfp32 & 服务器系统           | 和官方99.9相似                                                        |


NAI 原版网页UI+后端部署需要准备一台拥有12GB以上显存的 *Linux系统* 的GPU服务器。个人使用 4GB 精简 Leak 模型更加划算。NAI官方使用的是全量模型，但是这需要大量的显存，~你需要淘宝售价 7W 的 A100 显卡~

SDWebUi 是一个可以使用 模型 生产图片的**框架**。

NAI 是一个在线**服务**。

## 二次元之外

如果你想知道没有 NAI模型 的 stable-diffustion，请看 [这个合集](https://space.bilibili.com/250989068/channel/collectiondetail?sid=660352)。

顺便贴一个 [最近发布的1.5模型](https://huggingface.co/runwayml/stable-diffusion-v1-5) 和 [稳定体积小的1.4模型](https://huggingface.co/CompVis/stable-diffusion-v1-4)

