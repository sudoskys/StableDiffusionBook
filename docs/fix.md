
# 模型技法

这节介绍Debug和一些关于WebUi网页应用的优化方案，让它更好用。

至于如何创造令牌和参数相关，请看下一节：令牌，也就是参数如何写

!!! tip
    注意要经常从远端代码库拉取代码洗礼WebUi网页应用。

>本节只针对 NAI 模型展开教程。请多多关注 About 页面的社区获取最新进展和新闻。**大部分源教程来自：[^2]**


!!! info "版权"
    本仓库不提供具体链接（版权警告），可以看页面下标或关注中文社区 t.me@StableDiffusion_CN_WIKI。


[你应该看看，除了NAI模型外的其他模型：Stable Diffusion Models](https://rentry.org/sdmodels)


## 关于显卡

!!! info 
    注意你的温度并增加它们的冷却，有报道称 GPU 太热炸了.

先判断 cuda 是否可用。

打开命令窗，输入 python 进入，分行输入

```
import torch
print(torch.__version__)
print(torch.cuda.is_available())
```

**查看torch对应的cuda版本**
```
torch.version.cuda
```
输入 ctrl + z 退出

### 多GPU支持

Easiest mode would be implementing a ~data parallel approach, in which we have one model per GPU and you distribute the workload among them.

Given the amount of features this repo provides I think it could take some time to have em all supported in the parallel version.

[查看此 issue 页面](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/156)


### 16xx系显卡使用半精度生成图片[^3]


方案来自 [这个讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/28#issuecomment-1241448049)

1. 激活webui使用的venv,要在正确的虚拟环境里运行

2. 卸载掉现在所用的 torch 和 torchvision:
```
pip uninstall torch torchvision
```

3. 重新安装 `cuda 11.6`编译的 `torch` 和 `torchvision`。
```
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu116
```

4. 下载 `Cudnn_8.5` 备用。

使用下载工具下载，直接下载会跳转到 Nvidia 的开发者注册界面

Windows: https://developer.nvidia.com/compute/cudnn/secure/8.5.0/local_installers/11.7/cudnn-windows-x86_64-8.5.0.96_cuda11-archive.zip

其他版本：https://developer.nvidia.com/rdp/cudnn-archive

5. 将Cudnn 8.5压缩包里的bin和lib文件夹里的所有文件复制到 `venv\Lib\site-packages\torch\lib` 里，覆盖所有文件。

6. 然后16xx系显卡也可以愉快地使用半精度生成图片了！大幅降低显存占用，6G加载Full模型可以生成1024x640的图片。

但是，依然不能使用 `DDIM Sampling` ，但可以使用 `Euler a`





## NAI 模型

你的 models 文件夹应该是这样的。

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

其中，hypernetworks 和 Stable-diffusion 是需要新建的文件夹。其他文件根据规则重命名。

### Part 1

`final-pruned.ckpt即 stableckpt/animefull-final-pruned/model.ckpt` (pruned)，模型主文件。

`final-pruned.vae.pt` 即 `stableckpt/animevae.pt`，用于稳定风格。

`final-pruned.yaml` 即 `model.ckpt` 同文件夹的 `stableckpt/animefull-final-pruned/config.yaml`，记载额外的参数。

`hypernetworks` 包含了 `stableckpt/modules/modules` 里的文件，是风格相关的数据集，可以作为特定人物的 `embedding model` 调用，和 model 使用可以生成特定风格.文件 `*.pt`。在WebUi的设置标签页调用。


`stableckpt/vector_adjust/v2.pt` 没有什么用，可以自行训练，感觉不如 `hypernet`

`workspace` 就是前后端啦，40Gb显存及格，NAI采用的是 GPU 云。

### Part 2

`prodmodels` 是GPT模型(语言处理)，但是实际好像用了CLIP.

`random_stableckpt` 是一些模型，有的与Part1重复



![Part1](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/models.jpg)

??? info "附内容"
    - stableckpt/ - Stable Diffusion checkpoints
    - animefull-latest - The model NovelAI uses in production
    - workspace/ - Code used to train/run/finetune models
    - sd-private.tar.zst - Stuff to train Stable Diffusion
    - github/ - Code taken from GitHub. CREDENTIALS SCRUBBED
    - novelai/ - From NovelAI org
    - *.tar.zst Archived git repos, public AND PRIVATE
    - aboutus.gpg - Our public GPG key
    - sha256sum - SHA256 sums of every file
    - sha256sum.sig Detached signature for the sums, signed by our GPG key

启动 cli 有提示加载就 OK, 去设置选模型那里选喜欢的 `hypernetwork`



### 对于 NAI 模型的说明


`animesfw-latest` NAI 基线模型

`animefull-final-pruned` = `full-latest` = NAI 全量模型(成人内容)


!!!info "**4GB版本 or 7GB ？**"
        *diffusion model* 训练会产生两个模型：当前权重和加权平均后优化的EMA（效果好）
        7GB 的 ckpt 里包含了当前权重和EMA权重，pruned.py 删除了当前权重，留下了 EMA权重并重命名。所以差别不大。


**详细介绍**

<iframe src="//player.bilibili.com/player.html?aid=688965561&bvid=BV1Gm4y1A7VM&cid=857942294&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
BV1Gm4y1A7VM


## 说明

### Config.yaml？


NAIleak里边有个 config.yaml ， 将其改名为 `模型前缀.yaml` 和模型丢在一起就能加载,，但不建议加载 yaml，因为它会使内存占用加倍而不会对输出改变不大，但如果你真的想要，将其重命名为 [model name].yaml 并将其放在你的模型旁边。

### Vae 额外的权重

如果需要更好模拟NAI,务必使用 `animevae.pt`

### 半精度/全精度

float32 用于较旧的 gpus，或者你想要 100% 的精度

两者的输出应该几乎相同，主要区别在于大小和支持它的 GPU。

大多数新 GPU 使用半精度，因为它会降低 VRAM。

如果您想获得绝对的最佳质量或运行 16xx 卡，请仅使用非半精度和/或全精度。


### 横条参数说明

`step` 迭代多少次, 取值和 sampling method 有关, DDIM 收敛较快, 见 Sampler vs. Steps Comparison (low to mid step counts) 我用 DDIM 取 20 够了.

`batch count/batch size` 决定生成的图片数量, 显存够就加 batch size, 不够就 batch count, 得到的图片数量是两者之积 (小显存还是只动 count 就好)

`sample method`  我无脑 DDIM, Eula 也挺好用? (带 a 的是 ancestral 的意思, step 增长出图不稳定)

`cfg scale` 符合 prompt 的程度, 值越高越会字面看待 prompt, 低则给模型较大的发挥空间, 但是实际模型表现上来看 cfg scale 低 (6-8) 饱和度低, 偏线稿, 偏杂乱, 高 (18-22) 则饱和度偏高, 偏 CG 风格.

>过高的 CFG 会引起颜色失真，CFG 应该在 5-15 之间

`denoise strength` img2img 专属参数, 从 0 到 1 取值, 值越高 AI 对原图的参考程度就越低 (同时增加迭代次数), 我个人喜欢低 cfg 高 denoise 重绘图, 高 cfg 低 denoise 改细节.


[一个指南：RedditAbout](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)




### 生成图片发生BUG的自救

#### 16系显卡生成黑/绿图

[Green or Black screen](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs)

启动参数需要加 `--precision full --no-half`, 显存不足还要加 `--medvram`

使用 vae 模型后如果偶尔黑图，尝试加入 `--no-half-vae` 参数[^2]

#### RuntimeError Sizes of tensors must match
(img2img) 如果你得到RuntimeError: Sizes of tensors must match，你需要改变输入图像的分辨率

#### 彩虹混乱图

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





-------

## 优化靠近 NAI

[对NAI模型靠近NAI效果相关讨论，应该读一读！](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017)

### 进一步优化

出图后，可以将喜欢的结果从右侧的输出选项卡拖回 img2img 以进行进一步迭代。

### **靠近NAI,调整 Eta noise seed delta**

设置为 `31337` 可以更靠近

相关讨论 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017


### **对NAI模型更改 layers 忽略层数**



在 WebUi 的设置页面把 ignore last layers of clip mode 的横条改成 `2`

具体为什么见 [这里](https://blog.novelai.net/novelai-improvements-on-stable-diffusion-e10d38db82ac)

```
CLIP is a very advanced neural network that transforms your prompt text into a numerical representation. Neural networks work very well with this numerical representation and that's why devs of SD chose CLIP as one of 3 models involved in stable diffusion's method of producing images. As CLIP is a neural network, it means that it has a lot of layers. Your prompt is digitized in a simple way, and then fed through layers. You get numerical representation of the prompt after the 1st layer, you feed that into the second layer, you feed the result of that into third, etc, until you get to the last layer, and that's the output of CLIP that is used in stable diffusion. This is the slider value of 1. But you can stop early, and use the output of the next to last layer - that's slider value of 2. The earlier you stop, the less layers of neural network have worked on the prompt.

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#ignore-last-layers-of-clip-model
```

### **模型超参数**

如果你想达到 NovelAi 的效果，需要加 negative prompt（消极令牌）, 加载 hypernetwork （网络）和 vae.

https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1868#discussioncomment-3824077



### **NAI模型消极令牌**

使用以下令牌削除水印和文字内容

```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry
```

from [Here](https://t.me/StableDiffusion_CN/6043)

### **Euler等Samplers采样器参数**

请阅读 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1162


下面的引用是来自 [Reddit](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/) 的内容

??? info 
    Has to do with how diffusion-based models work. Basically, they start with a random noise image and 'mine' the noisy image for a less noisy output.

    This process is defined by a differential equation that describes how much noise is removed in a step.

    Solving these equations is a bit tricky; there's different approaches with tradeoffs between speed and accuracy and occasionally some special sauce to make this more than a zero-sum tradeoff (i.e. can make something a little bit faster and a lot more accurate, or vice versa, for example).

??? info "采样器不同"
    Euler is the simplest, and thus one of the fastest. It and Heun are classics in terms of solving ODEs.

    Euler & Heun are closely related. Heun is an 'improvement' on Euler in terms of accuracy, but it runs at about half the speed (which makes sense - it has to calculate the normal Euler term, then do it again to get the final output).

    LMS and PLMS are their cousins - they use a related, but slightly different approach (averaging out a couple of steps in the past to improve accuracy). As I understand it, PLMS is effectively LMS (a classical method) adapted to better deal with the weirdness in neural network structure.

    DDIM is a neural network method. It's quite fast per step, but relatively inefficient in that it takes a bunch of steps to get a good result.

    DPM2 is a fancy method designed for diffusion models explicitly aiming to improve on DDIM in terms of taking less steps to get a good output. It needs to run the denoising twice per step, so once again - it's about twice as slow.

    The Ancestral samplers are deceptively much further away from the corresponding non-Ancestral samplers and closer to each other. The corresponding algorithms are used - hence the names - but in a different context.

    They can add a bunch of noise per step, so they are more chaotic and diverge heavily from non-Ancestral samplers in terms of the output images. As per the normal-flavored samplers, DPM2-A is about half as fast as Euler-A.

    Weirdly, in some comparisons DPM2-A generates very similar images as Euler-A... on the previous seed. Might be due to it being a second-order method vs first-order, might be an experiment muck-up.

Euler 是最简单的，因此也是最快的。

Euler & Heun 密切相关。
Heun 在准确性方面是对 Euler 的“改进”，但它以大约一半的速度运行（它必须计算正常的 Euler，然后再次执行以获得最终输出）。

DDIM 是一种神经网络方法。 每一步都相当快，但效率相对较低，因为它需要很多步骤才能获得好的结果。



### **xformers加速**

加速推理,分辨率越高加速效果越好。

[Windows](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases) (30 系之外要自己编译)

自己编译指路 [wiki/Xformers](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Xformers)

>30 系显卡正常启动 --xformers, 其他显卡 --force-enable-xformers


-----

## 进阶

[英文原版](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

### **Textual Inversion**

Textual Inversion 允许您在自己的图片上训练一小部分神经网络，并在生成新图片时使用结果。

训练的结果是一个 .pt 或一个 .bin 文件（前者是原作者使用的格式，后者作为 diffusers library）

将 embedding 放入`embeddings`目录并在 prompt 令牌中提到你要用的 embedding 的文件名(*.pt)即可。

不必重新启动程序即可使其正常工作。


[英文说明和效果图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion)


[自己训练 embedding](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion#training-embeddings)

### X/Y 图

创建具有不同参数的图像网格。使用X类型和Y类型字段选择应由行和列共享的参数，并将这些参数以逗号分隔输入X值/Y值字段。支持整数、浮点数和范围。

Simple ranges 简单范围
```
1-5 = 1, 2, 3, 4, 5
```

Ranges with increment in bracket 括号范围
```
1-5 (+2) = 1, 3, 5
10-5 (-3) = 10, 7
1-3 (+0.5) = 1, 1.5, 2, 2.5, 3
```

Ranges with the count in square brackets 方括号范围
```
1-10 [5] = 1, 3, 5, 7, 10
0.0-1.0 [6] = 0.0, 0.2, 0.4, 0.6, 0.8, 1.0
```

设置截图

![引用官方 Wiki 的设置图](https://raw.githubusercontent.com/wiki/AUTOMATIC1111/stable-diffusion-webui/images/xy_grid-medusa-ui.png)
>引用官方 Wiki 的设置图

### **Loopback 回环生成**

在 img2img 中设置loopback脚本，它允许自动将输出图像作为下一批的Batch提供，相当于保存输出图像，并用它替换输入图像。

Batch 数设置控制获得多少次迭代

通常，在执行此操作时，您会自己为下一次迭代选择许多图像中的一个，因此此功能的有用性可能值得怀疑，但反正我已经设法获得了一些我无法获得的非常好的输出。

### **Prompt matrix 参数矩阵**

使用 | 分隔多个Tag，程序将为它们的每个组合生成一个图像。 例如，如果使用 `a busy city street in a modern city|illustration|cinematic lighting` ，则可能有四种组合（始终保留提示的第一部分）：

- a busy city street in a modern city
- a busy city street in a modern city, illustration
- a busy city street in a modern city, cinematic lighting
- a busy city street in a modern city, illustration, cinematic lighting

### **Outpainting 外部修补**

Outpainting 扩展原始图像并修复创建的空白空间。
您可以在底部的 img2img 选项卡中找到该功能，在 Script -> Poor man's outpainting 下。

```
Outpainting, unlike normal image generation, seems to profit very much from large step count. A recipe for a good outpainting is a good prompt that matches the picture, sliders for denoising and CFG scale set to max, and step count of 50 to 100 with Euler ancestral or DPM2 ancestral samplers.
```

### **Inpainting 修补**

在 img2img 选项卡中，在图像的一部分上绘制蒙版，该部分将被修复。

![result](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting.png)

选项：
- 在网络编辑器中自己绘制蒙版。
- 在外部编辑器中擦除部分图片并上传透明图片。 任何稍微透明的区域都将成为蒙版的一部分。 请注意，某些编辑器默认将完全透明的区域保存为黑色。

- 将模式（图片右下角）更改为"Upload mask"并为蒙版选择单独的黑白图像(white=inpaint)。

### **全分辨率修复！**

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#inpaint-at-full-resolution


### **Variations种子变化**

Variation strength slider 和 Variation seed field允许您指定现有图片应更改多少以使其看起来不同。
在最大强度下，您将获得带有变异种子的图片，至少 - 带有原始种子的图片（使用先前采样器时除外）。

### **Styles风格模板**

“Save prompt as style” 按钮将当前的提示写入 styles.csv，该文件包含样式集合

提示右侧的下拉框将允许您从以前保存的样式中选择任何样式，并自动将其**附加**到输入中

要删除样式，请从 styles.csv 中手动将其删除并重新启动程序。

### **Clip**

CLIP 可以从图像中提取令牌。

令牌提示 不会让你重现这个图像（有时甚至不会很接近），但它可能有用。

默认情况下，只有一个列表 - 艺术家列表（来自artists.csv）。

不过你可以通过执行以下操作添加更多列表：
- interrogate在与 webui 相同的位置创建目录
- 将文本文件放入其中，每行都有相关描述

```
For example of what text files to use, see https://github.com/pharmapsychotic/clip-interrogator/tree/main/data. In fact, you can just take files from there and use them - just skip artists.txt because you already have a list of artists in artists.csv (or use that too, who's going to stop you). Each file adds one line of text to the final description. If you add ".top3." to filename, for example, flavors.top3.txt, the three most relevant lines from this file will be added to the prompt (other numbers also work).
```

### **渐变提示编辑**

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

### **Face restoration三次元人脸修复**

适用于三次元。

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration

### 自定义.css

创建一个名为user.cssnear的文件webui.py并将自定义 CSS 代码放入其中。

For example, this makes the gallery taller:
```
#txt2img_gallery, #img2img_gallery{
    min-height: 768px;
}
```
### notification.mp3 提示声音


If an audio file named `notification.mp3` is present in `webui's root folder`, it will be played when the generation process completes.

### 开发自定义脚本

你可以在`modules/scripts.py`中找到Script类。

如果要创建你自己的自定义脚本，请创建一个实现类的python脚本，并将其放到scripts文件夹中，使用以下示例或文件夹中已有的其他脚本作为指导。

Script 类有四个主要方法，这里通过一个简单的[示例脚本](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Developing-custom-scripts)进行更详细的描述，这个脚本可以旋转和/或翻转生成的图像。




## 运行

### 4GB 显卡支持

针对具有低 VRAM 的 GPU 的优化。这应该可以在具有 4GB 内存的视频卡上生成 512x512 图像。

`--lowvram` 是 basujindal 对优化思想的重新实现。模型被分成模块，GPU内存中只保存一个模块；当另一个模块需要运行时，前一个模块将从 GPU 内存中删除。这种优化的性质使处理速度变慢——与我的 RTX 3090 上的正常操作相比，速度慢了大约 10 倍。

`--medvram` 是另一个优化，通过不在同一批次中处理条件和无条件去噪，可以显着减少 VRAM 的使用。这种优化的实现不需要对原始的稳定扩散代码进行任何修改。

!!! info
    经过 10/10 的优化，RTX2050的4GB 显卡也可以使用 `--medvram` 。

当然也可以减半精度，或者生成一张 64x64 清理 vram


### 不间断生产

在 WebUi 的生成键右击即可出现 不间断生成 的选项。

### 图片信息 Png info

生成的图片自带 令牌信息，拖放到 查看页面即可查看 。

### Colab

Tip：每天重置资源

### NAI 4chan简化版本

4chan版本魔改官后程序，会动态分配，显存不够内存来凑。

[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

[^3]:[16xx系显卡可以使用半精度生成图片的方式](https://t.me/StableDiffusion_CN/50749)
