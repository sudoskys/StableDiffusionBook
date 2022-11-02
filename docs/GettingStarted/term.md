# 名词


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

