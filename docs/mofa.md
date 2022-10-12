# 调参魔法

这节会介绍 参数 和 相关的WebUi(SD)  网页应用资源。部分内容只做简单介绍，因为前面有相关描述。

先请拿上 [调参魔法书](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml)和 [手抄魔法本](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/)。


## 基本流程

画一些非常粗糙的东西，比如涂鸦

带提示词参数生产 img2img

涂鸦原始 img 或提示词参数或输出输入

重复 img2img / inpaint 涂鸦后修复

重复最后几个步骤，直到完成


## 魔法入门

请先在前面了解一下WebUi(SD)网页应用的参数。

### 提示词语法简介

**表示方法**

用 英文半角 `,` 做分隔符，用 `{}` 在 NAI 做增强，用 `()` 在 WebUi 做增强。

>在英文逗号前后存在一个或数个空格并不影响实际使用。

[fantasy:cyberpunk:16] 代表从第 16 step 后，使用 cyberpunk 标签

[to:when] 在固定数量的step后添加to到提示 ( when)

[from::when] 在固定数量的step后从提示中删除from( when)

使用 `|` 分隔多个关键词以混合多个要素，字面意义上的混合，可以多个使用

`cat :2 | dog` 也就是更像猫的狗



### 迭代步数

更多的迭代步数可能会有更好的生成效果，但是一定会导致生成时间变长。太多的steps也可能适得其反，几乎不会有提高。

### 采样器

观看前面章节对于采样器的介绍。

目前好用的有 `eular`，`eular a`，更细腻，和`Ddim`

一般推荐 `eular a` 和 `Ddim`


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

- 对于带括号的参数，`a (word)`  请在参数中使用 `\` 字符转义为 `a \(word\)`



**书写长度**

- 提示不要太长，超过 100 就有失败风险。

根据 手抄本的Tip，由于GPT-3模型限制，promot并不是无限的，positive token 在75-80之间，negative大概65，加太多会提示你xxx of xxx are truncated，所以别人那边的圣经不要照抄，太长的咒语后半都没有意义了，所以用简易反咒就足够，除非你有特定想屏蔽的东西


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

Ai 难以解析下划线，请少用

**调序编译**

tag 顺序是有影响的，webui突破tag75个限制的方式是把75个分为一组。

然后是主体往前放，接着描述装扮的词，画质提升词穿插在这些描述词之间，一般为了提高成品率要把动作、nsfw词等改变构图的词往后放，或者手动调低权重（主要是为了防止ai强行凑动作导致肢体到处跑）。

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

使用 `()`，可以像这样指定权重：(text:1.4)

如果未指定权重，则假定为 `1.1`

指定权重仅适用于 `()` 而不是 `[]`，注意 `[]` 是降低权重

```
> ( n ) = ( n : 1.1 )  
> (( n )) = ( n : 1.21 )  
> ((( n ))) = ( n : 1.331 )  
> (((( n )))) = ( n : 1.4641 )  
> ((((( n )))) = ( n : 1.61051 )  
> (((((( n )))))) = ( n : 1.771561 )  
```


!!! info
    权重增加通常会占一个提示词位。在token紧张的情况下没有必要加特别多括号。
    
    **NAI官方**

    在NAI官方前端我们使用 `{}` 来指定权重。

### 消极参数(提示词)

WebUi(SD)网页应用会在生成时**拒绝消极参数(提示词)有关的内容**。

比如使用以下参数(提示词)削除水印和文字内容

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

### 重现参数(提示词)

对于没有压缩的原图，我们可以将文件拖入 `PNG Info` 选项卡，进行参数(提示词)查看。

### 逆向参数(提示词)

这里有一些 **Image 逆向参数服务**，可以从图片中提取相关参数，不一定准确。

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)

### 已知参数(提示词)组合 / 搜索引擎

你可以访问以下传送门获取一些优秀的参数实例！（当然，中文社区的测试群有**大量素材**，一分钟20次）

[StableDiffusion_Show](https://t.me/StableDiffusion_Show)

[cyan_ai_sese](https://t.me/cyan_ai_sese)

[LEXICA搜索引擎](https://lexica.art/?q=Miku)

### 要素混合 

使用 | 分隔多个关键词以混合多个要素，前面有讲哦

可以进一步在关键词后添加 :x 来指定单个关键词的权重，x 的取值范围是 0.1~100，默认为 1

### 参数冲突(提示词)

比如 `sex` 包含较多姿势体位，在使用者想要特定姿势时，法术内单一的 `sex` tag就应该被删除。

同样地，`loli` Tag 附带了强画风属性，会很大地影响结果！改成 `femeal child` 会好一点。

### 种子调试

在相同 Step ，cfg ，Seed,参数（prompts） 的情况下，生产的图片基本相同！

在同一模型和后端实现中，保持所有参数一致的情况下，相同的种子会产生同样的图片。取决于其他参数

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


### Img2Img 图转图 3转2绘图/优化/换场景/修复

如果手脚残，可以试试 img2img,复制参数和 seed 可以进行调整。

**CFG Scale**

`cfg scale` 就是符合 prompt 的程度,Scale越高，程序对提示词越忠诚，越符合。


**Denoising strength 噪声**


`Denoising strength` 决定算法对图像内容的保留程度,可以减少对画风的变得，但也会弱化img2img能力。值越高 AI 对原图的参考程度就越低 (同时增加迭代次数)。


![info](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/00119_136826557_masterpiece%2C_best_quality%2C_1girl%2C_black_hair%2C_hat1.jpg)

>一张图片[^5]展现WebUI下img2img中不同参数下效果的详细对比图（prompt、steps、scale、各种seed等参数均保持一致）

纵轴是Denoising strength（线上版的strength），横轴是Variation strength



### inpaint 修复

把有缺陷的地方mask出来inpaint，图片上涂鸦地区会重画。一般选 `original`,`fill` 要更多 step 才能消除不自然感.


### Textual Inversion


`Textual Inversion`允许加载一个增强包神经网络。在许多情况下（例如不同的环境和姿势）对一个主题执行此操作通常可以让 AI 创建更好的嵌入.

使用时，将 embedding(一个 .pt 或一个 .bin 文件) 放入`embeddings`目录并在 prompt 提示词中提到你要用的 embedding 的文件名(*.pt)即可。

没错，NAI 的 `hypernetworks` 就是超网络，用来做 embeddings（风格化）。

给出相关 [embeddings](https://gitlab.com/16777216c/stable-diffusion-embeddings)，里面有相关效果预览。

[list of Textual Inversion embeddings for WebUi(SD)](https://rentry.org/embeddings)




### 注意 `尺寸`

比如出图尺寸宽了人可能会多

!!! tip
    要匹配好姿势，镜头和人物才不畸形，有时候需要限定量词，多人物时要处理空间关系和 prompt 遮挡优先级。人数->人物样貌->环境样式->人物状态
    
### 超分图像 extras

`realesrgan` 或者 `realcugan` 就可以，推荐 [realcugan](https://github.com/bilibili/ailab/tree/main/Real-CUGAN)

另外，`webui` extras 页有一个自带的超分功能，可以去`settings-upscaling` 使用 `R-ESRGAN 4x+ Anime6B`模型，重新启动程序

### 提示词速查

**[手抄本法术书](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/edit)**

**[Danbooru全部Tag列表](https://gelbooru.com/index.php?page=tags&s=list)**

**[参数法术全典](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml#)**

**[Tag在线协作](https://docs.google.com/spreadsheets/d/1zij5OzCZIaQuhAbiSayhFznjgJ3rwbaNwnUnaUMxyTQ/edit?usp=drivesdk)**

**[NSFWTag](https://github.com/scooderic/exhentai-tags-chinese-translation)**

**[**Ai艺术家文档](https://docs.google.com/spreadsheets/d/1SRqJ7F_6yHVSOeCi3U82aA448TqEGrUlRrLLZ51abLg/htmlview#)**

**[Novelai 关键词组合器](https://www.bilibili.com/read/cv19023021)**


### 调参工程师

[https://github.com/Maks-s/sd-akashic](https://github.com/Maks-s/sd-akashic)

[https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)


## 参数

### 转换——NAI和WebUi(SD)的增强语法不同

**Prompts 参数括号转换**

在 NAI 和 Webui 之间转换加强参数   [M2NM2NBot](https://t.me/M2NM2NBot)

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
