# 工作台

>此页面等待精修
>Todo
>
>Upscaler 2 的预期用途 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3749
>

这节会介绍 参数 和 相关的 WebUi(SD)  网页应用资源。如果你会画画，那么效果会更加稳定可观。

## 考场速查

### 标签参考

* [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups)

* [MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)

* [手抄魔法本](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/)

* [绘画技术分类](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

### 工具

* [Danbooru 标签超市](https://tags.novelai.dev/) [AI绘画tag生成器](https://aitag.top/)


### 成品

* [Pixai](https://pixai.art/)


标签参考应该使用 [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups) 和 [MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference) ，因为它们都比较原生。

笔触/绘画工具/风格化/绘画技术标签组请看 [绘画技术分类](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

如果需要中文的标签参考/模板共享/信息，请看[调参魔法书](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml)和 **[手抄魔法本](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/)**

如果你习惯阅读日语的标签参考和技巧，可以在 [日语Wiki](https://seesaawiki.jp/nai_ch/d/%be%ec%bd%ea%a1%a6%c7%d8%b7%ca) 中进行检索。

如果你觉得查表很麻烦，可以打开 [Danbooru 标签超市](https://tags.novelai.dev/),[项目地址](https://github.com/wfjsw/danbooru-diffusion-prompt-builder) 或者[AI绘画tag生成器](https://aitag.top/)(后者不支持负面Tag)

如果你不想读文字，可以打开 [推荐的视频教程](https://space.bilibili.com/35723238/channel/collectiondetail?sid=779851)，但需要了解 **通过 AI 模仿画风，特定镜头，增加特效，微修微调，PS嫁接出图，通过3D特定姿势，重画，迭代** 等等操作的话，需要**通读**下面的内容。


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



## 项目

[WebUi：为Krita插件等添加自定义后端API](https://github.com/Interpause/auto-sd-paint-ext)



[^1]:[installgentoo wiki](https://wiki.installgentoo.com/wiki/Stable_Diffusion)