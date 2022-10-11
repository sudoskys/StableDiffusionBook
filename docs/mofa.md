# 调参魔法

这节会介绍 参数 和 相关的SD网页应用资源。

## 魔法入门

请先在前面了解一下SD网页应用的参数。

### 参数权重

调参魔法的一个基本技能是设置权重。

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


!!! info
    **NV官方**

    在NV官方前端我们使用 `{}` 来指定权重。

### 消极参数

SD网页应用会在生成时**拒绝消极参数有关的内容**。

比如使用以下参数削除水印和文字内容

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

### 重现参数

对于没有压缩的原图，我们可以将文件拖入 `PNG Info` 选项卡，进行参数查看。

### 逆向参数

这里有一些 **Image 逆向参数服务**，可以从图片中提取相关参数，不一定准确。

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)

### 已知参数组合

你可以访问以下传送门获取一些优秀的参数实例！（当然，中文社区的测试群有**大量素材**，一分钟20次）

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)

### **种子调试**

在相同 Step ，cfg ，Seed,参数（prompts） 的情况下，生产的图片基本相同！

## 魔法进阶

调参基本原理模糊的说是：限定好的数据范围内相似样本越多，越稳定。


### 注意 `尺寸`

比如出图尺寸宽了人可能会多

!!! tip
    要匹配好姿势，镜头和人物才不畸形，有时候需要限定量词，多人物时要处理空间关系和 prompt 遮挡优先级。人数->人物样貌->环境样式->人物状态
    



## 工具速查


[参数法术全典](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml#)

## 方位调参

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





### 关于参数


**[Tag在线协作](https://docs.google.com/spreadsheets/d/1zij5OzCZIaQuhAbiSayhFznjgJ3rwbaNwnUnaUMxyTQ/edit?usp=drivesdk)**

所有 Tag 可以上[danbooru](https://gelbooru.com/index.php?page=tags&s=list)


NSFWTag 可以参考[Github Here](https://github.com/scooderic/exhentai-tags-chinese-translation)

**Image 逆向为参数**

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)

**Prompts 参数括号转换**

在 NV 和 Webui 之间转换加强参数

[M2NM2NBot](https://t.me/M2NM2NBot)

### 关于显卡

cuda 是否可用、以及 torch 对应的 cuda 的版本
打开命令窗，输入 python 进入，分行输入
```
import torch
print(torch.__version__)
print(torch.cuda.is_available())
```

**查看torch对应的cuda版本**
```
torch.version.cuda
```
输入 ctrl + z 退出


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



