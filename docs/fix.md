
# 调参

本节只针对 WebUi 展开教程。请多多关注About页面的社区获取最新进展和新闻。源教程来自：[^2]

# 准备模型

本仓库不提供具体链接（版权警告），可以自行寻找或关注中文社区 t.me@StableDiffusion_CN_WIKI。


[除了NV模型外的其他模型：Stable Diffusion Models](https://rentry.org/sdmodels)

## 如果使用NV模型

你的 models 文件夹应该是这样的

```
./models
├── Codeformer
├── ESRGAN
├── GFPGAN
├── hypernetworks
│   ├── aini.pt
│   ├── anime_2.pt
│   ├── anime_3.pt
│   ├── anime.pt
│   ├── furry_2.pt
│   ├── furry_3.pt
│   ├── furry_kemono.pt
│   ├── furry_protogen.pt
│   ├── furry.pt
│   ├── furry_scalie.pt
│   ├── furry_transformation.pt
│   └── pony.pt
├── LDSR
├── Stable-diffusion
│   ├── final-pruned.ckpt -> novelai 的 model.ckpt (pruned)
│   ├── final-pruned.vae.pt -> novelai 的 animevae.pt
│   ├── final-pruned.yaml -> model.ckpt 同文件夹的 config.yaml
│   └── 其他模型文件.ckpt
└── SwinIR
```

启动 cli 有提示加载就 OK, 去设置选模型那里选喜欢的 `hypernetwork`



## 说明

### 对于 LeakNV 模型的说明


`animesfw-latest`即NV基线模型

`fullhqdone` = `full-latest` = NV全量模型

**4GB版本 or 7GB ？**

*diffusion model* 训练会产生两个模型：当前权重和加权平均后优化的EMA（效果好）。

7GB 的 ckpt 里包含了当前权重和EMA权重，pruned.py 删除了当前权重，留下了 EMA权重并重命名。

所以差别不大

### 关键词权重

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis


a (word) - 将权重提高 1.1 倍
a ((word)) - 将权重提高 1.21 倍（= 1.1 * 1.1）
a [word] - 将权重减少 1.1 倍
a (word:1.5) - 将权重提高 1.5 倍
a (word:0.25) - 将权重减少 4 倍 (= 1 / 0.25)
a \(word\) - 在提示中使用文字 () 字符

使用 ()，可以像这样指定权重：(text:1.4)
如果未指定权重，则假定为 1.1
指定权重仅适用于 () 而不是 []


**NV官方**

在NV官方前端我们使用 `{}` 来指定权重。

### 横条参数说明

`step` 迭代多少次, 取值和 sampling method 有关, DDIM 收敛较快, 见 Sampler vs. Steps Comparison (low to mid step counts) 我用 DDIM 取 20 够了.

`batch count/batch size` 决定生成的图片数量, 显存够就加 batch size, 不够就 batch count, 得到的图片数量是两者之积 (小显存还是只动 count 就好)

`sample method`  我无脑 DDIM, Eula 也挺好用? (带 a 的是 ancestral 的意思, step 增长出图不稳定)

`cfg scale` 符合 prompt 的程度, 值越高越会字面看待 prompt, 低则给模型较大的发挥空间, 但是实际模型表现上来看 cfg scale 低 (6-8) 饱和度低, 偏线稿, 偏杂乱, 高 (18-22) 则饱和度偏高, 偏 CG 风格.

>过高的 CFG 会引起颜色失真，CFG 应该在 5-15 之间

`denoise strength` img2img 专属参数, 从 0 到 1 取值, 值越高 AI 对原图的参考程度就越低 (同时增加迭代次数), 我个人喜欢低 cfg 高 denoise 重绘图, 高 cfg 低 denoise 改细节.

[RedditAbout](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)




### 生成图片发生BUG的自救

#### 16系显卡生成黑/绿图

[Green or Black screen](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs)

启动参数加 `--precision full --no-half`, 要省显存还要加 `--medvram`

#### RuntimeError Sizes of tensors must match
(img2img) 如果你得到RuntimeError: Sizes of tensors must match，你需要改变输入图像的分辨率

#### 彩虹图

如果您的输出是混乱的彩虹混乱，则您的图像分辨率设置得太低

#### 但是高分辨率下出怪图

[读这里](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#%E9%AB%98%E5%88%86%E8%BE%A8%E7%8E%87%E4%B8%8B%E5%87%BA%E6%80%AA%E5%9B%BE)

#### RuntimeError: Unable to find a valid cuDNN algorithm to run convolution

生成报错解释：显存不足

先检查 cuda 是否可用
打开命令窗，输入 python 并分行输入
import torch
print(torch.cuda.is_available())

然后不行就切换 `--lowvram` 低配。

再不行：确保在浏览器中禁用硬件加速，并在出现内存不足错误时关闭任何可能占用 VRAM 的内容。

### 优化

[对NV模型靠近NV效果相关讨论，应该读一读！](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017)

#### 进一步优化

出图后，可以将喜欢的结果从右侧的输出选项卡拖回 img2img 以进行进一步迭代。

#### **对NV模型更改 layers 忽略层数**



在 WebUi 的设置页面把 ignore last layers of clip mode 的横条改成 `2`

```
CLIP is a very advanced neural network that transforms your prompt text into a numerical representation. Neural networks work very well with this numerical representation and that's why devs of SD chose CLIP as one of 3 models involved in stable diffusion's method of producing images. As CLIP is a neural network, it means that it has a lot of layers. Your prompt is digitized in a simple way, and then fed through layers. You get numerical representation of the prompt after the 1st layer, you feed that into the second layer, you feed the result of that into third, etc, until you get to the last layer, and that's the output of CLIP that is used in stable diffusion. This is the slider value of 1. But you can stop early, and use the output of the next to last layer - that's slider value of 2. The earlier you stop, the less layers of neural network have worked on the prompt.

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#ignore-last-layers-of-clip-model
```

#### **模型超参数**

如果你想达到 NovelAi 的效果，需要加 negative prompt（消极令牌）, 加载 hypernetwork （网络）和 vae.

https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1868#discussioncomment-3824077

#### **NV模型的Config.yaml**

NVleak里边有个 config.yaml ， 将其改名为 `模型前缀.yaml` 和模型丢在一起就能加载啦, 效果还是有提升的. 

#### **NV模型消极令牌**

使用以下令牌削除水印和文字内容

```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry
```

from [Here](https://t.me/StableDiffusion_CN/6043)

#### **采样器参数**

https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1162

#### **xformers加速**

加速推理,分辨率越高加速效果越好。

[Windows](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases) (30 系之外要自己编译)

自己编译指路 [wiki/Xformers](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Xformers)

>30 系显卡正常启动 --xformers, 其他显卡 --force-enable-xformers

### 进阶

[英文原版](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

#### **Textual Inversion**

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion

丢到 embeddings 文件夹, prompt 提到你要用的 embedding 的文件名即可。

#### **Loopback回环生成**

在 img2img 中设置loopback脚本，它允许自动将输出图像作为下一批的Batch提供，相当于保存输出图像，并用它替换输入图像。

Batch 数设置控制获得多少次迭代

通常，在执行此操作时，您会自己为下一次迭代选择许多图像中的一个，因此此功能的有用性可能值得怀疑，但反正我已经设法获得了一些我无法获得的非常好的输出。

#### **Prompt matrix参数矩阵**

使用 | 分隔多个Tag，程序将为它们的每个组合生成一个图像。 例如，如果使用 `a busy city street in a modern city|illustration|cinematic lighting` ，则可能有四种组合（始终保留提示的第一部分）：

- a busy city street in a modern city
- a busy city street in a modern city, illustration
- a busy city street in a modern city, cinematic lighting
- a busy city street in a modern city, illustration, cinematic lighting

#### **Outpainting外部修补**

Outpainting 扩展原始图像并修复创建的空白空间。
您可以在底部的 img2img 选项卡中找到该功能，在 Script -> Poor man's outpainting 下。

```
Outpainting, unlike normal image generation, seems to profit very much from large step count. A recipe for a good outpainting is a good prompt that matches the picture, sliders for denoising and CFG scale set to max, and step count of 50 to 100 with Euler ancestral or DPM2 ancestral samplers.
```

#### **Inpainting修补**

在 img2img 选项卡中，在图像的一部分上绘制蒙版，该部分将被修复。

![result](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting.png)

选项：
- 在网络编辑器中自己绘制蒙版。
- 在外部编辑器中擦除部分图片并上传透明图片。 任何稍微透明的区域都将成为蒙版的一部分。 请注意，某些编辑器默认将完全透明的区域保存为黑色。

- 将模式（图片右下角）更改为"Upload mask"并为蒙版选择单独的黑白图像(white=inpaint)。

#### **全分辨率修复！**
https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#inpaint-at-full-resolution


#### **Variations种子变化**

Variation strength slider 和 Variation seed field允许您指定现有图片应更改多少以使其看起来不同。
在最大强度下，您将获得带有变异种子的图片，至少 - 带有原始种子的图片（使用先前采样器时除外）。

#### **Styles风格模板**

“Save prompt as style” 按钮将当前的提示写入 styles.csv，该文件包含样式集合

提示右侧的下拉框将允许您从以前保存的样式中选择任何样式，并自动将其**附加**到输入中

要删除样式，请从 styles.csv 中手动将其删除并重新启动程序。

#### **Clip**

CLIP 可以从图像中提取令牌。

令牌提示 不会让你重现这个图像（有时甚至不会很接近），但它可能有用。

默认情况下，只有一个列表 - 艺术家列表（来自artists.csv）。

不过你可以通过执行以下操作添加更多列表：
- interrogate在与 webui 相同的位置创建目录
- 将文本文件放入其中，每行都有相关描述

```
For example of what text files to use, see https://github.com/pharmapsychotic/clip-interrogator/tree/main/data. In fact, you can just take files from there and use them - just skip artists.txt because you already have a list of artists in artists.csv (or use that too, who's going to stop you). Each file adds one line of text to the final description. If you add ".top3." to filename, for example, flavors.top3.txt, the three most relevant lines from this file will be added to the prompt (other numbers also work).
```

#### **渐变提示编辑**

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#prompt-editing

#### **Face restoration三次元人脸修复**

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration

#### 自定义.css

创建一个名为user.cssnear的文件webui.py并将自定义 CSS 代码放入其中。

For example, this makes the gallery taller:
```
#txt2img_gallery, #img2img_gallery{
    min-height: 768px;
}
```
#### notification.mp3 提示声音


If an audio file named `notification.mp3` is present in `webui's root folder`, it will be played when the generation process completes.

### 运行

#### 4GB 显卡支持

针对具有低 VRAM 的 GPU 的优化。这应该可以在具有 4GB 内存的视频卡上生成 512x512 图像。

`--lowvram` 是 basujindal 对优化思想的重新实现。模型被分成模块，GPU内存中只保存一个模块；当另一个模块需要运行时，前一个模块将从 GPU 内存中删除。这种优化的性质使处理速度变慢——与我的 RTX 3090 上的正常操作相比，速度慢了大约 10 倍。

`--medvram` 是另一个优化，通过不在同一批次中处理条件和无条件去噪，可以显着减少 VRAM 的使用。

这种优化的实现不需要对原始的稳定扩散代码进行任何修改。

经过 10/10 的优化，RTX2050的4GB 显卡也可以使用 `--medvram` 。

显存下限是 2GB。

#### 不间断生产

在WebUi的生成键右击即可出现 不间断生成 的选项。

#### 图片信息 Png info

生成的图片自带 令牌信息，拖放到 查看页面即可查看 。



[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)