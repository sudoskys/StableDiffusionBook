# 工作台

>此页面等待精修
>Todo
>
>Upscaler 2 的预期用途 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3749
>

这节会介绍 参数 和 相关的 WebUi(SD)  网页应用资源。如果你会画画，那么效果会更加稳定可观。

## 考场速查

这里是打开页面就可以看到的工具箱

### 标签参考

* [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups)

* [MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)

* [手抄魔法本](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/)

* [绘画技术分类](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

* [地点分类](https://danbooru.donmai.us/wiki_pages/tag_group%3Alocations)

* [服装分类](https://danbooru.donmai.us/wiki_pages/tag_group%3Aattire)

* [美学风格分类](https://aesthetics.fandom.com/wiki/List_of_Aesthetics)

* [颜色列表](https://en.wikipedia.org/wiki/List_of_colors_by_shade)

### 工具

* [Danbooru 标签超市](https://tags.novelai.dev/) [AI绘画tag生成器](https://aitag.top/)

* [快速分享-在线剪切板](https://netcut.cn/)

* [prompthero](https://prompthero.com/)

* [Prompt Search ](https://www.ptsearch.info/home/)


### 作品参考

* [arthub](https://arthub.ai/)

* [NOVELAI.IO ](https://novelai.io/)

* [Pixai](https://pixai.art/)

* [LEXICA](https://lexica.art/)

* [Krea](https://www.krea.ai/)

* [largest library of AI-generated images](https://libraire.ai/)

* [Top-sd-artists](https://www.urania.ai/top-sd-artists)

* [artiststostudy](https://artiststostudy.pages.dev/)

* [Stable Diffusion Artist Style Studies](https://proximacentaurib.notion.site/e28a4f8d97724f14a784a538b8589e7d?v=ab624266c6a44413b42a6c57a41d828c)

* [Stable Diffusion Artist Study](https://docs.google.com/spreadsheets/d/1SRqJ7F_6yHVSOeCi3U82aA448TqEGrUlRrLLZ51abLg/htmlview#)

标签参考应该使用 [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups) 和 [MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference) ，因为它们都比较原生。

笔触/绘画工具/风格化/绘画技术标签组请看 [绘画技术分类](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

如果需要中文的标签参考/模板共享/信息，请看[调参魔法书](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml)和 **[手抄魔法本](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/)**

如果你习惯阅读日语的标签参考和技巧，可以在 [日语Wiki](https://seesaawiki.jp/nai_ch/d/%be%ec%bd%ea%a1%a6%c7%d8%b7%ca) 中进行检索。

如果你觉得查表很麻烦，可以打开 [Danbooru 标签超市](https://tags.novelai.dev/),[项目地址](https://github.com/wfjsw/danbooru-diffusion-prompt-builder) 或者[AI绘画tag生成器](https://aitag.top/)(后者不支持负面Tag)

如果你不想读文字，可以打开 [推荐的视频教程](https://space.bilibili.com/35723238/channel/collectiondetail?sid=779851)，但需要了解 **通过 AI 模仿画风，特定镜头，增加特效，微修微调，PS嫁接出图，通过3D特定姿势，重画，迭代** 等等操作的话，需要 **通读** 后面的内容。


>提前告知：WebUi 的设置页面需要按下 `Apply setting` 才能保存设置。


## 基本流程

![WorkFlow](https://user-images.githubusercontent.com/75739606/197821744-99b18fbc-1ae4-4e37-b7e7-9a47adaa6ae5.svg)
<!--
![WorkFlow](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/draw_workflow.svg)
-->
这幅图演示了循环迭代的流程
迭代方式，有循环迭代和线性迭代两种，线性迭代适用于多样性测试，而 **循环迭代** 是优化的更好选择。

来回改提示+固定种子并不是好选择。

目前研究基本方向是

- 提示词 + PS/Inpaint(微修/嫁接)

- 提示词 + 3D 参考

## 参数介绍

来自[^1]

* Prompt：对你想要生成的东西进行文字描述。
    
* Negative prompt：用文字描述你不希望在图像中出现的东西。
    
*  Sampling Steps：扩散模型的工作方式是从随机高斯噪声向符合提示的图像迈出小步。这样的步骤应该有多少个。更多的步骤意味着从噪声到图像的更小、更精确的步骤。增加这一点直接增加了生成图像所需的时间。回报递减，取决于采样器。
    
* Sampling method：使用哪种采样器。Euler a（ancestral 的简称）以较少的步数产生很大的多样性，但很难做小的调整。随着步数的增加，非ancestral采样器都会产生基本相同的图像，如果你不确定的话，可以使用LMS。
    
* ddim_eta：使用DDIM时的随机性数量。
    
* Batch count/n_iter：生成一组图像的频率。
    
* Batch size：同时生成多少个图像。增加这个值可以提高性能，但你也需要更多的VRAM。图像总数是这个值乘以批次数。
    
* CFG Scale（无分类指导规模）：图像与你的提示的匹配程度。增加这个值将导致图像更接近你的提示（根据模型），但它也在一定程度上降低了图像质量。可以用更多的采样步骤来抵消。
    
* Width：单个图像的宽度，像素。要增加这个值，你需要更多的VRAM。大尺度的图像一致性（模型是在512x512的基础上训练的）会随着分辨率的提高而变差。非常小的值（例如256像素）也会降低图像质量。
    
* Height：与宽度相同，但用于单个图像高度。
    
* Seed：RNG的起点。保持这个值不变，可以多次生成相同（或几乎相同）的图像。没有什么种子天生就比其他的好，但如果你只是稍微改变你的输入参数，以前产生好结果的种子很可能仍然会产生好结果。

## 如何书写提示

这是一个通用的指南，内容是基本通用的，可能有例外情况，请读对应的章节了解不同应用的特性。

- 长度

除了 WebUi ，由于GPT-3模型限制，prompt 并不是无限的，positive token 在 75-80 之间，75 字符后的内容会被截断。

- 特殊情况

大写字母并不重要，下划线（"_"）通常不被转换为空格。

提示 开头和结尾的额外空格会被直接丢弃。词与词之间的额外空格也会被丢弃。

- 自然语言

可以直接使用自然语言，WebUi(SD) 有自然语言处理能力(英文句子),也可以使用 颜文字 和 emoji ，也可以使用 Unicode字符（如日语字符）。

至少有一些Unicode字符是拉丁语字符的替代版本，它们被映射为普通的拉丁语字符。日语中使用的全幅拉丁字符（如ABC）被确认为可以转换。法语重音符（如é和è）和德语umlauts（如ä和ö）不会被映射到它们的常规对应物。

- 标点符号

用逗号、句号、甚至是空字符（"/0"）来分隔关键词，可以提高图像质量。目前还不清楚哪种类型的标点符号或哪种组合效果最好--当有疑问时，只要以一种使提示更容易被阅读的方式来做。

### 运动和姿势

如果没有很大要求的话，选择只与少数姿势相关的提示。

这里的姿势是指某一事物的物理配置：图像主体相对于摄像机的位置和旋转，人类/机器人关节的角度，果冻块被压缩的方式，等等。你试图指定的事物中的差异越小，模型就越容易学习。

因为运动就其定义而言涉及到主体姿势的巨大变化，与运动相关的提示经常导致身体的扭曲，如重复的四肢。另外，因为人类的四肢，特别是人类的手和脚有很多关节，他们可以采取许多不同的、复杂的姿势。这使得他们的可视化特别难学，对于人类和神经网络都是如此。

简而言之：人类站着/坐着的好形象很容易，人类跳着/跑着的好形象很难。

### 特异性

对于神经网络的训练来说，特征的质量很重要：输入和输出之间的联系越强，神经网络就越容易学习这种联系。

换句话说，如果一个关键词有非常具体的含义，那么学习它与图像之间的联系要比一个关键词有非常广泛的含义容易得多。

这样一来，即使是像 "Zettai Ryouiki" 这样很少使用的关键词也能产生非常好的结果，因为它只在非常具体的情况下使用。另一方面，"动漫 "即使是一个比较常见的词，也不会产生很好的结果，这可能是因为它被用于许多不同的情况，即使是没有字面意思的动漫。如果你想控制你的图片的内容，选择具体的关键词尤其重要。另外：你的措辞越不抽象越好。如果可能的话，避免留下解释空间的措辞，或需要 "理解 "不属于图像的东西。甚至像 "大 "或 "小 "这样的概念也是有问题的，因为它们与物体离相机近或远是无法区分的。理想情况下，使用有很大可能逐字出现在你想要的图像标题上的措辞。

### 强调语法

如何增加或减少提示的特定部分的比重。

有些人断言，把关键词放在圆括号里会增加其效果，而把关键词放在方括号里则会减少其效果；使用更多的括号据说会带来更强的变化。然而，其他人在他们自己的提示中经常不能再现这种效果。

事实证明，造成这种差异的原因是，不同的脚本处理括号的方式不同。例如 [这个webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) 明确地处理了括号，而 [这个分支](https://github.com/hlky/stable-diffusion-webui) 只会得到模型从训练数据中隐含 学到的括号的效果 （用不同的语法来代替）。出于这个原因：一定要检查你从别人那里复制的提示语的语法是否与你使用的脚本的语法一致。在定量分析中，除非明确处理，否则方括号并没有产生一致的效果。

无论使用何种具体的脚本，重复某个关键词似乎都会增加其效果。

### 尺寸

尺寸明显影响内容，如果图像高度增加，就更有可能生成 个体或单独的东西。如果图像宽度增加，更有可能生成多人或风景画。

## 项目

[WebUi：为Krita插件等添加自定义后端API](https://github.com/Interpause/auto-sd-paint-ext)



[^1]:[installgentoo wiki](https://wiki.installgentoo.com/wiki/Stable_Diffusion)