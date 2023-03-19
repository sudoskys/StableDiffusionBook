# 准备

本文档目的是为了总结中文社区讨论成果，为刚入门接触使用 Stable Diffusion 绘画的小白编写一个自助指南，从 WebUi 入手接触 Ai 绘画，并学习相关知识。

## 在社区礼貌提问

[如何提问才是正确的？](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md)

如果你确认有错误的内容或者需要补充的重要内容，可以发 **Issue** 或 提交 **Pr** 到此仓库，帮助完成文档，帮到更多人。

欢迎交流，问问题请附上 Log 全部记录文件。问问题前请先自己确认文档内没有解释，且你应该明白任何人或社区成员在没有利益关系的情况下，没有义务为你解答问题。

如果看到不认识的章节，有可能是你的 UI 版本太低了或不支持此功能。

## 我可以参加 Party 吗？

首先，很不幸地，因为需要用到 `CUDA` 加速，所以只有 **英伟达显卡** 支持良好。（AMD 可以用但速度明显慢于英伟达显卡，~当然没显卡也可以用 CPU 花几百倍时间生成~）

**Linux + AMD 卡** 请读 [AMD 安装指南](https://rentry.org/ayymd-stable-diffustion-v1_4-guide) 和 [AMD 安装 WebUi 指北](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)

[对于支持 AMD GPU 方案相关讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1046)

!!! danger "显卡保修"
    显卡厂家对于深度学习卡的保修政策等同于矿卡。
    过度玩耍（比如连续 3 天出图），显卡会有坏掉的风险。

[各种显卡的稳定扩散性能测试报告](https://docs.google.com/spreadsheets/d/1Zlv4UFiciSgmJZncCujuXKHwc4BcxbjbSBg71-SdeNk/edit#gid=0)

## 模型选择

生成图片需要模型+UI, 你需要先下载一个模型才能使用。教程测试采用来自中国的第三方模型，而不是实景模型。

工欲善其事，必先利其器。市面上常见的模型往往会上传到这两个模型站：

- [civitai](https://civitai.com/)

- [huggingface](https://huggingface.co/)

**动漫风格**

值得一提的是，截至2023年3月，Lora 的兴起使得传统 DreamBooth 等 Merge （融合）模型风光不再，具有热插拔能力的 Lora 和 Hypernet 使得 画风 的灵活性大大上升了。尽管如此，一个优秀的底模仍是一件重要的事。下面是一些值得参考的优秀模型：

- [Anything V3.0](https://huggingface.co/andite/anything-v4.0/tree/main)。这是一个融合模型，大概是大杂烩，但效果还是蛮不错的。

- [OrangeMixs](https://huggingface.co/WarriorMama777/OrangeMixs)。这也是一个融合模型，通过 [Merge block weighted](https://github.com/bbc-mc/sdweb-merge-block-weighted-gui) 进行了融合，效果非常不错。作者提供了多种版本，其中比较著名的 深渊橘（[AOM2](https://huggingface.co/WarriorMama777/OrangeMixs/tree/main/Models/AbyssOrangeMix2)/[AOM3](https://huggingface.co/WarriorMama777/OrangeMixs/tree/main/Models/AbyssOrangeMix3)）是将 现实风格 和 动漫风格 融合的产物。

**现实风格**

- [Stable Diffusion 2.0 模型](https://stability.ai/blog/stable-diffusion-v2-release)

- [Stable Diffusion 1.5 模型](https://huggingface.co/runwayml/stable-diffusion-v1-5)

- [稳定体积小的 Stable Diffusion 1.4 模型](https://huggingface.co/CompVis/stable-diffusion-v1-4)

- [chilloutmix](https://civitai.com/models/6424/chilloutmix)

### 数据集来源交代

Stable Diffusion Model 系列模型基于 [LAION](https://laion.ai/).

NAI 系列模型基于 [Danbooru](danbooru.donmai.us/) 和 Stable Diffusion Model.

Any 系列模型基于 Merge.

OrangeMixs 系列模型基于 [Merge block weighted](https://github.com/bbc-mc/sdweb-merge-block-weighted-gui).
