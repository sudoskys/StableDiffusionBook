 

## NAI Leak 模型

关于 NAI Leak 的事故报告。

使用 NAI Leak 模型 的 models 文件夹结构应该如下。

```
./models
├── Codeformer
├── ESRGAN
├── GFPGAN
├── hypernetworks
│   ├── aini.pt
│   ├── anime_2.pt
│   ├── anime_3.pt
│   ├── anime.pt
│   ├── furry_2.pt
│   ├── furry_3.pt
│   ├── furry_kemono.pt
│   ├── furry_protogen.pt
│   ├── furry.pt
│   ├── furry_scalie.pt
│   ├── furry_transformation.pt
│   └── pony.pt
├── LDSR
├── Stable-diffusion
│   ├── final-pruned.ckpt -> novelai 的 model.ckpt (pruned)
│   ├── final-pruned.vae.pt -> novelai 的 animevae.pt
│   ├── final-pruned.yaml -> model.ckpt 同文件夹的 config.yaml
│   └── 其他模型文件.ckpt
└── SwinIR
```

其中，hypernetworks 和 Stable-diffusion 是需要新建的文件夹。其他文件根据规则重命名。

### Part 1

|文件|对应 Leak 路径|说明|
|----|---------|--------|
|`final-pruned.ckpt`|`stableckpt/animefull-final-pruned/model.ckpt` (pruned)|模型主文件|
|`final-pruned.vae.pt`|`stableckpt/animevae.pt`|用于稳定风格|
|`final-pruned.yaml`|`stableckpt/animefull-final-pruned/config.yaml`|与记载额外的参数，内存消耗大，效果不明显|
|`stableckpt/vector_adjust/v2.pt`|风格化|感觉不如 `hypernet`|
|个人不需要下载的|`workspace`|前后端全套，40GB仅能启动|

注意，`final-pruned .yaml` 的名称应该对应 `final-pruned .ckpt`

`hypernetworks` 包含了 `stableckpt/modules/modules` 里的文件，是风格相关的数据集，可以作为特定人物的 `embedding model` 调用，和 model 使用可以生成特定风格。主要格式为 `*.pt`。需要在WebUi的设置标签页启用这个增强模型。

`workspace` 不是个人可以负载的，NAI 采用的是 GPU 集群云。

### Part 2

`prodmodels` 是 GPT 模型(语言处理)，但是实际用了 CLIP，所以不用我们管。

`random_stableckpt` 是一些模型，有的与 Part1 重复


![Part1](https://user-images.githubusercontent.com/75739606/197821809-7eed7776-9508-4c71-9b07-5f02e13290b2.jpg)
<!--
![Part1](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/models.jpg)
-->
??? info "附内容"
    - stableckpt/ - Stable Diffusion checkpoints
    - animefull-latest - The model NovelAI uses in production
    - workspace/ - Code used to train/run/finetune models
    - sd-private.tar.zst - Stuff to train Stable Diffusion
    - github/ - Code taken from GitHub. CREDENTIALS SCRUBBED
    - novelai/ - From NovelAI org
    - *.tar.zst Archived git repos, public AND PRIVATE
    - aboutus.gpg - Our public GPG key
    - sha256sum - SHA256 sums of every file
    - sha256sum.sig Detached signature for the sums, signed by our GPG key

关于风格模型，启动 cli 有提示加载就 OK, 同时可以去设置选模型那里选喜欢的 `hypernetwork`

### 全量和基线模型

`animesfw-latest` = NAI 基线模型

`animefull-final-pruned` = `full-latest` = NAI 全量模型(包含成人内容)

### 使用 latest (7G) 还是 pruned (4G) 模型

4GB 的模型由 7GB 的模型修剪而来，去除了最后一次的权重，留下了 EMA 权重。

个人用户只需要使用 pruned 4GB 模型。使用 latest 会过度占用 RAM 和 VRAM。

且 NAI 在线上也使用 EMA 权重，所以选择 latest 7GB 模型是没有意义的。

[结论由此贴讨论得到](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3882551)。

**关于 EMA**

EMA 移动平均值对生成图像没有任何帮助。

但确实可以在后续的训练中防止过拟合。

不确定在 dreambooth 的训练中是否起作用。

**详细介绍**

<!--
<iframe src="//player.bilibili.com/player.html?aid=688965561&cid=857942294&page=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->
拆包视频可以看 [BV1Gm4y1A7VM](https://www.bilibili.com/video/av688965561/?zw)

### 风格化[^5]

![furry](https://user-images.githubusercontent.com/474879/194965036-4c9f70ca-a32f-4f68-a9a1-17a827e8e61e.jpg)
>furry

![test-result](https://user-images.githubusercontent.com/115398132/194805364-95e523aa-4bec-4a88-9fe1-b3d39d2f2992.jpg)

```
masterpiece, best quality, masterpiece, 1girl, solo, outdoors, flowers, dancing
Negative prompt: nsfw, lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts,signature, watermark, username, blurry, artist name
Steps: 28, Sampler: Euler, CFG scale: 12, Seed: [SEE COLUMN], Size: 512x512, Model hash: 925997e9, Hypernet: [SEE ROW]
```

`aini` 有一种你可能不喜欢的强烈风格，我认为它具有最高的一致性和质量。

`anime_3` 是该系列中质量最高的，但它们都有些不一致. 我一般不会推荐他们。

可以看到 `furry` 的超网络在添加动物特征方面更加激进，因此这里更保守的变化可能与采样器、步骤和 CFG 有关。[^5]

[^5]:[所有超网络的 X/Y](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3836360)
