#  实战指南

这个简短的实战指南，可以让你快速了解如何合理调整参数达成目的效果。

目前， Ai 处理不好细节和多人物，如果一个个处理，光影结构就会乱掉。如果修图，不如重画。但是画背景和做预览很实用。

核心逻辑就是缩小预选数据的范围，调节天平在消极和积极提示之间的方向，还有使用部分语法控制提示词效果的强烈程度和起效过程。


## 场景表

竖着看

|人物|场景|数据限定|事件|
|----|------|-----|-----|
|表情| | | |
|头发|广狭选择|绘画类型|缩写词|
|眼睛|光影选择|评价限定|sfw/nsfw|
|衣着|背景主体|联想元素| 出品公司 |
|状态|人物事件地|  3D/2D  |  作品品牌  |
|姿势位|  |  |         |
|镜头位|  |  |         |


## 画作焊接！

通过 [AiPhotoShop-无限外延画布的在线工具](https://www.painthua.com/) ，你可以连续 “焊接” 作品，非常方便。只需要在启动命令加上 `--api` 参数，然后打开网页就可以使用。

[Github](https://github.com/BlinkDL/Hua)


## 遏制风格污染

介绍如何在 WebUi 中生成图片时，将主体作为重点，而不是画风。[^12]

为了得到预期结果，我们将 `style` 改为 `[style:10]` 

Automatic 1111 的 WebUi 提示词语法中，[x:10] 告诉 稳定扩散 直到第十个 step 再渲染这个提示词。

当达到10步的时候，一个基本的图像已经形成了，所以风格化肆意污染主体成分，而是像在现有图像上进行 IMG2IMG(你可以把它看成是Img2img的形式)

你可以根据你所做的整体步骤的多少来试验这个数字。如果你把格式改为 [x:0.2]（即低于1的数字），那么它就变成了百分比，[x:0.6]告诉 SD 等到总步骤数的 60% 时再渲染 x ，以此类推。


## 图转图的秘籍

无论是 3D(DAZ这样的3D模型) 还是线稿，AI只识别 **色彩** ，而不是线条，色彩直接决定图转图的效果。


## 尺寸选择

不应该将其与画质挂钩，尺寸一定程度上影响了主题，因为它潜在代表选择的类别(比如竖屏人物，横屏风景，小分辨率表情包居多)。

画质可以使用 超分指南 进行操作。

## 视角

[推荐使用 Danbooru含有的术语](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

![shot](https://user-images.githubusercontent.com/75739606/198682933-25c1b2df-c573-44ea-aba7-2df4609299e5.png)
<!--
![shot](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/shot.png)
-->
>引用来自日本 Wiki 的图片，作者不明

`extreme closeup` ECU

`close up` CU

`medium close up` MCU

`medium shot` MS

`cowboy shot` CS

`medium full shot` MFS

`full shot`  FS


## 琐碎的细节

- 表情符号

emoji(💰💶💷💴💵,🎊🪅🪄🎀,👩‍🚀) 表情符号也是可以使用并且 **非常准确** 的，经过我多次测试，Ai 可以认识表情符号。

emoji 因为只有一个字符，所以在语义准确度上表现良好。

[表情符号参考](https://unicode.org/emoji/charts/emoji-list.html)

- 动漫风格

`anime coloring` 会有很好的效果，像动漫截图一样。(如果遇到黑白色，需要指定人物部件的色彩)

<iframe src="//player.bilibili.com/player.html?aid=304175820&bvid=BV1TP411N71t&cid=865253876&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>


### 优化作品效果

- 对目标进行审视

想一想，自己想要的作品，在互联网上会被打上什么标签呢？

贴纸肯定不会打上 `masterpiece, best quility,` 吧。

另外就是如果要求的是像素作品，应该移除一些冲突的消极提示(如果有的话)。


- 语义偏移防范

为了防止语义偏移，优先考虑 emoji,然后少用不必要的 `with` 一类的复杂语法。

- 彩虹混乱图

如果输出混乱的彩虹色图片，可能是生成分辨率被设置得太低。

- 波浪噪点图

密集的波浪或图案，可能是因为权重设置太高了。

- 抠图？

扣过的图，其余背景都会被处理成黑色。

如果你需要背景，效果一般不是很好，建议填色后再操作。

- 粗糙的噪声

细节不够，人物像米糕一样。需要适当提升 step.

- 保护图片不被分割线切割

消极提示加入

```comic 2koma 3koma 4koma collage```

应该可以改善。


## 跨领域术语/奇门遁甲

跨领域术语的核心逻辑就是**缩小指定的数据范围**，从画面内容之外的**平台，领域，事件**上入手来提升效果。

是的！你可以在提示中使用 [Film Glossary](https://www.owlnet.rice.edu/~engl377/film.html) [FILM GLOSSARY摄影术语](http://userhome.brooklyn.cuny.edu/anthro/jbeatty/COURSES/glossary.htm)， [Cinematic techniques摄影技术](https://en.wikipedia.org/wiki/Category:Cinematic_techniques)，以及绘画术语(类型) 来控制基本情况。

跨领域！你甚至可以使用各种惊险运动的名词来生产一些特效....比如空降

比如，景深，光圈，构图，拍摄机位，运动元素，[艺术摄影术语表中文介绍](https://gallerix.asia/pedia/photography-glossary/)

**但是**这种效果可能会带来附加作用:引入你不希望见到的风格(如实景**而不是**而二次元)数据进入图片。把握好量度。适当增加 Step 和 风格提示 来改善.

你还可以使用**平台名**来限定数据集的范围，比如 pixiv 之类的词汇。

**扩展阅读**

[推荐使用 Danbooru含有的术语](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

[Danbooru tag组](https://danbooru.donmai.us/wiki_pages/tag_groups)

有用的电影术语 https://en.wikipedia.org/wiki/Category:Cinematic_techniques

镜头类型 https://www.bhphotovideo.com/explora/video/tips-and-solutions/filmmaking-101-camera-shot-types

电视术语  https://en.wikipedia.org/wiki/Category:Television_terminology

摄影类型 https://en.wikipedia.org/wiki/Category:Photography_by_genre

摄影术语 https://zh.wikipedia.org/zh-cn/%E9%AB%98%E9%80%9F%E6%91%84%E5%BD%B1

极限运动 https://en.wikipedia.org/wiki/Extreme_sport 

https://en.wikipedia.org/wiki/Category:Sports_by_type

构图艺术 https://en.wikipedia.org/wiki/Composition_(visual_arts)


## 迭代草图[^8]

这里讨论一下如何将**手绘草图**通过 Ai 绘画优化，*注意不是二次元*。

在第一次迭代中，不需要太多 Steps，CFG 可以非常低（以获得更好的多样化结果），如果不想完全丢失草图，Denoising 应该在 0.3-0.4 左右。

在最后的迭代中，增加 Steps 和 Denoising 强度（但不超过 0.8，否则图像将被破坏，尤其是在大于 512*512 时）请参见[这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2213#issuecomment-1274137775)，同时根据需要提高 CFG 和尺寸。

你可以随时修复提示（添加或删除出现的细节）并尝试不同的采样器。

另外，你不应该在初次生成使用一个固定不变的种子？

如果你提供一个种子（而不是随机的 -1），你的图像很快就会变得过饱和、过度锐化、过度像素化..... 当然如果想微调，可以使用固定种子。


## 表情控制

我们可以使用 颜文字 来控制出图的表情！

:-) 微笑 :-( 不悦 ;-) 使眼色 :-D 开心 :-P 吐舌头 :-C 很悲伤 :-O 惊讶 张大口 :-/ 怀疑

仅支持西方颜文字，详细内容请见 [Danbooru颜文字部分](https://danbooru.donmai.us/wiki_pages/tag_group%3Aface_tags) 或 [维基百科](https://zh.wikipedia.org/wiki/%E8%A1%A8%E6%83%85%E7%AC%A6%E8%99%9F%E5%88%97%E8%A1%A8?oldformat=true)


## 多人物/单人物

打草图+IMG2IMG,这就秘诀～

宽幅画作单人物生成最好打草图，进行色彩涂抹，确定画面主体。

多人物确定人物数量，最好使用草稿/有色3d 排列 + 图生图。

人数超过三个就难以控制效果，人数大于6的图像模型里估计没有...


## 进行手掌修复

将图片送入 inpaint，使用大致相同的提示词，将关于 `手` 的提示放在前面，根据你希望它变动多少来设置降噪（如果只是希望手更完整，调至0.25以下)，然后保留步骤和 CFG 与 txt2img gen 相同。

或者仅遮住手部，以全分辨率修复，大大降低填充（它使用周围的像素来创建上下文，但只是在重新制作手部）并仅提示手部问题（详细的手部描写等）

CFG 越高，越符合提示词，降噪越高越偏离原图。


## 同人物&差分

需要用到进阶的 Img2Img 相关内容，最好的方法是准备一个带有色彩的 3D 母本模型，然后这样就可以保证基本一致。

也可以用很多提示词来限制角色内容，出很多张，挑能用的作品。

如果是表情或者是背景，可以采用进阶教程中的 重绘画 技巧。

如果你想了解一些差分的实例，[5CH日语Wiki](https://seesaawiki.jp/nai_ch/d/%c7%ed%a4%ae%a5%b3%a5%e9%a5%c6%a5%af) 提供了一个实例。


## 复刻

如果想在生成上复刻，需要相同的 `seed` ，提示词，和其他参数(甚至尺寸)。不过如果对方使用了风格化，那么你就几乎无法复刻。

--自欺欺人式复刻：图转图，降噪拉到到 0.3 以下...--


## 使用 Ai 进行设计

- 背景

拿到一张饱和度稍微高的照片后，可以用 AI 进行处理，然后当作背景使用。使用 `anime coloring` 来增强动漫风格。

- 图标

[看这个](https://www.bilibili.com/video/BV1384y1B7MX)

- 表情包

表情包效果真的一级棒。 `sticker` 配合 西方颜文字 完成度超高！

文档编写者试了一下，以下 prompt 可以出高质量表情包。

不要对贴纸要求质量提示词。

**积极词汇**

```
1girl,sticker,Chibi,Q,
white background,
```
`masterpiece,best quality` 可加可不加，质量不够就加

`lowres` 可加可不加，不够复古就加

`Chibi` 控制角色为 Q版(大头小身)，不够 Q 就加

`sticker` 是指定风格为社交软件贴纸，必须

**消极词汇**

```
bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, jpeg artifacts, signature, watermark, username, blurry, bad feet,3D
```

low quality 可加可不加，如果想更加平面化，请加入 3D 到负面提示。

尺寸要求正方形。

- 立绘

<iframe src="//player.bilibili.com/player.html?aid=559362671&bvid=BV14e4y1U7r9&cid=869144379&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
>BV14e4y1U7r9


## 参数

为什么不去 [这里](https://danbooru.donmai.us/wiki_pages/howto%3Atag_checklist) 看[原始数据站点](https://danbooru.donmai.us/wiki_pages/tag_groups)的参数呢？

[E 站标签翻译项目](https://github.com/EhTagTranslation)


### NAI 在使用的出图参数

- 使用全量模型(官方的GPU云特别强悍)

- CLIP layer = 2

- 使用 ema 权重加载，将yaml 配置其中的 `use_ema` 设置为 true

- 将 ` sigma noise/strength` 重置为默认值 1

- 设定 `eta noise seed delta` 为 31337（使 ` sigma noise/strength` 无需使用 0.69 / 0.67）

- 如果 prompt 有权重，转换权重（ WebUi 占比 1.1 ，NAI 占比 1.05）

- 使用 `--no-half` 参数启动程序（次要）


**NAI 默认的模型设置**

```
steps": 28, "sampler": "[sampler]", "seed": [seed], "strength": 0.69, "noise": 0.667, "scale": 11.0,

Strength ， noise 是 eta 和 sigma

scale 就是 CFG scale
```

**NAI 默认的 `SFW` 消极提示词为**
```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry
```


**其他**

将所有提示词前面加入 `masterpiece, best quality`


Clip 跳过 0，其他一切都很好（afaik 不要使用超网络、v2、yaml、VAE）


### 转换——NAI和WebUi(SD)的增强语法不同

**Prompts 参数括号转换**

在 NAI 和 Webui 之间转换加强参数，相关的机器人服务 [M2NM2NBot](https://t.me/M2NM2NBot)

相关的 [网页JS](https://github.com/naisd5ch/novel-ai-5ch-wiki-js)

权重增强标识：NAI 是 `{}` ，WebUi(SD) 是 `()`


### 元素法典

元素法典提供了一个即查即用的模板库(类比作文大全)，里面有Tag的调试记录，方便快捷。

[元素法典一卷](https://docs.qq.com/doc/DWHl3am5Zb05QbGVs)

[元素法典第一点五卷](https://docs.qq.com/doc/DWGh4QnZBVlJYRkly)

[元素法典制作委员会](https://space.bilibili.com/1981251194)


### 良好参数(风格趋向插画)[^4]

```
{an extremely delicate and beautiful}
```

[绘画媒介全分类](https://danbooru.donmai.us/wiki_pages?commit=Search&search%5Btitle_normalize%5D=%2A_%28medium%29)

[动漫人物/艺术家/风格化列表/Pt文件](https://rentry.org/anime_and_titties)

[风格化：人偶教室](https://www.yuque.com/longyuye/lmgcwy)

[风格化，日语Wiki](https://seesaawiki.jp/nai_ch/d/%b2%e8%c9%f7%a1%a6%b9%bd%bf%de)

[风格化: 32种](https://www.bilibili.com/video/BV1TP411N71t/)

[艺术家列表/SD1.4](https://rentry.org/artists_sd-v1-4)

[艺术家列表/SD1.4/1,833 位艺术家](https://www.urania.ai/top-sd-artists)

[艺术家博物馆](https://gallerix.asia/storeroom/)

[稳定扩散的艺术家风格研究](https://proximacentaurib.notion.site/e28a4f8d97724f14a784a538b8589e7d?v=42948fd8f45c4d47a0edfc4b78937474)

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


## 市场应用情况调查

这里是稳定扩散（非 NAI 模型）的应用情况。

https://www.reddit.com/r/StableDiffusion/comments/yh8j0a/ai_art_is_popular_and_makes_money_confessions_of/


- 3D

在 blender 上，Ai 有 [渲染插件](https://blendermarket.com/products/ai-render/?ref=110)

- 设计

[Microsoft 365 工具套件](https://www.xda-developers.com/microsoft-designer-image-creator-ai-dall-e-2/)

[为 Age of Empires 3 Definitive edition 的游戏模组生成肖像](https://github.com/matrix4767)

[产品和架构设计/素描](https://github.com/horribleCodes)

- 专辑图

[用稳定扩散生成歌手的图像放入视频](https://github.com/Chilluminati91)

- 配图

[制作怪图作为记忆工具](https://github.com/ClashSAN)

[稳定扩散的真实用例](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3219)

[稳定扩散的应用结合](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3572)

- 自媒体

哔哩哔哩已经一大堆了，抖音也不少，可以做的话题很多

- 套壳

无良某些公司套壳开源项目，比如 某某画廊，某某版图。

(题外：韭菜采收容易，可用于园艺，在世界广泛种植)

- 插图/背景

小说插图，AI画背景(据说原版模型也很好用)

- NFT

？



[^4]:[Paper朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)

[^12]:[a_simple_method_explained_in_the_comments_to](https://www.reddit.com/r/StableDiffusion/comments/ygyq2j/a_simple_method_explained_in_the_comments_to/)