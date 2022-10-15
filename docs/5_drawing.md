

# 调参魔法

这节会介绍 参数 和 相关的 WebUi(SD)  网页应用资源。部分内容只做简单介绍，因为前面有相关描述。

先请拿上 [调参魔法书](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml)和 [手抄魔法本](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/)。

如果你觉得查表很麻烦，可以打开 [Tag在线生成](https://aitag.top/)。

如果你不想读文字，可以打开 [推荐的视频教程](https://space.bilibili.com/35723238/channel/collectiondetail?sid=779851)

下面的语法对效果有很大影响，需要通读。

提前告知：WebUi 的设置页面需要按下 `Apply setting` 才能保存设置。

## 基本流程

![WorkFlow](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/draw_workflow.svg)

这幅图演示了循环迭代的流程
迭代方式，有循环迭代和线性迭代两种，线性迭代适用于多样性测试，而循环迭代是优化的更好选择。

来回改提示+固定种子并不是好选择。

目前研究基本方向是

- 咒术书 + PS/Inpaint

- 咒术书 + 3D 参考

### 迭代草图[^8]

这里讨论一下如何将**手绘草图**通过 Ai 绘画优化，*注意不是二次元*。

在第一次迭代中，您不需要太多 Steps，CFG 可以非常低（以获得更好的多样化结果），如果不想完全丢失草图，Denoising 应该在 0.3-0.4 左右。

在最后的迭代中，增加 Steps 和 Denoising 强度（但不超过 0.8，否则图像将被破坏，尤其是在大于 512*512 时）请参见[这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2213#issuecomment-1274137775)，同时根据需要提高 CFG 和尺寸。

你可以随时修复提示（添加或删除出现的细节）并尝试不同的采样器。

另外，你不应该在初次生成使用一个固定不变的种子？

因为如果你提供一个种子（而不是随机的 -1），你的图像很快就会变得过饱和、过度锐化、过度像素化。

当然如果你想微调，可以使用固定种子。

## 魔法入门

请先在前面了解一下 WebUi(SD) 网页应用的参数。

### 提示词语法简介

对于NAI用户，请阅读[官网Docs](https://docs.novelai.net/image/promptmixing.html),两方的提示词语法不通用。

**表示方法**

分隔，英文半角 `,` 做分隔符，在英文逗号前后存在一个或数个空格并不影响实际使用。

混合，WebUi 使用 `|` 分隔多个关键词以混合多个要素，字面意义上的混合，可以多个使用。

增强，用 `(提示词:权重)` 在 WebUi 权重增强，增强的范围是 `0.1 ~:100`，允许小数。NAI使用 `{}`

!!! tip
    `a ((((farm))), daytime` 的语法可能会吃掉逗号

渐变，使用 `[some1:some2:num]`

`[fantasy:cyberpunk:16]` 代表从第 16 step 后，使用 `cyberpunk` 标签替换 `fantasy`

`[to:when]` 在固定数量的step后添加 `to` 到提示 ( when)

`[from::when]` 在固定数量的step后从提示中删除 `from`( when)

转义，WebUi 对于带括号的参数，`a (word)`  请在参数中使用 `\` 字符转义为 `a \(word\)`。

降低权重，`[]` 或者 `(word:0.952)`，后者仅 WebUi

交替（alternate prompt）[^7]，这使您可以创建动物、人或风格的混合体，每一个 step 切换一个项，`[alison brie|emma stone|elizabeth olsen|scarlett johansson|anne hathaway|emma roberts], still film` 这是WebUi 语法，在 NAI 中是混合。

!!! tip "NAI"

    NAI中不允许单独指定权重，但支持混合权重 `cat:1|happy:-0.2|cute:-0:3` 这样的语法。
    
    因为NAI使用的是 WebUi 2022 年 9 月 29 日之前的实现，所以权重增强语法是旧的 `{}` ，新的 WebUi 更改为 `()`。
    
    **换算关系**
    NAI的 [word] = WebUi(word:0.952)(0.952 = 1/1.05)

    NAI的{word} = WeUi的(word:1.05)

    NAI花括号权重为1.05/个，WebUi圆权重为1.1/个
    
用 `{}` 在 NAI 做增强，注意NAI不支持单个词语指定权重，只支持


### Batch count&batch size

`batch count` 指定训练几批次图像。

`batchsize` 中文翻译为批大小（批尺寸）

简单点说，批量大小将决定我们一次训练的样本数目。 batch_size将影响到模型的优化程度和速度。

还要注意 `batch size` 是为了在`内存效率`和`内存容量`之间寻找最佳平衡，因为单个 512x512 图像生成中，GPU 资源没有被充分利用。更大的图像从中获得的收益更少。

!!! info
    迭代是重复反馈的动作，神经网络中我们希望通过迭代进行多次的训练以到达所需的目标或结果。
    每一次迭代得到的结果都会被作为下一次迭代的初始值。
    一个迭代 = 一个正向通过+一个反向通过


### 迭代步数

更多的迭代步数可能会有更好的生成效果，但是一定会导致生成时间变长。太多的 steps 也可能适得其反，几乎不会有提高。


### 采样器

观看前面章节对于采样器的介绍。

目前好用的有 `eular`，`eular a`，更细腻，和`Ddim`，DDIM 收敛较快,

一般推荐 `eular a` 和 `Ddim`


举个例子

![效果](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/raw_sample.jpg)

>不同 step 和 采样器 的不同效果


### 如何书写提示词(提示)

结合法术书编写它。

- 自然语言

可以直接使用自然语言，WebUi(SD) 有自然语言处理能力(英文句子),也可以使用 颜文字 和 emoji

- 参数[^6]

将你想要的相似的提示词组合在一起，并将这些按从最重要到最不重要的顺序排列

```
图片的质量
图片的主题
他们的外表
他们的情绪
他们的衣服
他们的姿势
图片的背景
```

[为文字转图像Ai提示编写指南：A Guide to Writing Prompts for Text-to-image AI](https://docs.google.com/document/d/1XUT2G9LmkZataHFzmuOtRXnuWBfhvXDAo8DkS--8tec/edit#)


**书写长度**

- 提示不要太长，超过 100 就有失败风险。

根据 手抄本的Tip，由于GPT-3模型限制，promot 并不是无限的，positive token 在 75-80 之间，negative 大概65，加太多会提示你 xxx of xxx are truncated，所以别人那边的圣经不要照抄，太长的咒语后半都没有意义了，所以用简易反咒就足够，除非你有特定想屏蔽的东西。


当提示超过75个`token`（比如150个`token`）时，WebUi 将分组提示词，提交多组75个 `token`。标记只具有同一集合中其他内容的上下文。这意味着您可能在第一组和第二组之间的边界处有`bule hair`，标记`blue`将在第一组中，`hair`将在第二组中。这导致了结果的不准确，因为这两个词是分开的。

新版本增加了一个选项 `Increase coherency by padding from the last comma within n tokens when using more than 75 tokens`

这个设置让程序试图通过查找最后N个标记中是否有最后一个逗号来缓解这种情况，如果有，则将所有经过该逗号的内容一起移动到下一个集合中。

!!! tip "比如"

    有词条为 `...,Comma,blue hair,PADDING,...`

    第75个词为 `blue`

    使用该选项前

    集合1:{[74]=Comma，[75]=blue}，集合2:{[76]=hair, [77]=PADDING}

    使用该选项后

    集合1:{[074]=Comma，[75]=PADDING}，集合2:{[76]=blue，[77]=hair}

    如果您的提示小于等于75个标记，不会发生分组。

**书写格式**

为了方便书写改动，多行书写的建议格式如下

```
masterpiece,
1girl,  hatsune miku, 
look at viewer,turning back,
blow wind, cyan hair, two side up, cyan eyes,eardrop,dress,
caustics, masterpiece, high resolution,
```

第一行指定作品风格类型，第二行指定人物，第三行指定动作，第四行指定场景和着装，第五放指定其他参数

以上顺序已经比较合理，但是你也可以调整提示词的顺序以产生不同的结果。 你可以手动调整，也可以 [使用 X/Y 图自动生成各种顺序](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1607)

Ai 难以解析下划线，请少用。

**调序编译**

A提示词放入的顺序就是优先级。

webui 突破 tag 75 个限制的方式是把 75 个分为一组。

推荐主体往前放，接着描述装扮的词，画质提升词穿插在这些描述词之间，一般为了提高成品率要把动作、nsfw 词等改变构图的词往后放，或者手动调低权重（主要是为了防止ai强行凑动作导致肢体到处跑）。

以上排序是每组tag都要遵守的，所以如果后面的tag超过 75 了就应该把前面的分一部分过来。


### (提示词)影响因子[^6]
 
"调参魔法" 的一个基本技能是设置权重。

[Wiki:魔导师介绍](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

具体规则如下

```
a (word) - 将权重提高 1.1 倍

a ((word)) - 将权重提高 1.21 倍（= 1.1 * 1.1），乘法的关系。

a [word] - 将权重减少 1.1 倍

a (word:1.5) - 将权重提高 1.5 倍

a (word:0.25) - 将权重减少 4 倍 (= 1 / 0.25)

a \(word\) - 在提示中使用文字 () 字符
```

必须使用 `()`指定，可以像这样指定权重：(text:1.4)

如果未指定权重，则假定为 `1.1`

指定权重仅适用于 `()` 而不是 `[]`，注意 `[]` 削减语法，指定单个权重仅适用于WebUi 且使用 `()`

```
> ( n ) = ( n : 1.1 )  
> (( n )) = ( n : 1.21 )  
> ((( n ))) = ( n : 1.331 )  
> (((( n )))) = ( n : 1.4641 )  
> ((((( n )))) = ( n : 1.61051 )  
> (((((( n )))))) = ( n : 1.771561 )  
```

`(cat :2 | dog)` 也就是更像猫的狗

在 WebUi 中需要使用 `()`指定权重！可以像这样指定权重：(text:1.4)。如果未指定权重，则假定为 1.1。



!!! info
    权重增加通常会占一个提示词位。在token紧张的情况下没有必要加特别多括号。
    
    过多圆括号会导致 字符 被程序吃掉，`a ((((farm))), daytime`会变成 `a farm daytime` 而没有逗号。
    
!!! tip "NAI"

    NAI中不允许单独指定权重，但支持混合权重 `cat:1|happy:-0.2|cute:-0:3` 这样的语法。
    
    因为 NAI 使用的是 WebUi 2022 年 9 月 29 日之前的实现，所以权重增强语法是旧的 `{}` ，新的 WebUi 更改为 `()`。
    
    **换算关系**
    NAI的 [word] = WebUi(word:0.952)(0.952 = 1/1.05)

    NAI的{word} = WeUi的(word:1.05)

    NAI花括号权重为1.05/个，WebUi圆权重为1.1/个


### 消极提示词

WebUi(SD)网页应用会在生成时**拒绝消极提示词有关的内容**。

比如使用以下提示词削除水印和文字内容

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


### 渐变

渐变标签，指示 WebUi 在训练中替换 Token，语法使用 `[some1:some2:num]`

`[fantasy:cyberpunk:16]` 代表从第 16 step 后，使用 `cyberpunk` 标签替换 `fantasy`

`[to:when]` 在固定数量的step后添加 `to` 到提示 ( when)

`[from::when]` 在固定数量的step后从提示中删除 `from`( when)

![sample_Gradient](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/sample_Gradient.jpg)

### 重现提示词

对于没有压缩的原图，我们可以将文件拖入 `PNG Info` 选项卡，进行提示词(Token)查看。


### 逆向提示词

这里有一些 **Image 逆向参数服务**，可以从图片中提取相关参数，不一定准确。

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)


### 已知提示词(Token)组合 / 搜索引擎

你可以访问以下传送门获取一些优秀的参数实例！（当然，中文社区的测试群有**大量素材**，一分钟20次）

[StableDiffusion_Show](https://t.me/StableDiffusion_Show)

[cyan_ai_sese](https://t.me/cyan_ai_sese)

[LEXICA搜索引擎](https://lexica.art/?q=Miku)


### **Prompt matrix 参数矩阵/要素混合**

使用 | 分隔多个Tag，程序将为它们的每个组合生成一个图像。 例如，如果使用 `a busy city street in a modern city|illustration|cinematic lighting` ，则可能有四种组合（始终保留提示的第一部分）：

- a busy city street in a modern city
- a busy city street in a modern city, illustration
- a busy city street in a modern city, cinematic lighting
- a busy city street in a modern city, illustration, cinematic lighting

可以进一步在关键词后添加 :x 来指定单个关键词的权重，x 的取值范围是 0.1~100，默认为 1。

`cat :2 | dog` 也就是更像猫的狗


### 参数冲突(提示词)

比如 `sex` 包含较多姿势体位，在使用者想要特定姿势时，法术内单一的 `sex` tag就应该被删除。

同样地，`loli` Tag 附带了强画风属性，会很大地影响结果！改成 `femeal child` 会好一点。


### 种子调试

在相同 Step ，cfg ，Seed,参数（prompts） 的情况下，生产的图片基本相同！

在同一模型和后端实现中，保持所有参数一致的情况下，相同的种子会产生同样的图片。取决于其他参数。

但是注意，**不同显卡可能会造成预料之外的不同结果**（比如精度这样的东西）

10xx 系列看起来与其他所有卡如此不同,见[这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3873467)

###  方位调参

竖着看

|人物|场景|数据限定|事件|
|----|------|-----|-----|
|表情| | | |
|头发|广狭选择|绘画类型|缩写词|
|眼睛|光影选择|评价限定|sfw/nsfw|
|衣着|背景主体|联想元素||
|状态|人物事件地|||
|姿势位||||
|镜头位||||


## 魔法进阶

调参基本原理模糊的说是：限定好的数据范围内相似样本越多，越稳定。

!!! tip
    请阅读前面章节的模型进阶1,了解具体的 Img2Img 和 inpaint 介绍操作。


### Img2Txt

生成按钮下有一个 `Interrogate CLIP`，点击后会下载 `CLIP`，用于生成当前图片框内图片的 Tag 并填充到提示词。

!!! tip 
    本文件为 [model_base_caption_capfilt_large.pth](https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_caption_capfilt_large.pth)
    
    大小为855MB


### Img2Img 介绍

一般我们有两种途径对图像进行修复：**PS 和 InPaint**，使用方法也十分多样。

!!! tip "不同之处"
    PS 重新绘画投入 Img2Img 的话，会导致画风的变动，而 Inpaint 就不会。


**CFG Scale**

`cfg scale` 就是符合 prompt 的程度,Scale越高，程序对提示词越忠诚，越符合。

**Denoising strength 降低噪声**

`Denoising strength` 决定算法对图像内容的保留程度,可以减少对画风的变得，但也会弱化img2img能力。值越高 AI 对原图的参考程度就越低 (同时增加迭代次数)。

对于以图做图来说，低 `denoising` 意味着修正原图，高 `denoising` 就和原图就没有大的相关性了。一般来讲阈值是 0.7 左右，超过 0.7 和原图基本上无关，0.3 以下就是稍微改一些。


###  Img2Img 三渲二

调整3D模型骨架比寻找样图更容易。

可以结合 **3D建模** 摆 Pose,可以使用 MMD 相关软件。

如果是真人图片，需要适当提高 `CFG Scale`。

推荐使用 blender 或者 Unity ，在对 3D 模型的测试中，**色彩主要影响 AI 的绘画效果**，所以你的模型需要有纹理。

不知道 VRchat 怎么样。


### Img2Img PS重绘画/修复手

使用PS软件增删元素，然后重新生产。这可以解决画手的问题。

比如我们可以通过 PS 给角色移植一个手让Ai来润色它。

或者涂鸦特定部位指定形状动作(比如衣料的覆盖率或者形状)

![info](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/00119_136826557_masterpiece%2C_best_quality%2C_1girl%2C_black_hair%2C_hat1.jpg)

>一张图片[^5]展现WebUI下img2img中不同参数下效果的详细对比图（prompt、steps、scale、各种seed等参数均保持一致）

纵轴是Denoising strength（线上版的strength），横轴是Variation strength


#### 重绘画技巧/去除/替换

- 首先要对人物描线，然后打上色块（如果有阴影，取亮色皮肤）。

- 变动强度，选择较低的 0.3 左右的去噪。

- 然后使用 Img2Img Inpaint + 相关提示词修复，不满意可以再改，直到满意。

- 然后对图像进行超分，降噪(去除图像纹理)。


### Img2Img **Outpainting 外部修补**

Outpainting 扩展原始图像并修复创建的空白空间。
您可以在底部的 img2img 选项卡中找到该功能，在 Script -> Poor man's outpainting 下。

```
Outpainting, unlike normal image generation, seems to profit very much from large step count. A recipe for a good outpainting is a good prompt that matches the picture, sliders for denoising and CFG scale set to max, and step count of 50 to 100 with Euler ancestral or DPM2 ancestral samplers.
```

### Img2Img **Inpainting 修补**

在 img2img 选项卡中，在图像的一部分上绘制蒙版，该部分将被重画。

对于 `Masked content` 设置，遮罩内容字段确定内容在修复之前放置到遮罩区域中。

一般选 `original`，可以保持潛空间一致性。

它们的效果如下:

| mask  | fill  | original   | latent noise      | latent nothing       |
|---------------------------|----------------|-----------------------|-------------------------|-----------------------|
| ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-mask.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-fill.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-original.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-noise.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-nothing.png) |

`mask` 横条决定了模糊程度，original 是`原图`，fill 是`填充底色`

Tip: `fill` 要更多 step 才能消除不自然感.

`Inpaint at full resolution` 即全分辨率修复。
通常，Inpaint 会将图像大小调整为*UI中指定的目标分辨率*。启用完全分辨率的Inpaint后，仅调整遮罩区域的大小，并在处理后将其**粘贴回**原始图片。这允许您处理大图片，并允许您以更大的分辨率渲染修复对象。

有几种方法进行重绘制:

- 在网络编辑器中自己绘制蒙版（`Inpaint masked `指重画涂鸦区域，`Inpaint not masked` 指重画涂鸦之外的区域）

- 在外部编辑器中擦除部分图片并上传透明图片。 任何稍微透明的区域都将成为蒙版的一部分。 请注意，某些编辑器默认将完全透明的区域保存为黑色。

- 将模式（图片右下角）更改为"Upload mask"并为蒙版选择单独的黑白图像(white=inpaint)。

![result](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting.png)

<iframe src="//player.bilibili.com/player.html?aid=474043788&bvid=BV1HK411Q7uk&cid=860273094&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>

通过这种方法，我们可以更改角色衣物风格或者其他任何细节。

<iframe src="//player.bilibili.com/player.html?aid=559044202&cid=859852841&page=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>

### Img2Img Loopback 回环生成

在 img2img 中设置loopback脚本，它允许自动将输出图像作为下一批的Batch提供，相当于保存输出图像，并用它替换输入图像。

Batch 数设置控制获得多少次迭代

通常，在执行此操作时，您会自己为下一次迭代选择许多图像中的一个，因此此功能的有用性可能值得怀疑，但反正我已经设法获得了一些我无法获得的非常好的输出。


### 超分图像 extras

`webui` extras 页有一个自带的超分功能，可以使用 `ESRGAN_4x`模型

当然 `realesrgan` 或者 `realcugan` 也可以，[realcugan](https://github.com/bilibili/ailab/tree/main/Real-CUGAN)

!!! tip "相关模型"

    文件统一下载到 `SDwebUI文件夹\models` 下

    [LDSR](https://heibox.uni-heidelberg.de/f/578df07c8fc04ffbadf3/?dl=1)
    
    文件大小为1.9GB

    [BSGRAN 4x](https://github.com/cszn/KAIR/releases/download/v1.0/BSRGAN.pth) 

    文件大小为63.9M

    [ESRGAN_4x](https://github.com/cszn/KAIR/releases/download/v1.0/ESRGAN.pth))
    
    文件大小为63.8MB

    [ScuNET GAN/PSNR](https://github.com/cszn/KAIR/releases/download/v1.0/scunet_color_real_gan.pth" to D:\stable-diffusio\models\ScuNET\ScuNET.pth)
    
    文件大小为68.6MB

    [SwinIR 4x](https://github.com/JingyunLiang/SwinIR/releases/download/v0.0/003_realSR_BSRGAN_DFOWMFC_s64w8_SwinIR-L_x4_GAN.pth) 
    
    文件大小为136MB

**应该使用什么 Upscaler？**

推荐 `SD Upscaler`, 它注重细节的同时还提升分辨率。

曾经有段时间，`LSDR` 被认为是最好的。有些人喜欢 swinir，有些喜欢`esrgan4x`，`ymmv`


### 图像去噪

推荐使用 [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) 降噪。

![效果](https://raw.githubusercontent.com/xinntao/Real-ESRGAN/master/assets/teaser.jpg)
>效果图


### DreamBooth 训练

DreamBooth 的模型是一种新的文本到图像“个性化”（可适应用户特定的图像生成需求）扩散模型

[Colab 在线训练模型](https://colab.research.google.com/drive/17yM4mlPVOFdJE_81oWBz5mXH9cxvhmz8#scrollTo=lJoOgLQHnC8L)，需要Pro套餐算力(18-24G显存需要)

[DreamBooth_Stable_Diffusion](https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb#scrollTo=K6xoHWSsbcS3)

[fast-DreamBooth](https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb)

https://github.com/XavierXiao/Dreambooth-Stable-Diffusion


### Textual Inversion 训练新角色

[官方英文说明和效果图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion)

`Textual Inversion`允许你在自己的图片上训练一小部分神经网络，并在生成新图片时使用结果。可以数据集没有新出的角色画不出的问题，模仿特定的艺术风格。

这对制作数据集没有囊括的 2021 年之后的动漫人物非常有效。（PS:相应的，Miku因为图多所以效果最好）


在许多情况下（例如不同的环境和姿势）对一个主题执行此操作通常可以让 AI 创建更好的嵌入

使用时，将 embedding(一个 .pt 或一个 .bin 文件) 放入`embeddings` 目录并在 prompt 提示词中提到你要用的 embedding 的文件名(*.pt)即可。不必重新启动程序即可使其正常工作。

没错，NAI 的 `hypernetworks` 就是超网络，用来做 embeddings（风格化）。

多 `embeddings` 可以一起用，启动时候会自动加载。

[相关 embeddings](https://gitlab.com/16777216c/stable-diffusion-embeddings)，里面有相关效果预览。

[list of Textual Inversion embeddings for WebUi(SD)](https://rentry.org/embeddings)

[HuggingFace 的embeddings库](https://cyberes.github.io/stable-diffusion-textual-inversion-models/)

### Textual Inversion 自训练[^7]

[英文:自己训练 embedding](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion#training-embeddings)

!!! danger "重命名 VAE 文件"

    重命名VAE模型文件非常关键，如果带着VAE训练效果十分差。

    或者不需要重命名vae文件，直接在设置里打开 `auto unload`


#### 模型环境要求

WebUi 应该是 Git 的最新版本。

显存至少 6GB，正常使用需要 12GB 显存。根据实验数据，8GB显存应该选择 `512x512` 分辨率。

训练可使用半精度浮点，但是否好用还需要进行实验。

如果你有足够的内存，那么在`--no-half --precision full`下运行会更安全。

你可以中断和恢复训练而不会丢失任何数据（AdamW 优化参数除外，但似乎现有的存储库都没有保存这些，所以一般认为它们并不重要）。

不支持 `batch sizes` 和 `gradient accumulation`。

不可能在 `--lowvram` and `--medvram` 参数下运行起来。

#### 设置

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


#### 创建训练

打开 `train` 选项卡，在 `Create embedding`选项卡新建一个 `embedding` 模型。

`Name` 输入框输入你预想的出现此人设的 提示词。

`Initialization text` 可以输入 `one girl` 这种大分类，只能填一个，人物可以是1girl或者1boy，或者画风。

`Number of vectors per token` 是此 `embedding` 要占据的 token 位数量，越多越好，但是相应也会减少其他提示词 token 的位置。

新建，会创建一个在 `embedding` 下的 pt 文件。


#### 预处理

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

#### 训练

在 `Train` 子选项卡中，选择你要训练的模型。

`Learning rate`(学习率)，决定训练的速度。Learning rate越低学习越慢（花费更长的时间收敛）。


一般为 0.005。，如果想快一些，可以使用 0.001 加快，但是如果设置得太高，可能会破坏 `embedding`

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

如果太多会过拟合(可以理解为Ai的死板)

随时观察，如果过拟合，可以停止。如果效果不是很好，可以去找早些时候的模型继续训练。找到一个好的效果。

!!! danger "重命名 VAE 文件"
    重命名VAE模型文件非常关键，或者确定你已经设置里打开 `auto unload`。
    
    如果带着VAE训练效果十分差。
    

点击 右下角训练，等待。

训练完毕。再次重命名 Vae 文件，重启程序。




#### 备注

**[filewords]**

是 提示词模板文件 的 Tag，可以实现把文件名中的词汇插入提示。

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


### 训练 Hypernetworks

Hypernetworks 是一种新颖的概念，用于在不触及任何权重的情况下微调模型。

你可以在 `Textual Inversion` `train` 选项卡训练Hypernetworks

训练的工作方式与文本倒置相同。

唯一的要求是使用非常非常低的学习率，例如 0.000005 或 0.0000005。

[应该查看的英文指南](https://rentry.org/hypernetwork4dumdums)

[相关的英文讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)


![hypernet对比图](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/exp_hypernet.jpg)
>hypernet对比图


### **渐变提示词**

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


### 注意 `尺寸`

比如出图尺寸宽了人可能会多

!!! tip
    要匹配好姿势，镜头和人物才不畸形，有时候需要限定量词，多人物时要处理空间关系和 prompt 遮挡优先级。人数->人物样貌->环境样式->人物状态
    


### 提示词速查

**[手抄本法术书](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/edit)**

**[Danbooru全部Tag列表](https://gelbooru.com/index.php?page=tags&s=list)**

**[参数法术全典](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml#)**

**[Tag在线协作](https://docs.google.com/spreadsheets/d/1zij5OzCZIaQuhAbiSayhFznjgJ3rwbaNwnUnaUMxyTQ/edit?usp=drivesdk)**

**[NSFWTag](https://github.com/scooderic/exhentai-tags-chinese-translation)**

**[Ai艺术家文档](https://docs.google.com/spreadsheets/d/1SRqJ7F_6yHVSOeCi3U82aA448TqEGrUlRrLLZ51abLg/htmlview#)**

**[Novelai 关键词组合器](https://www.bilibili.com/read/cv19023021)**


### 调参工程师

[https://github.com/Maks-s/sd-akashic](https://github.com/Maks-s/sd-akashic)

[https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)


## 参数

### 自动补全

这里有一个项目，可以使SD做到类似 NAI 的补全效果。

https://github.com/DominikDoom/a1111-sd-webui-tagcomplete


### NAI 官方在使用的参数

个人跑不起来的参数

- 使用全量模型(官方的GPU云特别强悍)
- 使用 ema 权重加载，将yaml 配置其中的 `use_ema` 设置为 true
- CLIP layer = 2
- 将` sigma noise/strength`重置为默认值 1（无需使用 0.69 / 0.67）
- 设定 `eta noise seed delta` 为 31337
- 如果使用欧拉 a，eta 噪声种子 delta = 31337
- 如果prompt有权重，转换权重（WebUi用1.1，NAI用1.05）
- 使用 --no-half 参数（次要）


**NAI 默认的 `SFW` 消极提示词为**
```
nsfw, lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry
```

**NAI 默认的模型设置**
```
steps": 28, "sampler": "[sampler]", "seed": [seed], "strength": 0.69, "noise": 0.667, "scale": 11.0,

Strength ， noise 是 eta 和 sigma

scale 就是 CFG scale
```


**其他**

将所有提示词前面加入 `masterpiece, best quality`


Clip 跳过 0，其他一切都很好（afaik 不要使用超网络、v2、yaml、VAE）

### 转换——NAI和WebUi(SD)的增强语法不同

**Prompts 参数括号转换**

在 NAI 和 Webui 之间转换加强参数，相关的机器人服务 [M2NM2NBot](https://t.me/M2NM2NBot)

权重增强标识：NAI 是 `{}` ，WebUi(SD) 是 `()`


### 良好参数(风格趋向插画)[^4]

```
{an extremely delicate and beautiful}
```


#### 草图风格

| 词                                                            | 描述                                               |
|---------------------------------------------------------------|----------------------------------------------------|
| sketch                                                        | 可以让图片看起来像随手画的草稿                     |
| {{lineart}}                                                   | 可以让线条变得很粗                                 |
| {{{posing sketch}}}, {{monochrome}}                           | 黑白草图                                           |
| {rough sketch}                                                | 上了颜色的草图                                     |
| monochrome+lineart                                            | 情况下一般只会让眼睛上色，强调发色后头发也可以上色 |
| {{{monochrome}}}, {{{gray scale}}}, {{{pencil sketch lines}}} | 做出的铅笔速写的感觉                               |


利用sketch，pastel color，lineart的tag模拟一张图的绘画过程


#### 艺术风格

| 词                                                                  | 描述                                   |
|---------------------------------------------------------------------|----------------------------------------|
| chibi                                                               | 可以画出低头身比的效果(二头身, 三头身) |
| {{watercolor pencil}}                                               | 可以生成彩铅画                         |
| {{faux traditional media}}                                          | 可以做出签绘的风格                     |
| anime screeshot，                                                   | 可以让画面变成动画风格                 |
| {{{retro artstyle}}}                                                | 赛璐璐风                               |
| {photorealistic}, {painting}, {realistic}, {sketch}, {oil painting} | 厚涂                                   |
| pastel color和sketch                                                | 搭配会有速涂的质感                     |


#### 杂志/设定集 风格

| 词                                                             | 描述                                                           |
|----------------------------------------------------------------|----------------------------------------------------------------|
| official art                                                   | 变得更加官方一点                                               |
| three views from front, back and side和costume setup materials | 可以用来生成设定图                                             |
| multiple views                                                 | 会出现类似设定图                                               |
| {character sheet}                                              | 会出现设定图                                                   |
| magazine cover                                                 | 会把背景换成杂志封面, 配合office art更像真实杂志(虽然字没法看) |
| magazine scan                                                  | 类似杂志内页的风格                                             |
| posing                                                         | 会强调有一个动作, 不至于出现混乱的动作(露出有六个手指头的手)   |
| caustics                                                       | 画面向主题聚焦, 类似海报                                       |


### 常用参数:SFW

| 人物数量 | 描述                                                                  |
| ----------- | ----------------------------------------------------------------------- |
| 数量      | , one boy , one girl , two boy ,two girl,one_boy_one_girl(这是错误的) |


| 人物画风                                         | 描述 |
| -------------------------------------------------- | ------ |
| 质量提升参数 |   , masterpiece, best quality   |
| 原神                             |   , Genshin Impact   |
| 萝莉                  |    , female child , loli画风差  |

| 人物样貌                           | 描述                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| 头发                               | hair                                                           |
| 长发                               | longhair                                                       |
| 短发                               | shorthair                                                      |
| 眼睛                               | eyes                                                           |
| 渐变颜色长发                       | gradient pink longhair                                         |
| 渐变颜色眼睛                       | gradient pink eyes                                             |
| 粗眉毛                             | thick eyebrows                                                 |
| 猫尾巴                             | cat tail                                                       |
| 猫耳朵                             | cat ears                                                       |
| 动物耳朵                           | animal ears                                                    |
| 毛茸茸的动物耳朵                   | animal ear fluff                                               |
| 刘海                               | bangs                                                          |
| 两眼之间的头发                     | hair between eyes                                              |
| 眉毛后面的头发                     | eyebrows behind hair                                           |
| 锁骨                               | collarbone                                                     |
| 斗篷(要在很前面才有效)             | cape                                                           |
| 乳房尺寸                           | small breasts                                                  |
| 出汗                               | sweating                                                       |
| 颜色丝袜(和长丝袜冲突)             | white stockings , black stockings                              |
| 长丝袜                             | thighhighs                                                     |
| 女仆                               | maid                                                           |
| 发带                               | ribbon                                                         |
| 爱心眼                             | heart-shaped pupils                                            |
| 御姐/JK/辣妹?                      | gyaru                                                          |
| 肌肉发达                           | muscular                                                       |
| 天使翅膀(要是形容人的第一个才正常) | angel wings                                                    |
| 颜色内裤(赠内衣)                   | pink underpants                                                |
| 肚脐                               | navel                                                          |
| 颈部颜色项圈                       | white collar                                                   |
| 黑色皮肤                           | dark skin                                                      |
| 撕裂的衣服                         | torn clothes                                                   |
| 撕裂的裤子                         | torn legwear                                                   |
| 开襟夹克(配合叉开腿特色)           | open jacket                                                    |
| 异色瞳                             | heterochromia_blue_red                                         |
| 吊袜带(会和内衣冲突)               | garter straps                                                  |
| 靴子                               | boots                                                          |
| 眼罩                               | blindfold                                                      |
| 流泪                               | tears                                                          |
| 项链                               | necklace                                                       |
| 眼镜                               | glasses                                                        |
| 比基尼                             | bikini                                                         |
| 湿衣服                             | wet clothes                                                    |
| 透明衣物                           | transparent raincoat , transparent jacket , transparent tshirt |
| 唾液(自动伸舌头)                   | saliva                                                         |
| 流口水(和唾液冲突)                 | drooling                                                       |
| 水手服                             | sailor dress                                                   |

| 环境样式                                                                 | 描述                           |
| -------------------------------------------------------------------------- | -------------------------------- |
| 在床上                                                                   | on bed                         |
| 光线反射                                                                 | reflection light               |
| 赛博朋克                                                                 | cyberpunk, city, kowloon, rain |
| 在地毯上                                                                 | on carpet                      |
| 在瑜伽垫上(它分不清什么是瑜伽垫，只知道色块比较大，所以要配合one girl用) | on_yoga_mats                   |

| 人物视角     | 描述        |
| -------------- | ------------- |
| 正面视角     | from viewer |
| 从上到下视角 | from below  |
| 全身         | full body   |

| 人物状态           | 描述                                                 |
| -------------------- | ------------------------------------------------------ |
| 叉开腿             | spread leg                                           |
| 露出腋下           | armpits                                              |
| 举起手             | hands up , arms up                                   |
| 爪子手             | paw pose                                             |
| 站立               | standing                                             |
| 行走               | walking                                              |
| 吐舌头             | tongue out                                           |
| 抬起腿             | legs up                                              |
| 手放背后           | arms behind back , hidden hands                      |
| 衬衫               | shirt                                                |
| 长袖               | long sleeves                                         |
| 连帽衫             | hoodie                                               |
| 褶边               | frills                                               |
| 喇叭裤             | bloomers                                             |
| 白色连衣裙         | white dress                                          |
| 捆绑               | bondage , bondage body , bondage foot , bondage hand |
| 蹲下               | crouch , squatting                                   |
| 真画风           | photorealistic                                       |
| 跪下               | kneel down                                           |
| 湿身               | wet body                                             |


[^4]:[Paper朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)
