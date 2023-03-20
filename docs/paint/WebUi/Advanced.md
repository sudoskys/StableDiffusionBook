# Img2*

此页面介绍 Img2Img 的基本操作。如果对参数有疑惑请查看“模型调试”页面的 WebUi 参数介绍。

如果你在寻找一些应用例子，你应该看 “实战指南”页面。

!!! tip
    请阅读前面章节的模型进阶 1, 了解具体的 Img2Img 和 inpaint 介绍操作。

## 基本参数

`cfg scale` 符合 prompt 的程度，值越高越会字面看待 prompt, 低则给模型较大的发挥空间，但是实际模型表现上来看 CFG scale 低 (6-8) 饱和度低，偏杂乱，高 (18-22) 则饱和度偏高，风格更强烈。

> 过高的 CFG 会引起颜色失真，CFG 应该在 5-15 之间

`denoise strength` img2img 专属参数，从 0 到 1 取值，值越高 AI 对原图的参考程度就越低 （同时增加迭代次数）, 个人喜欢低 CFG 高 denoise 重绘图，高 CFG 低 denoise 改细节。

如果对其他参数有疑惑(如 `seed` )请查看模型调试页面的 WebUi 参数介绍。

## Img2Txt

生成按钮下有一个 `Interrogate CLIP`，点击后会下载 `CLIP`，用于推理当前图片框内图片的 Prompt 并填充到提示词。

CLIP 询问器有两个部分：一个是 BLIP 模型，它承担解码的功能，从图片中推理文本描述。另一种是 CLIP 模型，它会把文本转化成向量（在模型的推理过程中）。

!!! tip
    Img2Txt 使用到的模型为 [model_base_caption_capfilt_large.pth](https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_caption_capfilt_large.pth)

    大小为 `855MB`

## Img2Img

一般我们有两种途径对图像进行修复：**PS 和 InPaint**，使用方法也十分多样。

WebUi 使用 `--gradio-img2img-tool color-sketch` 启动会带入一个插件，可以对图片进行颜色涂抹（不是 Inpaint 涂蒙版用的，是修改上传图片用的）

!!! tip "不同之处"
    PS 重新绘画投入 Img2Img 的话，会导致画风的变动，而 Inpaint 就不会。

### 调整大小

*Just resize* 仅仅调整大小 - 简单地将源图像的大小调整到目标分辨率，导致不正确的长宽比。

*Crop and resize* 裁剪和调整大小 - 在保持长宽比的情况下调整源图像的大小，使其完全占据目标分辨率，并裁剪伸出的部分。

*Resize and fill* 调整大小和填充 - 调整源图像的大小，保留长宽比，使其完全适合目标分辨率，并通过源图像的行/列来填充空白空间

###  Img2Img 三渲二

调整 3D 模型骨架比寻找样图更容易。

可以结合 **3D 建模** 摆 Pose, 可以使用 MMD 相关软件。

如果是真人图片，需要适当提高 `CFG Scale` 相似度，结合提示词一起生成。降噪 `denoising` 越高，相关性越低。

推荐使用 [DAZ](https://www.daz3d.com/get_studio) 或者 [blender](https://www.blender.org/) 或者 Unity ，在对 3D 模型的测试中，**色彩主要影响 AI 的绘画效果**，所以你的模型需要有纹理。

如果你使用 blender ，你可以使用 [这个视频](https://youtu.be/MClbPwu-75o) 分享的 [模型娃娃](https://www.artstation.com/marketplace/p/VOAyv/stable-diffusion-3d-posable-manekin-doll?utm_source=artstation&utm_medium=referral&utm_campaign=homepage&utm_term=marketplace)

### Img2Img **Outpainting 外部修补**

Outpainting 扩展原始图像并修复创建的空白空间。
您可以在底部的 img2img 选项卡中找到该功能，在 Script -> Poor man's outpainting 下。

```
Outpainting, unlike normal image generation, seems to profit very much from large step count. A recipe for a good outpainting is a good prompt that matches the picture, sliders for denoising and CFG scale set to max, and step count of 50 to 100 with Euler ancestral or DPM2 ancestral samplers.
```

### Img2Img **Inpainting 修补**

在 Inpainting 选项卡中，在图像的一部分上绘制蒙版，该部分将被重画。

`Masked content` 设置确定在修复之前放置到遮罩区域中的内容，一般选 `original`，可以保持潜在空间一致性，如果你不希望修补内容继承原来的色彩分布，选 `fill` 就是使用图片的大部分底色，选 `latent noise` 可以获得随机色彩点阵图（使生成内容脱离关联）。

它们的效果如下：

| 示意操作                                                                                                         | fill                                                                                                         | original                                                                                                         | latent noise                                                                                                         | latent nothing                                                                                                         |
|--------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-mask.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-fill.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-original.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-noise.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-nothing.png) |

`mask` 横条决定了模糊程度。original 是 `原图`，fill 是 `填充底色`，`fill` 要更多 step 才能消除不自然感。

`Inpaint at full resolution` 即全分辨率修复。默认情况下 Inpaint 会将生成的图像大小 **整体** 调整为 *UI 中指定的目标分辨率*。启用 `Inpaint at full resolution` 后，**仅调整遮罩区域** 的大小，并在处理后将其 **粘贴回** 原始图片。这允许你处理大尺寸图片，并允许以更大的分辨率渲染修复对象。

目前有几种方法进行重绘制操作：

- 在网络编辑器中自己绘制蒙版（`Inpaint masked ` 指重画涂鸦区域，`Inpaint not masked` 指重画涂鸦之外的区域）

- 在外部编辑器中擦除部分图片并上传透明图片。透明区域会成为蒙版的一部分。Tips：某些编辑器默认将完全透明的区域保存为黑色。

- 将模式（图片右下角）更改为 "Upload mask" 并为蒙版处理为单独的黑白图像（白色部分会被 inpaint)。

如果 `inpaint at full resolution` 出现黑块，可能是 RAM 不足，尝试卸载 vae.

![result](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting.png)

[开源调研-AI 绘画全参数讲解-002img2img 图像到图像](https://www.bilibili.com/video/BV1HK411Q7uk/?zw)
<!--
<iframe src="//player.bilibili.com/player.html?aid=474043788&bvid=BV1HK411Q7uk&cid=860273094&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

通过这种方法，我们可以更改角色衣物风格或者其他任何细节。

[如何教会 Ai 画手](https://www.bilibili.com/video/av559044202/?zw)
<!--
<iframe src="//player.bilibili.com/player.html?aid=559044202&cid=859852841&page=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

### Img2Img Loopback 回环生成

在 img2img 中设置 loopback 脚本，它允许自动将输出图像作为下一批的 Batch 提供，相当于保存输出图像，并用它替换输入图像。

Batch 数设置控制获得多少次迭代

通常，在执行此操作时，您会自己为下一次迭代选择许多图像中的一个，因此此功能的有用性可能值得怀疑，但反正我已经设法获得了一些我无法获得的非常好的输出。

### Img2Img 让低显存生成大分辨率图片

在前面提到了，如果遇到生成鬼图或者 低显存生产高分辨率图片 可以采用的 Img2Img 画质提升脚本。

其实我 **强烈推荐** 你使用 Extras 的功能对低分辨率进行重放，效果不错且体验良好！

#### 脚本

但是如果你想使用脚本提供的分辨率增强，这里有 Img2Img 的具体流程

1. 使用 `--medvram` 或者 `--lowvram` 参数启动 webui

2. 选择较小分辨率生成图片。记住你生成图片的分辨率。生成完毕之后，复制图片的 `Seed`

3. 生成完毕后，先查看图片效果是否满意。如果满意，直接将图片送进 Img2img。（点击 `Send to img2img`）

4. 在 img2img 界面底部，有一个 `Script` 选项。将 `Script` 选为 `SD Upscale`，里面的 Tile overlap 尽量调小

5. 一般送入 Img2img 的图，输入框自动填充原提示词。如果你发现 prompt 有变动，请手动填充

6. 选择合适的 `Sampling Steps` 和 `Sampling method`

7. 确认你的 `Width` 和 `Height` 与 **原图** 一致

8. 将第 2 步复制的 Seed 填入 img2img 的 Seed 里并生成

这里的 Width 和 Height 是超分时 img2img 的图片大小，如果不等会导致出现重叠问题

SD Upscale 选项在 Img2Img 的 Script 栏目中，主要作用是提升分辨率。

[脚本解决方案来源于此](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#%E9%AB%98%E5%88%86%E8%BE%A8%E7%8E%87%E4%B8%8B%E5%87%BA%E6%80%AA%E5%9B%BE)





[^4]:[Paper 朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD 金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[Style train](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^10]:[角色与画风 tag 训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI 美学权重](https://www.bilibili.com/read/cv19102552)
