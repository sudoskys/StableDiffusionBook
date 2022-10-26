# 炼丹
>Todo 
>材料
>
>https://github.com/huggingface/diffusers/issues/712
>
>https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2002


这节对显卡的要求较高。

在法律上，**不应该**无视画师约定的版权许可，采集作品数据进行训练。在道德上，**不应该**用训练结果贬低原画师作品价值，俗谚有 “吃水不忘挖井人”，训练数据并不能作为你的贡献。

目前个人认为效果认为效果最好的是 DreamBooth，但是其训练时对显存要求较高（>12GB）。


## 认知炼丹

如果你在 `--medvram` 参数下开始训练，可能会出现 `RuntimeError: Expected all tensors to be on the same device` 错误，无法创建训练。
这是优化机制导致的[问题](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2399)，WebUi 在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/cbb857b675cf0f169b21515c29da492b513cc8c4) 中允许了在 `--medvram` 下创建 embedding 的情况。请更新版本到这个版本之后。

**关于 batch size**

更大的 batch size 可能稍微加快训练并稍微提升训练效果，但也需要更大的显存。


### Textual Inversion (TI)

从一些具有共同语义 [v] 的图片中，提取 [v] 的一个方法。提取出的 [v] 张量称之为 "Embedding"。将 Embedding 保存为文件，之后生成图片时就可以在 prompt 中以文件名引用。


#### 特征

训练产物大小较小，webui 自带训练支持。

可以解决新出的角色画不出的问题，或者模仿特定的可以用语言精确描述的艺术风格。因为 TI 是在 Text Encoder 的输出做处理，所以并不能让模型学习到它不知道的概念。


#### 使用

使用时，将 embedding（一个 .pt 或一个 .bin 文件）放入 webui 的 `embeddings` 目录并在 prompt 中写要用的 embedding 的文件名（不包括扩展名）即可，不必重启 webui。可以同时使用多个 embedding。


#### 相关

[webui 给的英文说明和效果图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion)

[相关 embeddings](https://gitlab.com/16777216c/stable-diffusion-embeddings)，里面有相关效果预览。

[list of Textual Inversion embeddings for webui(SD)](https://rentry.org/embeddings)

[HuggingFace 的 embedding 库](https://cyberes.github.io/stable-diffusion-textual-inversion-models/)


### Hypernetwork (HN)

一类给模型生成权重的网络，在这里是给 LDM 生成权重。是一个较为实验性的方法，NAI 率先探索了在 LDM 上使用。


#### 特征

与 TI 不同，Hypernetwork 会改动 LDM 本身的权重，所以可以训练出无法用语言精确表述的细节，也更适用于画风的训练。

训练产物大小中等，webui 自带训练支持。


#### 使用

使用时，将 Pt 放入 `/models/hypernetworks` 并在设置选项勾选启用它。

NAI Leaks 的 `novelaileak\stableckpt\modules\modules` 中有 NAI 训练的一些 Hypernetwork。

!!! tip
    `.pt`文件，一般情况下小的是 embedding 大的是 hypernetwork。


### DreamBooth (DB)

直接微调 LDM 和 Text Encoder 以适应用户特定的图像生成需求的一个方法。

> 你能想象你自己的狗环游世界，或者你最喜欢的包在巴黎最独特的展厅里展示吗？你的鹦鹉成为一本插图故事书的主角呢？


#### 特征

与 TI 和 HN 不同，DreamBooth 可以做到出图和训练集高度相似但是却不失泛化能力，用于训练特定具象概念（比如一个角色穿着特定衣服）效果特别好。但是不像 TI 和 HN 像完整权重的 “插件” 一样即插即用，强度也不可调。

这个模型并非为学习画风（抽象概念）而设计。但似乎可以一定程度上适应“画风”。具体效果交由读者你实验。


#### 使用

把 DreamBooth 训练出的 .ckpt 文件放进 webui 的 `models\Stable-diffusion` 目录里，在 webui 的左上角切换到即可使用。

![SAMPLE](https://dreambooth.github.io/DreamBooth_files/high_level.png)

官网 https://dreambooth.github.io/

论文 https://arxiv.org/abs/2208.12242


### Advanced Prompt Tuning (APT)

> "Can super dramatically improve the image quality and diversity"

[Pull Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2945)

添加对否定词的即时嵌入学习，以显着提高生成图像的质量。 高质量的概念可以从单个图像中学习。 

添加重建损失以提高生成图像的细节质量和丰富度。

添加通过人工注释训练的鉴别器（使用 convnext 实现）允许嵌入基于模型进行学习。 


### Aesthetic Gradients

微调 CLIP 以适应某个特定生成需求的方法，可以和 TI 一样起到缩短 prompt 的作用。可能略微提升出图的质量。

这项技术通过在生成时计算每个图片的权重，提供了一个 `我不说你应该懂往哪里训练` 的功能。使AI更聪明地调整并增加细节。

此项功能来自这个 [存储库](https://github.com/vicgalle/stable-diffusion-aesthetic-gradients)，在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/2b91251637078e04472c91a06a8d9c4db9c1dcf0) 中，此功能被剥离为插件。


#### 特征

通过这项技术，你不需要通过 过多提示词 来提升图片的质量，而是保持作品原始的总体构图，并提高美观度。在少量提示词情况下也可以生成效果不错的作品。

据暗影·夜光所言[^11]，添加 25% 以内的权重，就可以稍微改善画面的美观度而不影响内容。美学 与 Hypernetworks 让 Ai 作品更接近原画师风格，但是美学权重本身效果并不好。需要配合 Hypernetworks 超网络。

训练这项模型很快，但是在每一次生产时都会重新为图片计算一次，所以出图很慢。

注意：当种子改变时，训练结果也会改变。


#### 使用

你可以使用下面的 Git 命令来安装这个东西。

```commandline
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients extensions/aesthetic-gradients
```

安装后，在 webui 的 `extensions` 文件夹下面创建 `aesthetic-gradients` 文件夹。

使用时，把 Pt 放在 `models/aesthetic_embeddings/`

然后重启程序，你就可以在 Img2Img 中使用此项功能。

![Aesthetic_other](https://user-images.githubusercontent.com/75739606/197824140-50dca98e-856f-44ca-99e6-da2fee5bb23c.png)
<!--
![Aesthetic_other](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/Aesthetic_other.png)
-->



---------------


## Textual Inversion[^7]

[官方Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion#training-embeddings)

!!! danger "重命名 VAE 文件"

​	重命名 VAE 权重文件非常关键，如果带着VAE训练效果十分差。你应该在启动 webui 前把 "xxx.vae.pt" 重命名为 "xxx.vae.pt.disabled" 或其他名字。

#### 准备数据集

数据集应保证风格一致，内容具有同一概念。如果算力允许，图片越多越好。数据内容可以是插画，抽象画作，也可以是表情包。


### 要求

显存至少 6GB，舒适使用需要 12GB 显存。根据实验数据，8GB 显存应该选择 `512x512` 分辨率；不推荐 `--lowvram` 和 `--medvram` 参数下进行训练。

如果你有足够的 VRAM，那么使用 `--no-half --precision full` 可能会防止溢出带来的问题。


你可以中断和恢复训练，但是 optimizer state 不会被保存，所以不推荐这样做。


### 设置说明

准备 30 张以上的目标人设图片，每一张图片应当裁剪为同样的比例。（ webui 已经支持了长方形图片的裁剪）

在 `Interrogate Options` 设置中，`Interrogate: deepbooru score threshold` 是 deepdanbooru (从图像提取标签的一个模型) 标签置信度阀值。建议使用 0.75，也就是保留置信度大于 0.75 的结果。

比如，假定识别结果为：

```
1.000 1girl
0.986 blush
0.981 long_hair
0.980 solo 
0.918 japanese_clothes
0.912 rating:safe 
0.798 bangs
0.795 wide_sleeves
0.731 blunt_bangs 
0.637 long_sleeves
0.628 monochrome
```

就会丢弃 `wide_sleeves` 以下的内容。

`Interrogate: deepbooru sort alphabetically` 是按照字母顺序排序 Tag，因为 Tag 对结果影响很大，所以我们取消勾选此项。

`use spaces for tags in deepbooru` 应当开启。

`escape (\) brackets in deepbooru (so they are used as literal brackets and not for emphasis)` 应当开启。

按下 `Apply setting` 保存设置。


### 创建训练

打开 `train` 选项卡，在 `Create embedding`选项卡新建一个 `embedding` 模型。

`Number of vectors per token` 是 embedding 的宽度，与数据集有关，如果少于 百张，可以设置 3。

宽度越大时效果越强，但是需要数万轮训练。据 [usim-U](https://space.bilibili.com/99162407) 的视频，设置 24 至少需要三百张高质量作品。

`Name` 输入框输入你预想的出现此人设的 提示词。

`Initialization text` 可以输入 `one girl` 这种大分类，只能填一个，人物可以是1girl或者1boy，或者画风。

`Number of vectors per token` 是此 `embedding` 要占据的 token 位数量，越多越好，但是相应也会减少其他提示词 token 的位置。

新建，会创建一个在 `embedding` 下的 pt 文件。


### 预处理

打开 `Preprocess images` 选项卡。

`Source directory` 中填入你的训练用图片文件夹目录，里面只允许有训练图片。

`Destination directory` 中填入预处理完毕后图片保存路径。

选择训练的图片大小，一般 8Gb 显卡使用 `512x512` ，尺寸越大不一定越好。


**接下来有四个复选框**


`Create flipped copies` 

- 勾选后会将图片镜像反转来增加数据量。

- `Use deepbooru caption as filename` 

深度学习识 Tag，勾选后可以训练适用于 NAI 的 `embedding`。 如果你没有这个选项，需要在启动项添加`--deepdanbooru`

Windows 需要在 `webui-user.bat` 的 `COMMANDLINE_ARGS=` 一行添加，或者直接 `python launch.py --deepdanbooru` 。（如果启动卡住是网络问题）

- `Use BLIP for caption` 使用 BLIP 模型为文件名添加标题。不太适合二次元图片。

- `Split oversized images into two` 将超大图像一分为二，一般不用。


所以我们勾选 `Use deepbooru caption as filename` 和 `Create flipped copies` 。

点击按钮，等待处理结束。


### 训练

训练是一个动态的过程！

在 `Train` 子选项卡中，选择你要训练的模型。

`Learning rate`(超参数：学习率)，学习速率代表了神经网络中随时间推移，信息累积的速度，这个参数较大地影响了影响训练的速度。

通常，Learning rate越低学习越慢（花费更长的时间收敛），但是效果一般更好。

训练时，可以先用较大的学习率进行测试，然后逐步调小 `0.1 -- 0.02 -- 0.005` ，每次测试都用上一次效果最好的。

一般设置为 0.005，如果想快一些，可以使用 0.01 加快。但是如果设置得太高，梯度下降时候步长太大无法收敛，会且可能会破坏 `embedding` ， 效果达不到预期。如果设置的太小，容易陷入局部最优。

![CS231n](https://user-images.githubusercontent.com/75739606/197824268-273f074d-9622-41ad-9f1a-2980f180dbd9.png)
<!--
![CS231n](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/CS231n.png)
-->

`Log directory` 是日志目录

`Prompt template file`是带有提示的文本文件，每行一个，用于训练模型。

目录中`textual_inversion_templates`，解释了你可以使用这些文件做什么。

训练时会使用 `style.txt` 和`subject.txt`

**如果是训练画风，使用`style.txt`**

**如果是训练人物，使用`subject.txt`**

```
[name]:  embedding 名称
[filewords]: words from the file name of the image from the dataset. See below for more info.
```
`Preview prompt` 预览，完成后用此提示词生成一张预览。 
如果为空，将使用来自 prompt 的提示。

`Save a copy of embedding to log directory every N steps, 0 to disable`
每 N 步将嵌入的副本保存到日志目录，0 表示禁用

`Save an image to log directory every N steps, 0 to disable`
每 N 步保存一个图像到日志目录，0 表示禁用

`Max steps`决定完成多少 `step` 后，训练将停止。

一个 step 是向模型训练一张图片（或一批图片，但目前不支持批量）并用于改进 embedding。如果你中断训练并在以后恢复训练，步数会被保留。


角色形象的风格化模型，建议步数为 15000-40000

画师画风的风格化模型，建议步数为 40000-80000

!!! tip
    这里给出的是一个参考，实际上 5000 和 7000 也有人成功。
    
    关键在于 Loss 率，Loss 10 轮不降低就可以停止了。
    
    如果Loss大于 0.3 ，效果就不是很好

如果太多会过拟合(可以理解为Ai的死板)，请随时观察，如果过拟合，可以停止。如果效果不是很好，可以去找早些时候的模型继续训练。**不断调整**找到一个好的效果。

`Save images with embedding in PNG chunks` 是生成一个图片形式的 pt 文件。~人物卡~

!!! danger "再次检查你有没有重命名 VAE 文件"

​	如果你忘了，带着VAE训练效果十分差。重启把 "xxx.vae.pt" 重命名为 "xxx.vae.pt.disabled" 或其他名字。


点击 右下角训练，等待。

训练完毕。将 VAE 权重文件重命名回去，重启程序。


### 备注


**效果评价**

可以尝试模仿原作品。

**[filewords]**

这个是代表 提示词模板文件 的 Tag，可以实现把**文件名插入提示词**。

第一，默认情况下，文件的扩展名以及-文件名开头的所有数字和破折号 ( ) 都会被删除。

所以这个文件名：`000001-1-a man in suit.png`将成为提示文本：`a man in suit`。文件名中文本的格式保持不变。

第二，可以使用 `Filename word regex` 和 `Filename join string` 选项更改文件名中的文本。

例如，使用单词 `regex =\w+` 和 连接字符串 = `,` ，上面的文件将生成以下文本：`a, man, in, suit`。

正则表达式会从文本提取提示词
`['a', 'man', 'in', 'suit', ]`

并将连接字符串（'，'）放在这些单词之间以创建一个文本`a, man, in, suit`

也可以创建一个与图像 ( 000001-1-a man in suit.txt) 具有相同文件名的文本文件，然后将提示文本放在那里。将不使用文件名和正则表达式选项。

**Unload VAE and CLIP from VRAM when training**

训练时从 VRAM 中卸载 VAE 和 CLIP
设置选项卡上的此选项允许您以较慢的预览图片生成为代价节省一些内存。

训练的结果是一个 .pt 或一个 .bin 文件（前者是原作者使用的格式，后者作为 diffusers library）

<iframe src="//player.bilibili.com/player.html?aid=559085039&bvid=BV1ae4y1S7v9&cid=859894044&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>


## Hypernetwork[^13]

Hypernetworks 是一种新颖的概念，用于在不触及任何权重的情况下微调模型。

!!! tip
    在设置内勾选 **Move VAE and CLIP to RAM when training hypernetwork. Saves VRAM.**，防止VAE引起问题。

你可以在 `train` 选项卡训练 `Hypernetworks`，训练方式与  `Textual Inversion` 相同。

注意,应该使用非常低的学习率，例如 0.000005 或 0.0000005.

如果学习率是5e-6 ，步数大约 10000 到 20000 。[学习率计算器](https://colab.research.google.com/drive/1qzweYEMIFkG6jPa04tD1MhWWOzgSnDvP?usp=sharing)，来自 [这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

分辨率设置为要训练的分辨率，最好使用 1:1 的纵横比。

如果数据集的图片是长条或者横条，请勾选 `Split cversized images into two`

如果你还没有打标签，可以勾选 `Using deepbooru for caption` ，来让 deepbooru 识别标签。


### hyper network layer structure

If write "1, 2, 1", hypernetworks are composed of 2 fully connected layers whose intermediate dim is 2x, which is same as up to now.

The more you add the number, like "1, 2, 4, 2, 1", the more the structure of hypernetworks becomes deeper. Deep hypernetworks are suited for training with large datasets.

层结构目前还是推荐1，2，1，如果想扩展的话，可以尝试1，2，2，1.[^16]

1，2，1的pt文件大概是83.8MB，1，2，2，1大概是167MB。如果要扩展的话，一定要注意盘空间是否足够，否则很容易爆盘。[^16]

### Add layer normalization

If checked, add layer normalization after every fully connected layer.

防止超网络过拟合，使训练更加稳定。

LN对防止过拟合使训练更加稳定是有意义的，但是根据之前的测试，linear开开启LN的情况，可能学不进去东西，出空包弹。使用激活函数后，LN现在是可使用的。[^16]

Swish似乎并不需要开启LN，因为Swish的训练速度慢，所以应该使用更高的学习率。

### USE Dropout

启用Dropout可以防止超网络过拟合，目前不支持自定义dropout比率，默认为0.3

在1，2，1的层结构上，dropout可能只会对小数据集有帮助。


### 激活函数 activation functions

关于[激活函数](https://www.geeksforgeeks.org/activation-functions-neural-networks/)

激活函数是神经网络中非线性的来源。如果去掉激活函数，神经网络的权重和偏差只会进行线性变换(不管神经网络有多少层，线性运算的复合还是线性运算的)，最终的效果只相当于单层的线性模型。

激活函数对输入进行非线性变换，使其能够学习和执行更复杂的任务，深层神经网络表达能力更加强大。

在 10/21/22 的一个 [合并请求](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3199) 中，这项功能被 discus0434 提出。

支持 relu、leakyrelu 和 linear 。选择 linear(线性) 的效果和没有激活函数一样。

Swish对比Relu和Linear要更好，Swish适合更大的网络，而Elu可能对1，2，1的超网络带来更好的结果。[^16]


[相关的英文指南](https://rentry.org/hypernetwork4dumdums)

[相关的英文讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

![hypernet对比图](https://user-images.githubusercontent.com/75739606/197823893-88a3bece-9100-41d1-8f07-bc6d9b3aa92c.jpg)
<!--
![hypernet对比图](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/exp_hypernet.jpg)
-->
>hypernet对比图,the woman in picture is 24 years old

### 额外注意[^16]

在开启LN选用激活函数并开启Dropout后，超网络没以前那么容易过拟合，但是可能需要更大的学习率进行学习。

[Hypernetwork画风训练经验分享](https://shiina-h.notion.site/shiina-h/Hypernetwork-1fc0b60645284c5e96bb41b583a4e86f)



## DreamBooth

DreamBooth 的模型是一种新的文本到图像“个性化”（可适应用户特定的图像生成需求）扩散模型方法。

### 配置要求

Windows 系统至少需要 16, Linux 系统要求显存大于 8 GB

### 训练

- Linux

直接用下面的笔记本里的代码，不过需要英文基础。

[Nyanko Lepsoni 的 Colab 笔记本](https://colab.research.google.com/drive/17yM4mlPVOFdJE_81oWBz5mXH9cxvhmz8)

[RcINS 的 Colab 笔记本](https://colab.research.google.com/drive/1C1vVZ59S4kWfL7jIsczyLpmxbD4cOA-k)

以上笔记本来自 [社区置顶](https://t.me/StableDiffusion_CN/196744)

- Windows

？

- 参数分析

[使用 Dreambooth 训练稳定扩散的实验的分析](https://wandb.ai/psuraj/dreambooth/reports/Dreambooth-training-analysis--VmlldzoyNzk0NDc3)

### 其他 

相关的还有 [DreamBooth_Stable_Diffusion](https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb#scrollTo=K6xoHWSsbcS3) 和 [fast-DreamBooth](https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb)

[Dreambooth Gui](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2927)

[ShivamShrirao/diffusers](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

<!--
[飞桨dreambooth训练教程](https://docs.qq.com/doc/DUHVuZ3BNV0FkT1R6)
-->

https://github.com/XavierXiao/Dreambooth-Stable-Diffusion

##  Aesthetic Gradients


Source directory 填数据源文件夹地址。

生成的embedding在models文件夹里面。


[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)

[^12]:[超网络训练](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2284)

[^13]:[超网络训练指南](https://rentry.org/hypernetwork4dumdums)


[^14]:[dreambooth-小鬼峰](https://www.bilibili.com/read/cv18935031?from=search)

[^15]:[dreambooth-论文](https://arxiv.org/abs/2208.12242)

[^16]:[Hypernetwork画风训练经验分享](https://shiina-h.notion.site/shiina-h/Hypernetwork-1fc0b60645284c5e96bb41b583a4e86f)

