# 帮助

## Sd如何工作

### 推理过程

![jalammar s pic](https://jalammar.github.io/images/stable-diffusion/stable-diffusion-components-and-tensors.png)

information creator 完全在图像信息空间（或潜伏空间）中工作。这一特性使它比以前在像素空间工作的扩散模型更快。在技术上，这个组件是由一个UNet神经网络和一个调度算法组成的。

- Text Encoder

提示词的解析由 Text Encoder/CLIP 处理(token embedding)，这里是提示词转译给AI的关键一步。

文本编码器负责将输入的提示转换为U-Net可以理解的嵌入空间。它通常是一个简单的基于变换器的编码器，将一连串的输入标记映射到一连串的 latent text-embeddings 中。

稳定扩散使用 ClipText 用于文本编码。输入文本，输出77个标记嵌入向量，每个都有768个维度。

- information creator

UNet + Scheduler(也就是采样算法)在潜在空间中逐步处理/分散信息。

它输入文本嵌入和一个由噪声组成的起始多维数组（结构化的数字列表，也叫张量），输出一个经过处理的信息阵列。

- Image Decoder

Text Decoder 根据从 information creator 那里获得的信息绘制一幅图画。 它只在过程结束时运行一次以生成最终图像。

autoencoder(VAE)模型有两个部分，一个编码器和一个解码器。编码器用于将图像转换为 latent representation，作为U-Net模型的输入。解码器则将 latent representation 转回图像。

在推理过程中，使用VAE解码器将反向扩散过程产生的去噪潜像转换回图像。在推理过程中，我们只需要VAE解码器。

Autoencoder Decoder(VAE)使用处理过的信息阵列绘制最终图像的解码器。输入处理过的信息阵列(dimensions: (4,64,64))，输出结果图像(dimensions: (3, 512, 512)，即(red/green/blue, width, height)。

- CLIP 的工作

![训练图](https://pic3.zhimg.com/80/v2-340920caff256e06c29cff7097e23e62_1440w.jpg)
>CLIP 训练图 from https://bbs.huaweicloud.com/blogs/371319

- 流程

Stable Diffusion 中使用的自动编码器的缩减系数为 8。这意味着一张 (4, 512, 512) 的图像在潜在空间中是 (4, 64, 64)。

在使用稳定扩散推理一张 512 x 512 的图片的过程中，模型用一个种子和一个文本提示作为输入。潜在种子生成大小 64 × 64 的随机潜在图像，而 prompt 进入 Text Encoder 通过CLIP的文本编码器转化为大小为77 × 768的文本嵌入。

U-Net 在以文本嵌入为条件的同时迭代地对随机高斯噪声表示进行去噪。U-Net通过 采样算法 计算去噪的潜在图像表示，输出噪声残差。这个步骤重复许多次后，潜在表示由 Image Decoder 的 auto encoder 的解码器解码输出。


![流程](https://raw.githubusercontent.com/patrickvonplaten/scientific_images/master/stable_diffusion.png)

- 扩展

[illustrated-stable-diffusion](https://jalammar.github.io/illustrated-stable-diffusion/)

[稳定扩散](https://huggingface.co/blog/stable_diffusion)

[稳定扩散入门 ](https://pub.towardsai.net/getting-started-with-stable-diffusion-f343639e4931)

[Stable Diffusion From Wikipedia](https://en.wikipedia.org/wiki/Stable_Diffusion)

### WebUi 的预处理

[WebUi的prompt_parser](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/prompt_parser.py) 通过本地 WebUi 实现了渐变等功能。

WebUi prompt 语法会转换为相应时间的 prompt,然后通过 embedding 交给 Ai 处理。

关于权重的实现：权重增加通常会占一个提示词位。

关于交叉注意力控制的实现：到了指定 Step ，WebUi 程序会替换对应 提示词，达到交叉注意力控制效果。

其他以此类推。

整个看下来，原理流程如图 

![prompt_draw](https://user-images.githubusercontent.com/75739606/198675128-c2c849d0-d024-468b-80c4-374f13e933e3.png)
>By RcINS

### 数学知识

[diffusion_model](https://www.cs.unc.edu/~ronisen/teaching/fall_2022/pdf_lectures/lecture8_diffusion_model.pdf)

[How diffusion models work: the math from scratch](https://theaisummer.com/diffusion-models/#classifier-free-guidance)

## 名词

### VAE

[变分自编码器](https://blog.csdn.net/a312863063/article/details/87953517)

### CFG 

这个词汇为 Classifier Free Guidance Scale 的缩写，用于衡量模型 生成的预期图片和你的提示保持一致的 程度。 Cfg Scale 值为 0 时，会生成一个基于种子的随机图像。[1^]

打个比方，想象你的提示是一个带有可变宽度光束的手电筒，你将它照到模型的潜在空间上以突出显示特定区域——你的输出图像将从该区域内的某个地方绘制，具体取决于种子。

将 Cfg Scale 拨向 零会产生极宽的光束 ，突出显示整个潜在空间——您的输出几乎可以来自任何地方。

将 Cfg Scale 拨向 20 会产生非常窄的光束， 以至于在极端情况下它会变成激光指示器，照亮潜在空间中的一个点。 

https://arxiv.org/abs/2207.12598

### 损失函数

关于 [损失函数](https://fangkaipeng.com/?p=2056#header-id-16)

### 潜在空间

压缩数据的表示，其中相似的数据点在空间上更靠近在一起。

### 黑话


| 缩写          | 解释        |
|---------------|----------------------------------------------|
| NAI           | (Novel AI ，一般特指Leak)                                            |
| 咒语/念咒     | prompts                                                              |
| 施法/吟唱/t2i | Text2Image                                                           |
| 魔杖          | t2i/i2i参数                                                          |
| i2i           | Image2Image,一般特指全部图片生成                                       |
| inpaint       | i2i一种maskredraw，可以局部重绘                                        |
| ti/emb/炼丹   | Train中的文本反转，一般特指Embedding插件                                        |
| hn/hyper/冶金 | hypernetwork，超网络                                                                           |
| 炸炉          | 指训练过程中过度拟合，但炸炉前的日志插件可以提取二次训练                                               |
| 废丹          | 指完全没有训练成功                                                                               |
| 美学/ext      | aesthetic_embeddings,emb一种，特性是训练飞快，但在生产图片时实时计算。                                |
| db/梦展       | DreamBooth，目前一种性价比高（可以在极少步数内完成训练）的微调方式，但要求过高                            |
| ds            | DeepSpeed，微软开发的训练方式，移动不需要的组件到内存来降低显存占用，可使db的vram需求降到8g以下。开发时未考虑win,目前在win有兼容性问题故不可用 |
| 8bit/bsb      | 一般指Bitsandbyte，一种8比特算法，能极大降低vram占用，使16g可用于训练db。由于链接库问题，目前/预计未来在win不可用                         |


### ENSD


在设置页中的 ENSD 是 eta 噪声种子增量。

它改变了你的种子，执行一些 eta/sigma 的操作。

NAI使用31337


### CLIP

```
CLIP is a very advanced neural network that transforms your prompt text into a numerical representation. Neural networks work very well with this numerical representation and that's why devs of SD chose CLIP as one of 3 models involved in stable diffusion's method of producing images. As CLIP is a neural network, it means that it has a lot of layers. Your prompt is digitized in a simple way, and then fed through layers. You get numerical representation of the prompt after the 1st layer, you feed that into the second layer, you feed the result of that into third, etc, until you get to the last layer, and that's the output of CLIP that is used in stable diffusion. This is the slider value of 1. But you can stop early, and use the output of the next to last layer - that's slider value of 2. The earlier you stop, the less layers of neural network have worked on the prompt.

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#ignore-last-layers-of-clip-model
```

要让 AI 作画，先要让程序“听懂”你的指令，比如

“a big cherry tree above a lake with flying petals in the sky.”

对一个相对复杂场景的文本描述，AI 需要能“理解”并匹配到对应的画面，大部分项目依赖的都是一个叫 CLIP 的模型。

CLIP 在生成模型的潜在空间进行搜索，从而找到与给定的文字描述相匹配的潜在图像。

它非常现代且高效。


### CUDA

配合 CUDA 技术，显卡可以模拟成一颗 PhysX 物理加速芯片。目前，全系列的 GeForce 8 显示核心都支持 CUDA。

使用 CUDA 技术，GPU 可以用来进行通用处理（不仅仅是图形）；这种方法被称为 GPGPU。与 CPU 不同的是，GPU 以较慢速度并发大量线程，而非快速执行单一线程。以 GeForce 8800 GTX 为例，其核心拥有 128 个内处理器。利用 CUDA 技术，就可以将那些内处理器做为线程处理器，以解决数据密集的计算。


### LDM

Latent Diffusion Model 潜在扩散模型。

![？](https://user-images.githubusercontent.com/75739606/198675128-c2c849d0-d024-468b-80c4-374f13e933e3.png)
>下面的框

[潜在扩散模型](https://zhuanlan.zhihu.com/p/573984443)

[1^]:[Getting Started With Stable Diffusion: A Guide For Creators](https://www.jonstokes.com/p/getting-started-with-stable-diffusion)
