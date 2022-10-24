# 自训练

>此页面正在精修

>Todo 
>Dreambooth
>
>
>
>

本节讨论 WebUi 的高度自定义功能：`train`，通过这项功能，我们可以为 Ai 增加类似 DLC 扩展包一样的功能。

目前个人认为效果认为效果最好的是 `dreambooth`，但是其对显存要求较高（>12GB）

`Textual Inversion` 训练为了 `embeddings`，`embeddings` 为 Ai 提供处理过的输入数据，告诉这个角色“具体是什么”，训练特殊的人物，角色会更好。

`Hypernetworks` 则会对超网络的改动，与 `embeddings` 不同，`Hypernetworks` 会对模型进行微调，所以泛化效果更加好，训练画风会更好。

`Aesthetic Gradients` 通过计算平均权重，来提升图片的质量，提高美观度。在少量提示词情况下也可以生成效果不错的作品。

`dreambooth` 可适应用户特定的图像生成需求。"只需几张指定物体的照片和相应的类名（如“狗”）作为输入，并添加一个唯一标识符植入不同的文字描述中，DreamBooth 就能让被指定物体“完美”出现在用户想要生成的场景中。"

如果你在 `--medvram` 参数下开始训练，可能会出现 `RuntimeError: Expected all tensors to be on the same device` 错误，无法创建训练。

更大的 `batch size` 可以加快训练，但也需要更大的显存。

这是优化机制导致的[问题](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2399)，解决方法是去除这个参数。这节对显卡的要求较高。


## Textual Inversion 识别新角色

`Textual Inversion` 允许你在自己的图片上训练一小部分神经网络，并在生成新图片时使用结果。可以数据集没有新出的角色画不出的问题，模仿特定的艺术风格。实际上是 NLP 中的即时调优。

[官方英文说明和效果图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion)

使用时，将 `embedding` (一个 .pt 或一个 .bin 文件) 放入 `Textual Inversion` 目录并在 `prompt` 提示词中提到你要用的 `embedding` 的文件名(*.pt)即可。不必重新启动程序即可使其正常工作。

通过这项技术，我们可以让 Ai 认识 2021 年之后的动漫人物（数据集没有囊括的）。但是使用场景单一，构图必须和原素材一致。

多 `embeddings` 可以一起使用，程序启动时会自动加载它们

[相关 embeddings](https://gitlab.com/16777216c/stable-diffusion-embeddings)，里面有相关效果预览。

[list of Textual Inversion embeddings for WebUi(SD)](https://rentry.org/embeddings)

[HuggingFace 的embeddings库](https://cyberes.github.io/stable-diffusion-textual-inversion-models/)


## Hypernetworks 超网络

超网络是一种新颖的概念，用于在不触及任何权重的情况下微调模型。[^12]

NAI Leak 的 `hypernetworks` 就是超网络，用来做 embeddings（风格化）。

使用时，将 Pt 放入 `/models/hypernetworks` 并在设置选项勾选启用它。

!!! tip
    Pt文件，小的是 embedding 大的是 hypernetworks

## Dreambooth 

DreamBooth 的模型是一种很新的扩散模型，给定3-5张自己随意拍摄的某一物体的图片，就能得到不同背景下的该物体的新颖再现。

`Dreambooth` 会把你给出的数据插入到模型的输出中，而 Textual Inversion 从模型中挖掘内容。 所以如果你要画一个非常冷门的东西，它的数据恰巧又不在模型中，AI就傻了。

另外，这个模型不可以学习画风，只能学习物体人物特点。但可以适应画风。模型无法很好地学习到照片中物体的整体特征，反而可能出现过拟合。

生成的模型是剪切过训练数据的 ckpt 模型，需要放进 model 目录里进行替换。

![SAMPLE](https://dreambooth.github.io/DreamBooth_files/high_level.png)

https://dreambooth.github.io/

https://arxiv.org/abs/2208.12242



## Aesthetic Gradients 美学权重

这项技术通过在生成时计算每个图片的权重，提供了一个 `我不说你应该懂往哪里训练` 的功能。使AI更聪明地调整并增加细节。

此项功能来自这个 [存储库](https://github.com/vicgalle/stable-diffusion-aesthetic-gradients)，在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/2b91251637078e04472c91a06a8d9c4db9c1dcf0) 中，此功能被剥离为插件。

你可以使用下面的 Git 命令来安装这个东西。

```commandline
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients extensions/aesthetic-gradients
```

安装后，在 webui 的 `extensions` 文件夹下面创建 `aesthetic-gradients` 文件夹。


然后重启程序，你就可以在 Img2Img 中使用此项功能。

通过这项技术，你不需要通过 过多提示词 来提升图片的质量，而是保持作品原始的总体构图，并提高美观度。在少量提示词情况下也可以生成效果不错的作品。

据暗影·夜光所言[^11]，添加 25% 以内的权重，就可以稍微改善画面的美观度而不影响内容。美学 与 Hypernetworks 让 Ai 作品更接近原画师风格，但是美学权重本身效果并不好。需要配合 Hypernetworks 超网络。

训练这项模型很快，但是在每一次生产时都会重新为图片计算一次，所以出图很慢。

注意：当种子改变时，训练结果也会改变。

![Aesthetic_other](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/Aesthetic_other.png)





---------------



## Textual Inversion 自训练[^7]

[官方Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion#training-embeddings)

!!! danger "重命名 VAE 文件"

    重命名VAE模型文件非常关键，如果带着VAE训练效果十分差。

    或者不需要重命名vae文件，直接在设置里打开 `Move Vae...`

### 准备数据集

数据集在保证风格一致和内容同质化的情况下，如果算力允许，越多越好。数据内容可以是插画，风格抽象画作，表情包。

可以从 [yande](https://yande.re/post)，pixiv 等平台获取数据集。[^10]

!!! tip
    如果作品不是公共版权，请获取授权再进行操作。


### 模型要求

WebUi 应该是 Git 的最新版本。

显存至少 6GB，正常使用需要 12GB 显存。根据实验数据，8GB 显存应该选择 `512x512` 分辨率，不能在 `--lowvram` 和 `--medvram` 参数下进行训练，不支持 `batch sizes` 和 `gradient accumulation` 。

训练可使用半精度浮点，但是否好用还需要进行实验。

如果你有足够的内存，那么在 `--no-half --precision full` 下运行会更安全。


你可以中断和恢复训练而不会丢失任何数据（AdamW 优化参数除外，但似乎现有的存储库都没有保存这些，所以一般认为它们并不重要）。


### 设置说明

准备 30 张以上的目标人设图片，每一张图片应当裁剪为同样的比例。（即使WebUi已经支持了长方形图片的裁剪）

在 `Interrogate Options` 设置中，`Interrogate: deepbooru score threshold` 是 deepbooru(从图像提取标签) 可用标签的阀值。建议使用 0.75,也就是保留预测结果大于 75%的结果。

比如，假定识别结果为

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

就会丢弃 `wide_sleeves` 以下的内容

`Interrogate: deepbooru sort alphabetically` 是按照字母顺序排序 Tag，因为 Tag 对结果影响很大，所以我们取消勾选此项。

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

Windows 需要在 `web-user.bat的COMMANDLINE_ARGS=` 一行添加，或者直接 `python webui.py --deepdanbooru` ，如果设施条件比较好可以加上 `--precision full`
(如果启动卡住就是网络问题咯)

- `Use BLIP for caption`

使用来自 interrogator 的 BLIP 模型为文件名添加标题。

勾选后适用于 `Stable Diffusion` 的训练。

- `Split oversized images into two` 

意思就是将超大图像一分为二，一般不用。


我们勾选 `Use deepbooru caption as filename` 和 `Create flipped copies`

点击按钮，等待处理结束。


### 训练

训练是一个动态的过程！

在 `Train` 子选项卡中，选择你要训练的模型。

`Learning rate`(超参数：学习率)，学习速率代表了神经网络中随时间推移，信息累积的速度，这个参数较大地影响了影响训练的速度。

通常，Learning rate越低学习越慢（花费更长的时间收敛），但是效果一般更好。

训练时，可以先用较大的学习率进行测试，然后逐步调小 `0.1 -- 0.02 -- 0.005` ，每次测试都用上一次效果最好的。

一般设置为 0.005，如果想快一些，可以使用 0.01 加快。但是如果设置得太高，梯度下降时候步长太大无法收敛，会且可能会破坏 `embedding` ， 效果达不到预期。如果设置的太小，容易陷入局部最优。

![CS231n](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/CS231n.png)


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

!!! danger "等一下！ VAE 文件"
    重命名VAE模型文件非常关键，或者确定你已经设置里打开  `Move Vae...`
    
    如果带着VAE训练效果十分差。
    

点击 右下角训练，等待。

训练完毕。再次重命名 Vae 文件，重启程序。


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


##  Aesthetic Gradients 美学权重自训练


Source directory 填数据源文件夹地址。

生成的embedding在models文件夹里面。


## Hypernetworks 自训练[^13]

Hypernetworks 是一种新颖的概念，用于在不触及任何权重的情况下微调模型。

你可以在 `train` 选项卡训练 `Hypernetworks`，训练方式与  `Textual Inversion` 相同。

注意,应该使用非常低的学习率，例如 0.000005 或 0.0000005.

如果学习率是5e-6 ，步数大约 10000 到 20000 。

分辨率设置为要训练的分辨率，最好使用 1:1 的纵横比。

如果数据集的图片是长条或者横条，请勾选 `Split cversized images into two`

如果你还没有打标签，可以勾选 `Using deepbooru for caption` ，来让 deepbooru 识别标签。


### hyper network layer structure

If write "1, 2, 1", hypernetworks are composed of 2 fully connected layers whose intermediate dim is 2x, which is same as up to now.

The more you add the number, like "1, 2, 4, 2, 1", the more the structure of hypernetworks becomes deeper. Deep hypernetworks are suited for training with large datasets.

### Add layer normalization

If checked, add layer normalization after every fully connected layer.

防止超网络过拟合，使训练更加稳定。

### USE Dropout

Dropout 似乎很有用，如果需要避免过拟合/重影，请勾选这个。

### 激活函数 activation functions

关于[激活函数](https://www.geeksforgeeks.org/activation-functions-neural-networks/)

激活函数是神经网络中非线性的来源。如果去掉激活函数，神经网络的权重和偏差只会进行线性变换(不管神经网络有多少层，线性运算的复合还是线性运算的)，最终的效果只相当于单层的线性模型。

激活函数对输入进行非线性变换，使其能够学习和执行更复杂的任务，深层神经网络表达能力更加强大。

在 10/21/22 的一个 [合并请求](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3199) 中，这项功能被 discus0434 提出。

支持 relu、leakyrelu 和 linear 。选择 linear(线性) 的效果和没有激活函数一样。

一般来说relu用的最多。


[相关的英文指南](https://rentry.org/hypernetwork4dumdums)

[相关的英文讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

![hypernet对比图](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/exp_hypernet.jpg)
>hypernet对比图


## DreamBooth 训练

DreamBooth 的模型是一种新的文本到图像“个性化”（可适应用户特定的图像生成需求）扩散模型




[Colab 在线训练模型](https://colab.research.google.com/drive/17yM4mlPVOFdJE_81oWBz5mXH9cxvhmz8#scrollTo=lJoOgLQHnC8L)，需要Pro套餐算力(18-24G显存需要)

[DreamBooth_Stable_Diffusion](https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb#scrollTo=K6xoHWSsbcS3)

[fast-DreamBooth](https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb)

https://github.com/XavierXiao/Dreambooth-Stable-Diffusion




[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)

[^12]:[超网络训练](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2284)

[^13]:[超网络训练指南](https://rentry.org/hypernetwork4dumdums)


[^14]:[dreambooth-小鬼峰](https://www.bilibili.com/read/cv18935031?from=search)

