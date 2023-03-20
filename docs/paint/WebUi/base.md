# Txt2Img

此页面为 WebUi 的 Txt2Img 的基本指南，如果你想学习如何书写提示词，请读 `Prompt Editing` 相关的内容。

如果你在寻找一些应用例子，你应该看 “实战指南”页面。

## 基本参数

`cfg scale` 符合 prompt 的程度，值越高越会字面看待 prompt, 低则给模型较大的发挥空间，但是实际模型表现上来看 CFG scale 低 (
6-8) 饱和度低，偏线稿，偏杂乱，高 (18-22) 则饱和度偏高，偏 CG 风格。

> 过高的 CFG 会引起颜色失真，CFG 应该在 5-15 之间

`denoise strength` img2img 专属参数，从 0 到 1 取值，值越高 AI 对原图的参考程度就越低 （同时增加迭代次数）, 个人喜欢低 CFG 高
denoise 重绘图，高 CFG 低 denoise 改细节。

如果对其他参数有疑惑（如 `seed` ) 请查看模型调试页面的 WebUi 参数介绍。

## 消极提示词

WebUi(SD) 网页应用会在生成时 **避免生成消极提示词有关的内容**。

消极提示是一种使用稳定扩散的方式，允许用户指定他不想看到的东西，而不需要对模型有任何额外的负担或要求。工作方式是在做取样时，使用用户指定的文本而不是空字符串作为 `unconditional_conditioning`。

下面是来自 [txt2img.py](https://github.com/CompVis/stable-diffusion/blob/main/scripts/txt2img.py) 的（简化）代码。

```python3
prompts = ["a castle in a forest"]
# batch_size = 1

c = model.get_learned_conditioning(prompts)
uc = model.get_learned_conditioning(batch_size * [""])

samples_ddim, _ = sampler.sample(conditioning=c, unconditional_conditioning=uc, [...])
```

这就启动了采样器，反复进行以下流程。

```
    对图片进行去噪处理，使其看起来更像你的提示（条件）。
    对图片进行去噪处理，使其看起来更像一个空的提示（无条件条件）。
    观察这两者之间的差异，并利用它来产生一组对噪声图片的改变（不同的采样器对这一部分的处理方式不同）。
```

然后，采样器将查看经过噪声处理的图像与经过噪声处理的图像之间的差异，使其看起来像你的提示（一座城堡），以及经过噪声处理的图像看起来像你的负面提示（颗粒状、雾状），并尝试将最终结果移向前者而远离后者。

- 使用否定提示的样例

```python
prompts = ["a castle in a forest"]
# negative_prompts = ["grainy, fog"]

c = model.get_learned_conditioning(prompts)
uc = model.get_learned_conditioning(negative_prompts)

samples_ddim, _ = sampler.sample(conditioning=c, unconditional_conditioning=uc, [...])
```

比如使用以下提示词拒绝水印和文字内容

```
lowres, bad anatomy, bad hands, text, error, missing fingers,
extra digit, fewer digits, cropped, worst quality, low quality,
normal quality, jpeg artifacts, signature, watermark, username, blurry
```

还如这个例子

```
ugly, fat, obese, chubby, (((deformed))), [blurry], bad anatomy,
disfigured, poorly drawn face, mutation, mutated, (extra_limb),
(ugly), (poorly drawn hands fingers), messy drawing, morbid,
mutilated, tranny, trans, trannsexual, [out of frame], (bad proportions),
(poorly drawn body), (poorly drawn legs), worst quality, low quality,
normal quality, text, censored, gown, latex, pencil
```

## Hi-Res

#TODO

## CFG Scale/Denoising strength 契合度/降噪

**CFG Scale**

`cfg scale` 就是预测时对 prompt 的参考度，越高，预测时对，契合度越高。

**Denoising strength 降低噪声**

`Denoising strength` 决定算法对图像内容的保留程度，可以减少对画风的变得，但也会弱化 img2img 能力。值越高 AI 对原图的参考程度就越低
（同时增加迭代次数）。

对于以图做图来说，低 `denoising` 意味着修正原图，高 `denoising` 就和原图就没有大的相关性了。一般来讲阈值是 0.7 左右，超过
0.7 和原图基本上无关，0.3 以下就是稍微改一些。

实际执行中，具体的执行步骤为 Denoising strength * Sampling Steps.

## 图片 Prompt

程序默认会在图片中加入提示词，参数，模型 元数据信息，对于没有压缩的原图，我们可以将文件拖入 `PNG Info` 选项卡，进行提示词 (
Token) 查看。

或者使用 [在线工具](https://spell.novelai.dev/) 查看它。

## 逆向提示词

这里有一些 **Image 逆向参数服务**，可以从图片中提取相关参数，不一定准确。

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)

## Prompt 搜索引擎

你可以访问以下链接获取一些优秀的参数实例。

- [PIXAI](https://pixai.art/)

- [StableDiffusion_Show](https://t.me/StableDiffusion_Show)

- [cyan_ai_sese](https://t.me/cyan_ai_sese)

- [LEXICA 搜索引擎](https://lexica.art/?q=Miku)

## Batch count&batch size

`batch count` 指定训练几批次图像。

`batchsize` 中文翻译为批次大小（批次尺寸）

简单点说，批量大小将决定我们一次训练的样本数目。 batch_size 将影响到模型的优化程度和速度。

还要注意 `batch size` 是为了在 `内存效率` 和 `内存容量` 之间寻找最佳平衡，因为单个 512x512 图像生成中，GPU
资源没有被充分利用。更大的图像从中获得的收益更少。

!!! info
迭代是重复反馈的动作，神经网络中我们希望通过迭代进行多次的训练以到达所需的目标或结果。
每一次迭代得到的结果都会被作为下一次迭代的初始值。
一个迭代 = 一个正向通过+一个反向通过

## 修复大尺寸画面崩坏

`Highres.fix` 是一个方便的选项，在 `txt2img` 页面勾选此复选框来启用。

默认情况下，`txt2img` 在非常高的分辨率下生成非常混沌的图像。而此插件这使得它可以避免使用小图片的构图，在较低的分辨率下部分渲染你的图片，提高分辨率，然后在高分辨率下添加细节。

### 注意 `尺寸`

比如出图尺寸宽了人可能会多

!!! tip
要匹配好姿势，镜头和人物才不畸形，有时候需要限定量词，多人物时要处理空间关系和 prompt 遮挡优先级。人数-> 人物样貌->
环境样式-> 人物状态

1024 之上的尺寸可能会出现不理想的结果！推荐使用 小尺寸 + 适量提高 Step 步数 + 图片超清分辨率（见进阶）。

## Step 迭代步数

更多的迭代步数可能会有更好的生成效果，**更多细节** 和锐化，但是会导致生成时间变长。而在实际应用中，30 步和 50 步之间的差异几乎无法区分。

太多的迭代步数也可能适得其反，几乎不会有提高。

进行图生图的时候，正常情况下更弱的去噪需要更少的迭代步数（这是工作原理决定的）。你可以在设置里更改设置，让程序确切执行滑块指定的迭代步数。

高阶采样器如 `DPM-Solver++` 更高效，花费的步数也少。

实际推理中，具体的执行步骤为 Denoising strength * Sampling Steps.

## Samplers 采样器

一阶目前好用的有 `eular`，`eular a`，更细腻，和 `Ddim`，标注 `++` 的采样器为高阶求解器，有更少的步数更多的细节，更快的实现。

**新手推荐使用 `eular a`**

`eular a` 富有创造力，不同步数可以生产出不同的图片。
PS：调太高步数 (> 30) 效果不会更好

`DDIM` 收敛快，但效率相对较低，因为需要很多 step 才能获得好的结果，**适合在重绘时候使用**

`LMS` 和 `PLMS` 是 `eular` 的衍生，它们使用一种相关但稍有不同的方法（平均过去的几个步骤以提高准确性）。大概 30 step 可以得到稳定结果

`PLMS` 是一种有效的 LMS（经典方法），可以更好地处理神经网络结构中的奇异性

`DPM2` 是一种神奇的方法，它旨在改进 DDIM，减少步骤以获得良好的结果。它需要每一步运行两次去噪，它的速度大约是 DDIM
的两倍。但是如果你在进行调试提示词的实验，这个采样器效果不怎么样

`Euler` 是最简单的，因此也是最快的之一

高阶采样器为 `DPM-Solver++`，你可以使用 `DPM++ 2S a` 来尝试。实验表明 DPM-Solver++可以 只需 15 到 20 个步骤即可生成高质量的样本，用于引导采样
像素空间和潜在空间 DPM。

- 不同 step 和 采样器 的不同效果

| 预览一                                                                                                                                    | 预览二                                                                                                                                    |
|----------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <img src="https://user-images.githubusercontent.com/22421310/187063145-3d4f16d7-7bd6-4804-be1c-acf228ed2507.jpg" width="400" alt="效果"> | <img src="https://user-images.githubusercontent.com/75739606/197824518-f68188a3-0572-4b52-8fe7-289b6d7b640b.jpg" width="400" alt="效果"> |

![exp](https://user-images.githubusercontent.com/40903705/200149887-935a6f95-0bfa-4f8e-b6b1-0fb0bfe0b39e.jpg)

> from https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/4363

[英文 Wiki 介绍](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

[英文论坛介绍](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)

## 种子调试

实际的种子整数并不重要。它只是初始化一个定义扩散起点的随机数生成器，是一个随机初始值。

一个好的种子可以在各种提示、采样器和 CFG 中执行成分和颜色等内容。但是它现在作用有限。

在相同 Step ，cfg ，Seed, 参数（prompts） 的情况下，生产的图片基本相同！

在同一模型和后端实现中，保持所有参数一致的情况下，相同的种子会产生同样的图片。取决于其他参数。

但是注意，**不同显卡可能会造成预料之外的不同结果**（比如精度这样的参数）

10xx
系列看起来与其他所有卡如此不同，见 [这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3873467)

### Merging Seeds

这项技术由 UI 实现。它是一种从同一提示合并不同种子的方法。

单击种子输入旁边的 `Extra` 按钮开启这项功能。

对初始噪声进行改动，尝试融合不同特征。
