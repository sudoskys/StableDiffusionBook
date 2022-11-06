# Practical Guide

This short practical guide will give you a quick overview of how to adjust the parameters appropriately to achieve the desired effect.

Currently, Ai does not handle details and multiple characters well, and the light and shadow structure will be messed up if handled one by one. If retouching, it is better to redraw. But it is practical to draw backgrounds and do previews.

**The core logic is to narrow the range of pre-selected data, adjust the balance between negative and positive prompts, and use part of the grammar to control the intensity of the prompt word effect and the process of effect.**


## Scene table

Viewed vertically

|characters|scenes|data-limited|events|
|----|------|-----|-----|
|expressions| | |expressions
|Hair|Wide and Narrow Selection|Paint Type|Abbreviations|
|eyes|lighting options|evaluation qualification|sfw/nsfw|
|clothing|background subjects|associative elements| Production Company |
|state|character event|  3D/2D  | Artwork and Brand   |
|Pose position ||||
|Lens position ||||


## Painting soldering!

With [AiPhotoShop - the online tool for infinite extents of canvas](https://www.painthua.com/) you can "weld" your work continuously, very conveniently. Just add the `--api` parameter to the start command and open the web page to use it.

[Github](https://github.com/BlinkDL/Hua)


## Curbing style pollution

Introduces how to generate images in WebUi with the subject as the focus, rather than the style of the drawing.[^12]

To get the desired result, we change `style` to `[style:10]` 

In Automatic 1111's WebUi prompt method, [x:10] tells stable diffusion until the tenth step before rendering the prompt.

By the time 10 steps are reached, a basic image has been formed, so styling pollutes the main component recklessly, instead acting like an IMG2IMG (which you can think of as a form of Img2img) over the existing image

You can experiment with this number depending on how many overall steps you've done. If you change the format to [x:0.2] (i.e. a number below 1) then it becomes a percentage, [x:0.6] tells SD to wait until 60% of the total number of steps before implementing x, and so on.


## The secret of diagram to diagram conversion

Whether it is 3D (3D models like DAZ) or line drawings, AI only recognises **colour**, not lines, and colour directly determines the result of a diagram to diagram conversion.


## Size selection

It should not be linked to picture quality; size affects the subject to some extent, as it potentially represents the category chosen (e.g. vertical figures, horizontal landscapes, small resolution emojis mostly).

Picture quality can be manipulated using the Super Score guide.


## Shot

[Recommended use of terms contained in Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

![shot](https://user-images.githubusercontent.com/75739606/198682933-25c1b2df-c573-44ea-aba7-2df4609299e5.png)
<!--
![shot](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/shot.png)
-->
>citing an image from the Japanese Wiki, author unknown


`extreme closeup` ECU

`close up` CU

`medium close up` MCU

`medium shot` MS

`cowboy shot` CS

`medium full shot` MFS

`full shot`  FS


## Reproduction

If you want to reproduce it on the generation, you need the same `seed`, prompts, and other parameters (even size). However, if the other side uses stylisation, then you can hardly reproduce it.

--Self-deceptive reproduction: image to image, noise reduction to below 0.3...--


## Frivolous details

- Emoticons

emoji(💰💶💷💴💵,🎊🪅🪄🎀,👩 🚀) Emoticons are also available and **very accurate**, I have tested them many times and Ai can recognise them.

The emoji performs well in terms of semantic accuracy because it is only one character.

[emoji-list](https://unicode.org/emoji/charts/emoji-list.html)


- Anime style

`anime colouring` will work well, like an anime screenshot. (If you encounter black and white, you need to specify the colour of the character parts)

<iframe width="560" height="315" src="https://www.youtube.com/embed/hGATDT-Y37g" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[some_art_styles_that_i_tried](https://www.reddit.com/r/StableDiffusion/comments/yllj3r/some_art_styles_that_i_tried/)

### Optimised artwork

- Take a look at the target

Think about what you want your work to be labelled as on the internet.

A sticker would certainly not say `masterpiece, best quility,` would it?

Another thing is that if the request is for pixel work, you should remove any negative hints of conflict (if any).

- Semantic offset prevention

To prevent semantic shifts, prioritise emoji and then use less complex syntax like `with` unnecessarily.

- Rainbow messy pictures

If you are outputting chaotic rainbow coloured images, it is possible that the generation resolution has been set too low.

- Wavy noisy picture

Dense waves or patterns may be because the weights are set too high.

- Keyed image?

The rest of the background of a buttoned image will be processed as black.

If you need a background, the results are generally not very good and it is recommended to fill in the colours before manipulating them.

- Rough noise

Not enough detail, characters look like rice cakes. Needs proper enhancement step.


## Associated

The core logic of cross-domain terminology is to **narrow the scope of the specified data** and start with **platforms, domains, events** outside of the screen content to enhance the effect.

Yes! You can use [Film Glossary](https://www.owlnet.rice.edu/~engl377/film.html) [FILM GLOSSARY Photography Terminology](http://userhome.brooklyn.cuny.edu/anthro) in the prompt /jbeatty/COURSES/glossary.htm), [Cinematic techniques](https://en.wikipedia.org/wiki/Category:Cinematic_techniques), and painting terms (types) to control the basics.

Crossover! You can even use various thrill-sport terms to produce some special effects .... such as airborne

for example, depth of field, aperture, composition, camera position, elements of movement, [a Chinese introduction to the glossary of artistic photography](https://gallerix.asia/pedia/photography-glossary/)

**BUT** this effect may have the added effect of introducing data into the picture that you don't want to see in the style (e.g. live action** rather than **but secondary). Get the measurements right. Add Step and style cues as appropriate to improve.

You can also use **platform names** to limit the scope of the dataset, for example, terms like pixiv.

**extended reading**

[Recommended use of terms contained in Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

[Danbooru tag group](https://danbooru.donmai.us/wiki_pages/tag_groups)

[Useful film terms ](https://en.wikipedia.org/wiki/Category:Cinematic_techniques)

[Types of shots](https://www.bhphotovideo.com/explora/video/tips-and-solutions/filmmaking-101-camera-shot-types) 

[TV terms](https://en.wikipedia.org/wiki/Category:Television_terminology) 

[Types of photography ](https://en.wikipedia.org/wiki/Category:Photography_by_genre)

[Photographic terms](https://zh.wikipedia.org/zh-cn/%E9%AB%98%E9%80%9F%E6%91%84%E5%BD%B1) 

[Extreme sports](https://en.wikipedia.org/wiki/Extreme_sport)

[Sports_by_type](https://en.wikipedia.org/wiki/Category:Sports_by_type)

[Art of composition](https://en.wikipedia.org/wiki/Composition_(visual_arts))


## Iterative sketches [^8]

Here's a discussion on how to optimise **hand sketches** by Ai drawing, *note not anime girls*.

In the first iteration you don't need too many Steps, the CFG can be very low (for better diversification results) and Denoising should be around 0.3-0.4 if you don't want to lose the sketch completely.

In the final iteration, increase the Steps and Denoising strength (but no more than 0.8, otherwise the image will be corrupted, especially at greater than 512*512) see [here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues /2213#issuecomment-1274137775), while increasing the CFG and size as needed.

You can always fix the hints (add or remove details that appear) and try a different sampler.

Also, shouldn't you use a fixed and unchanging seed for the initial generation?

If you provide a seed (instead of a random -1) your image will soon become oversaturated, oversharpened, overpixelated ..... Of course you can use a fixed seed if you want to fine tune it.


## Facial expressions Control

We can control the expressions in the picture using the Face Text!

:-) smile :-( unhappy ;-) wink :-D happy :-P tongue out :-C very sad :-O surprised open mouth :-/ suspicious

Only western face characters are supported, for details see [Danbooru Face Characters section](https://danbooru.donmai.us/wiki_pages/tag_group%3Aface_tags) or [Wikipedia](https://zh.wikipedia.org/wiki/%) E8%A1%A8%E6%83%85%E7%AC%A6%E8%99%9F%E5%88%97%E8%A1%A8?oldformat=true)


## Multi-figure/single-figure

Answer: sketching + IMG2IMG

Wide paintings of single figures are best sketched and coloured to determine the subject of the picture.

Multiple figures determine the number of figures, best to use sketches/coloured 3d arrangement + figure generation.

More than three figures makes it difficult to control the effect

Maybe There are no models for images with more than 6 people...


## Make a hand fix

Feed the image into inpaint, use roughly the same prompting, put the cue about the `hand` in front, set the noise reduction depending on how much you want it to change (if you just want the hand to be more complete, tune it to 0.25 or less), then leave the steps the same as CFG and txt2img gen.

Or just mask the hand, fix it at full resolution, greatly reduce the padding (it uses the surrounding pixels to create the context, but is just re-creating the hand) and only hint at hand issues (detailed hand depiction etc)

The higher the CFG, the more it matches the prompts, and the higher the noise reduction the more it deviates from the original image.


## Identical figures & differentiation

Advanced Img2Img related content is required, the best way to do this is to prepare a 3D master model with colours and then this will ensure basic consistency.

You can also use a lot of prompts to limit the content of the character, come up with many sheets and pick the pieces that work.

If it's an expression or a background, you can use the repainting technique from the advanced tutorial.

If you want some examples of differencing, [5CH Japanese Wiki](https://seesaawiki.jp/nai_ch/d/%c7%ed%a4%ae%a5%b3%a5%e9%a5%c6%a5%af) provides an example.




## Designing with Ai

- Backgrounds

Once you get a slightly more saturated photo, you can process it in AI and use it as a background.

Use `anime coloring` to enhance the anime style.

- Icons

[Look at this](https://www.bilibili.com/video/BV1384y1B7MX)

- Emotion packs

The emojis work really well. The `sticker` with the western face script is very well done!

The documentation writer has tried the following prompt and it produces high quality emojis.

Don't ask for quality prompt words for stickers.

**positive vocabulary**

```
1girl,sticker,Chibi,Q,
white background,
```
`masterpiece,best quality` may or may not be added, if quality is not enough

`lowres` can be added or not, add if not vintage enough

`Chibi` controls the character to be Q (big head, small body), add if not Q enough

`sticker` is to specify the style as a social media sticker, required

**negative vocabulary**

```
bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, jpeg artifacts, signature, watermark, username, blurry, bad feet
```

low quality can be added or not, if you want to be more flat, please add `3D` to negative hints.

Size required square.

- Character design

<iframe src="//player.bilibili.com/player.html?aid=559362671&bvid=BV14e4y1U7r9&cid=869144379&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
>BV14e4y1U7r9


## Prompt

SEE [HERE](https://danbooru.donmai.us/wiki_pages/howto%3Atag_checklist) From[Origin wiki](https://danbooru.donmai.us/wiki_pages/tag_groups)


### NAI in use out parameters

- Use the full model

- CLIP layer = 2

- Use ema weight loading, set `use_ema` to true in the yaml configuration

- reset ` sigma noise/strength` to the default value of 1

- set `eta noise seed delta` to 31337 (so that ` sigma noise/strength` does not need to use 0.69 / 0.67)

- If prompt has weights, convert the weights (WebUi to 1.1, NAI to 1.05)

- Use the `-no-half` parameter to start the program (secondary)




**NAI default model settings**


```
steps": 28, "sampler": "[sampler]", "seed": [seed], "strength": 0.69, "noise": 0.667, "scale": 11.0,

Strength ， noise 是 eta 和 sigma

scale 就是 CFG scale
```

**NAI default `SFW` negative prompt**

```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry
```

**other**

Add `masterpiece, best quality` in front of all prompt words


Clip skip 0, everything else is fine (afaik don't use supernet, v2, yaml, VAE)




### Conversions - NAI and WebUi(SD) have different enhanced syntax

**Prompts parameter bracket conversion**

Convert enhanced parameters between NAI and Webui, related tgbot services [M2NM2NBot](https://t.me/M2NM2NBot)

Related [WebJS](https://github.com/naisd5ch/novel-ai-5ch-wiki-js)

Weight enhancement markers: NAI is `{}` and WebUi(SD) is `()`


### Prompt Magic Book

The Prompt Magic Book provides a library of ready-to-use templates (analogous to the Composition Book) with a convenient debugging log for Tag.

[Prompt Magic Book Volume 1](https://docs.qq.com/doc/DWHl3am5Zb05QbGVs)

[Prompt The Book of Magic Volume 1.5](https://docs.qq.com/doc/DWGh4QnZBVlJYRkly)

[Prompt 魔法書製作委員會](https://space.bilibili.com/1981251194)



### Good parameters (style tends to be illustrative) [^4]

```
{an extremely delicate and beautiful}
```

[Full classification of drawing mediums](https://danbooru.donmai.us/wiki_pages?commit=Search&search%5Btitle_normalize%5D=%2A_%28medium%29)

[List of anime characters/artists/stylizations/Pt files](https://rentry.org/anime_and_titties)

[Stylization: Doll Class](https://www.yuque.com/longyuye/lmgcwy)

[Stylization, Japanese Wiki](https://seesaawiki.jp/nai_ch/d/%b2%e8%c9%f7%a1%a6%b9%bd%bf%de)

[Stylization: 32 types](https://www.bilibili.com/video/BV1TP411N71t/)

[Artist List/SD1.4](https://rentry.org/artists_sd-v1-4)

[Artist List/SD1.4/1,833 artists](https://www.urania.ai/top-sd-artists)

[Artist Museum](https://gallerix.asia/storeroom/)

[Stable Diffusion Artist Style Studies](https://proximacentaurib.notion.site/e28a4f8d97724f14a784a538b8589e7d?v=42948fd8f45c4d47a0edfc4b78937474)

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


## Market Adoption Survey

Here are the applications for stable diffusion (non-NAI models).

https://www.reddit.com/r/StableDiffusion/comments/yh8j0a/ai_art_is_popular_and_makes_money_confessions_of/

- 3D

On blender, Ai has [rendering plug-in](https://blendermarket.com/products/ai-render/?ref=110)

- Design

[Microsoft 365 Tool Suite](https://www.xda-developers.com/microsoft-designer-image-creator-ai-dall-e-2/)

[Portrait generation for the Age of Empires 3 Definitive edition of the game module](https://github.com/matrix4767)

[Product and architecture design/sketch](https://github.com/horribleCodes)

- Album artwork

[Image generation of singer with stable diffusion into video](https://github.com/Chilluminati91)

- Supporting images

[Making weird diagrams as a memory tool](https://github.com/ClashSAN)

[A real-life use case for stable diffusion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3219)

[A combination of applications for stable diffusion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3572)

- Self-Media

There's been a lot of it.

- Redistribution for sale

Unconscionable companies re-open source projects

(The Chinese version of the document here is meant to condemn people who disregard open source agreements to package and sell open source projects)

- Illustrations/backgrounds

Novel illustrations, AI painting backgrounds (the original model is said to work well too)

- NFT

?


[^4]:[Paper朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)

[^12]:[a_simple_method_explained_in_the_comments_to](https://www.reddit.com/r/StableDiffusion/comments/ygyq2j/a_simple_method_explained_in_the_comments_to/)