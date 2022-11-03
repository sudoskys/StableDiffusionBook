 
# 入门


>此页面等待精修

>Todo
>修复混乱的逻辑
>
>Upscaler 2 的预期用途 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3749
>


这节会介绍 参数 和 相关的 WebUi(SD)  网页应用资源。如果你会画画，那么效果会更加稳定可观。

标签参考应该使用 [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups) 和 [MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference) ，因为它们都比较原生。

笔触/绘画工具/风格化/绘画技术标签组请看 [Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

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


## 项目

[WebUi：为Krita插件等添加自定义后端API](https://github.com/Interpause/auto-sd-paint-ext)

