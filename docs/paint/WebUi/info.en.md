# Prompt-Info-Script


## Custom Scripts

The content of this topic may not be instantly updated here, [source address](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts#prompt-interpolation)



### Auto-completion

Here's a project that enables SD to do a NAI-like patching effect.

https://github.com/DominikDoom/a1111-sd-webui-tagcomplete


### Advanced prompt matrix

https://github.com/GRMrGecko/stable-diffusion-webui-automatic/blob/advanced_matrix/scripts/advanced_prompt_matrix.py


```
<cyber|cyborg|> cat <photo|image|artistic photo|oil painting> in a <car|boat|cyber city>
```


### XYZ Drawing Script

Generate an .html file to interactively navigate through the image set. Use the scroll wheel or arrow keys to move through the Z dimensions.

https://github.com/xrpgame/xyz_plot_script

### Embedding to PNG


https://github.com/dfaker/embedding-to-png-script

### Random Steps and CFG

https://github.com/lilly1987/AI-WEBUI-scripts-Random



## Market Adoption Survey

Here are the applications for stable diffusion (non-NAI models).

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

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)
