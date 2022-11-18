# 魔法进阶

调参基本原理模糊的说是：限定好的数据范围内相似样本越多，越稳定。

!!! tip
    请阅读前面章节的模型进阶1,了解具体的 Img2Img 和 inpaint 介绍操作。


## 提示词原理

### 图像生成器

![jalammar s pic](https://jalammar.github.io/images/stable-diffusion/stable-diffusion-components-and-tensors.png)

information creator 完全在图像信息空间（或潜伏空间）中工作。这一特性使它比以前在像素空间工作的扩散模型更快。在技术上，这个组件是由一个UNet神经网络和一个调度算法组成的。

- Text Encoder

提示词的解析由 Text Encoder/CLIP 处理(token embedding)，这里是提示词转译给AI的关键一步。

ClipText用于文本编码。
输入文本，输出77个标记嵌入向量，每个都有768个维度。


- information creator

UNet + Scheduler在信息（潜在）空间中逐步处理/分散信息。

它输入文本嵌入和一个由噪声组成的起始多维数组（结构化的数字列表，也叫张量），输出一个经过处理的信息阵列。

- Image Decoder

Text Decoder 根据从 information creator 那里获得的信息绘制一幅图画。 它只在过程结束时运行一次以生成最终图像。

Autoencoder Decoder使用处理过的信息阵列绘制最终图像的解码器。输入处理过的信息阵列(dimensions: (4,64,64))，输出结果图像(dimensions: (3, 512, 512)，即(red/green/blue, width, height)。

- CLIP 的工作

![训练图](https://pic3.zhimg.com/80/v2-340920caff256e06c29cff7097e23e62_1440w.jpg)
>CLIP 训练图 from https://bbs.huaweicloud.com/blogs/371319

在使用中，Text Encoder 对输入词汇进行特征提取，与高斯噪声结合输入 information creator ，输出一个经过处理的信息阵列。
每次处理后 图像生成器 都会预测一些像素，然后在循环的生成中，像素相互结合。

处理完一次数组就是完成了一次 step.然后处理完的噪声数组和 token集合再次进行一次 step 生成.

完成所有 step 后，噪声输入Image Decoder，输出成图。

### WebUi 的实现

[WebUi的prompt_parser](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/prompt_parser.py) 通过本地 WebUi 实现了渐变等功能。

WebUi prompt 语法会转换为相应时间的 prompt,然后通过 embedding 交给 Ai 处理。

关于权重的实现：权重增加通常会占一个提示词位。

关于渐变的实现：到了指定 Step ，WebUi 程序会替换对应 提示词，达到渐变效果。

其他以此类推。

整个看下来，原理流程如图 ![prompt_draw](https://user-images.githubusercontent.com/75739606/198675128-c2c849d0-d024-468b-80c4-374f13e933e3.png)
>By RcINS

你可以在 [illustrated-stable-diffusion](https://jalammar.github.io/illustrated-stable-diffusion/) 看到全面的介绍。本节部分内容也是由此翻译。


## Img2Txt

>应用指南请读实战指南。

生成按钮下有一个 `Interrogate CLIP`，点击后会下载 `CLIP`，用于生成当前图片框内图片的 Tag 并填充到提示词。

CLIP 询问器有两个部分：一个是 BLIP 模型，它从图片中创建文本描述。另一种是 CLIP 模型，它会从列表中挑选出与图片相关的几行

!!! tip
    本文件为 [model_base_caption_capfilt_large.pth](https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_caption_capfilt_large.pth)

    大小为855MB


## Img2Img 介绍

一般我们有两种途径对图像进行修复：**PS 和 InPaint**，使用方法也十分多样。

WebUi 使用 `--gradio-img2img-tool color-sketch` 启动会带入一个插件对图片进行颜色涂抹(这里不是 Inpaint)

!!! tip "不同之处"
    PS 重新绘画投入 Img2Img 的话，会导致画风的变动，而 Inpaint 就不会。


### 调整大小


*Just resize* 仅仅调整大小 - 简单地将源图像的大小调整到目标分辨率，导致不正确的长宽比。

*Crop and resize* 裁剪和调整大小 - 在保持长宽比的情况下调整源图像的大小，使其完全占据目标分辨率，并裁剪伸出的部分。

*Resize and fill* 调整大小和填充 - 调整源图像的大小，保留长宽比，使其完全适合目标分辨率，并通过源图像的行/列来填充空白空间



##  Img2Img 三渲二

调整3D模型骨架比寻找样图更容易。

可以结合 **3D建模** 摆 Pose,可以使用 MMD 相关软件。

如果是真人图片，需要适当提高 `CFG Scale` 相似度，结合提示词一起生成。降噪 `denoising` 越高，相关性越低。

推荐使用 [DAZ](https://www.daz3d.com/get_studio) 或者 [blender](https://www.blender.org/) 或者 Unity ，在对 3D 模型的测试中，**色彩主要影响 AI 的绘画效果**，所以你的模型需要有纹理。

如果你使用 blender ，你可以使用 [这个视频](https://youtu.be/MClbPwu-75o) 分享的 [模型娃娃](https://www.artstation.com/marketplace/p/VOAyv/stable-diffusion-3d-posable-manekin-doll?utm_source=artstation&utm_medium=referral&utm_campaign=homepage&utm_term=marketplace)


## Img2Img **Outpainting 外部修补**

Outpainting 扩展原始图像并修复创建的空白空间。
您可以在底部的 img2img 选项卡中找到该功能，在 Script -> Poor man's outpainting 下。

```
Outpainting, unlike normal image generation, seems to profit very much from large step count. A recipe for a good outpainting is a good prompt that matches the picture, sliders for denoising and CFG scale set to max, and step count of 50 to 100 with Euler ancestral or DPM2 ancestral samplers.
```

## Img2Img **Inpainting 修补**

在 Inpainting 选项卡中，在图像的一部分上绘制蒙版，该部分将被重画。

`Masked content` 设置确定在修复之前放置到遮罩区域中的内容，一般选 `original`，可以保持潜在空间一致性，如果你不希望修补内容继承原来的色彩分布，选`fill`就是使用图片的大部分底色，选 `latent noise` 可以获得随机色彩点阵图（使生成内容脱离关联）。

它们的效果如下:

| 示意操作  | fill  | original   | latent noise      | latent nothing       |
|---------------------------|----------------|-----------------------|-------------------------|-----------------------|
| ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-mask.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-fill.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-original.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-noise.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-nothing.png) |

`mask` 横条决定了模糊程度。original 是`原图`，fill 是`填充底色`，`fill` 要更多 step 才能消除不自然感。

`Inpaint at full resolution` 即全分辨率修复。默认情况下 Inpaint 会将生成的图像大小**整体**调整为*UI中指定的目标分辨率*。启用 `Inpaint at full resolution` 后，**仅调整遮罩区域** 的大小，并在处理后将其**粘贴回**原始图片。这允许你处理大尺寸图片，并允许以更大的分辨率渲染修复对象。

目前有几种方法进行重绘制操作：

- 在网络编辑器中自己绘制蒙版（`Inpaint masked `指重画涂鸦区域，`Inpaint not masked` 指重画涂鸦之外的区域）

- 在外部编辑器中擦除部分图片并上传透明图片。透明区域会成为蒙版的一部分。Tips：某些编辑器默认将完全透明的区域保存为黑色。

- 将模式（图片右下角）更改为 "Upload mask" 并为蒙版处理为单独的黑白图像(白色部分会被inpaint)。

如果 `inpaint at full resolution` 出现黑块，可能是RAM不足，尝试卸载 vae.

![result](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting.png)

[开源调研-AI绘画全参数讲解-002img2img图像到图像](https://www.bilibili.com/video/BV1HK411Q7uk/?zw)
<!--
<iframe src="//player.bilibili.com/player.html?aid=474043788&bvid=BV1HK411Q7uk&cid=860273094&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

通过这种方法，我们可以更改角色衣物风格或者其他任何细节。

[如何教会Ai画手](https://www.bilibili.com/video/av559044202/?zw)
<!--
<iframe src="//player.bilibili.com/player.html?aid=559044202&cid=859852841&page=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

## Img2Img Loopback 回环生成

在 img2img 中设置loopback脚本，它允许自动将输出图像作为下一批的Batch提供，相当于保存输出图像，并用它替换输入图像。

Batch 数设置控制获得多少次迭代

通常，在执行此操作时，您会自己为下一次迭代选择许多图像中的一个，因此此功能的有用性可能值得怀疑，但反正我已经设法获得了一些我无法获得的非常好的输出。


## Img2Img 让低显存生成大分辨率图片

在前面提到了，如果遇到生成鬼图或者 低显存生产高分辨率图片 可以采用的 Img2Img 画质提升脚本。

其实我**强烈推荐**你使用 Extras 的功能对低分辨率进行重放，效果不错的，且体验良好！


### 脚本

但是如果你想使用脚本提供的分辨率增强，这里有 Img2Img 的具体流程

1. 使用 `--medvram` 或者 `--lowvram` 参数启动webui

2. 选择较小分辨率生成图片。记住你生成图片的分辨率。生成完毕之后，复制图片的 `Seed`

3. 生成完毕后，先查看图片效果是否满意。如果满意，直接将图片送进Img2img。（点击 `Send to img2img`）

4. 在img2img界面底部，有一个 `Script` 选项。将 `Script` 选为 `SD Upscale`，里面的 Tile overlap 尽量调小

5. 一般送入 Img2img 的图，输入框自动填充原提示词。如果你发现prompt有变动，请手动填充

6. 选择合适的 `Sampling Steps` 和 `Sampling method`

7. 确认你的 `Width` 和 `Height` 与**原图**一致

8. 将第 2 步复制的 Seed 填入img2img的 Seed 里并生成

这里的 Width 和 Height 是超分时 img2img 的图片大小，如果不等会导致出现重叠问题

SD Upscale 选项在 Img2Img 的 Script 栏目中，主要作用是提升分辨率。


[脚本解决方案来源于此](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#%E9%AB%98%E5%88%86%E8%BE%A8%E7%8E%87%E4%B8%8B%E5%87%BA%E6%80%AA%E5%9B%BE)

### 超分图像 extras

`webui` extras 页有一个自带的超分功能，可以使用 `ESRGAN_4x`模型

当然 `realesrgan` 或者 `realcugan` 也可以。

!!! tip "相关模型"

    文件统一下载到 `SDwebUI文件夹\models` 下

    [LDSR](https://heibox.uni-heidelberg.de/f/578df07c8fc04ffbadf3/?dl=1)，文件大小为1.9GB

    [BSGRAN 4x](https://github.com/cszn/KAIR/releases/download/v1.0/BSRGAN.pth) ，文件大小为63.9M

    [ESRGAN_4x](https://github.com/cszn/KAIR/releases/download/v1.0/ESRGAN.pth)，文件大小为63.8MB

    [ScuNET GAN/PSNR](https://github.com/cszn/KAIR/releases/download/v1.0/scunet_color_real_gan.pth" to D:\stable-diffusio\models\ScuNET\ScuNET.pth)，文件大小为68.6MB

    [SwinIR 4x](https://github.com/JingyunLiang/SwinIR/releases/download/v0.0/003_realSR_BSRGAN_DFOWMFC_s64w8_SwinIR-L_x4_GAN.pth)，文件大小为136MB

**Highres Fix/超分应该使用什么 Upscaler？**

`SD Upscaler` 在注重细节的同时还提升分辨率。

曾经有段时间，`LSDR` 被认为是最好的。有些人喜欢 swinir，有些喜欢`esrgan4x`，`ymmv`，推荐使用 `ESRGAN_4x`

如果你要搞二次元，推荐使用[realcugan](https://github.com/bilibili/ailab/tree/main/Real-CUGAN)


## 图像去噪

推荐使用 [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) 降噪。

![效果](https://raw.githubusercontent.com/xinntao/Real-ESRGAN/master/assets/teaser.jpg)
>效果图




## **渐变提示词**

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#prompt-editing

允许您开始对一张图片进行采样，但在中间切换到其他图片。基本语法是：

```
[from:to:when]
```

其中from和to是任意文本，并且when是一个数字，用于定义应在采样周期多长时间内进行切换。越晚，模型绘制to文本代替from文本的能力就越小。如果when是介于 0 和 1 之间的数字，则它是进行切换之后的步数的一小部分。如果它是一个大于零的整数，那么这只是进行切换的步骤。

将一个提示编辑嵌套在另一个提示中不起作用。

**使用方法**

[to:when] 在固定数量的step后添加to到提示 ( when)

[from::when] 在固定数量的step后从提示中删除from( when)

例子： a [fantasy:cyberpunk:16] landscape

开始时，模型将绘制a fantasy landscape。

在第 16 步之后，它将切换到绘图a `cyberpunk landscape`，从幻想停止的地方继续。

比如 [male:female:0.0], 意味着你开始时就要求画一个女性。

## 扩展模型/DLC

见 `训练模型` 的详细内容。




## 提示词速查

**[手抄本法术书](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/edit)**

**[Danbooru全部Tag列表](https://gelbooru.com/index.php?page=tags&s=list)**

**[参数法术全典](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml#)**

**[Tag在线协作](https://docs.google.com/spreadsheets/d/1zij5OzCZIaQuhAbiSayhFznjgJ3rwbaNwnUnaUMxyTQ/edit?usp=drivesdk)**

**[NSFWTag](https://github.com/scooderic/exhentai-tags-chinese-translation)**

**[Ai艺术家文档](https://docs.google.com/spreadsheets/d/1SRqJ7F_6yHVSOeCi3U82aA448TqEGrUlRrLLZ51abLg/htmlview#)**

**[Novelai 关键词组合器](https://www.bilibili.com/read/cv19023021)**


## 调参工程师

[https://github.com/Maks-s/sd-akashic](https://github.com/Maks-s/sd-akashic)

[https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)



[^4]:[Paper朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)
