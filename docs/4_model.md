# 架炉生火

这节介绍 Debug 和一些 关于 WebUI 网页应用模型参数 的优化方案，让它更好用。本节演示所用模型为 NAI 展开教程。**部分源教程来自：[^2]**

推荐经常从远端代码库拉取代码 `git pull` 更新 WebUI 网页应用。

!!! info "版权"
    为避免涉及版权纠纷，本仓库不提供 NAI 的模型链接。
    可以尝试关注中文社区 t.me@StableDiffusion_CN_WIKI 或 打开整合包自行查看 `model` 目录。

SDWebUi是一个框架，除了 NAI 模型外还有许多[其他模型](https://rentry.org/sdmodels)


## 关于显卡

!!! tip
    注意显卡温度，有报道称显卡太热炸了。

先判断 CUDA 是否可用。

打开终端，输入 python 进入，分行输入

```python
import torch
print(torch.__version__)
print(torch.cuda.is_available())
```


**查看 torch 对应的 CUDA 版本**

```python
import torch
torch.version.cuda
```

输入 ctrl + z 退出


### 多 GPU 支持

最简单的模式就是实现一个多数据并行处理的方法，通过 `--device-id` 参数启动多个实例。每个 GPU 加载一个模型，然后给她们分配工作。

考虑到这个项目所提供的功能众多，我（作者）认为可能需要一段时间才能在并行的情况下使用所有的功能。

[作者的回应](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/156#issuecomment-1241218733)

>Using memory from between two GPUs is not simple. I only have one so I can't research/develop this.


### 16xx系显卡使用半精度生成图片[^3]

方案来自[这个讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/28#issuecomment-1241448049)

1. 激活 WebUI 使用的 venv,要在正确的虚拟环境里运行

2. 卸载掉现在所用的 torch 和 torchvision:

```bash
pip uninstall torch torchvision
```

3. 重新安装 `CUDA 11.6` 编译的 `torch` 和 `torchvision`。

```bash
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu116
```

4. 下载 `Cudnn_8.5` 备用。

使用下载工具下载，直接下载会跳转到 Nvidia 的开发者注册界面

Windows: <https://developer.nvidia.com/compute/cudnn/secure/8.5.0/local_installers/11.7/cudnn-windows-x86_64-8.5.0.96_cuda11-archive.zip>

其他版本：<https://developer.nvidia.com/rdp/cudnn-archive>

5. 将 Cudnn 8.5 压缩包里的 bin 和 lib 文件夹里的所有文件复制到 `venv\Lib\site-packages\torch\lib` 里，覆盖所有文件。

6. 然后 16xx 系显卡也可以愉快地使用半精度生成图片了！大幅降低显存占用，6G 加载 Full 模型可以生成 1024x640 的图片。

但是，依然不能使用 `DDIM Sampling` ，但可以使用 `Euler a`

## NAI Leak 模型

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

启动 cli 有提示加载就 OK, 同时可以去设置选模型那里选喜欢的 `hypernetwork`

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


## 基础

### 横条参数说明

`step` 迭代多少次, 取值和 `sampling method` 有关, `DDIM` 采样方法收敛较快, 具体差别见调参魔法 `Sampler vs. Steps Comparison (low to mid step counts)` 。

`batch count/batch size` 决定生成的图片数量, 显存够就加 batch size, 不够就 batch count, 得到的图片数量是两者之积 (小显存还是只动 count 就好)

`sample method`  采样方法。DDIM, Eula 也挺好用。 (带 a 的是 ancestral 的意思, step 增长出图不稳定)

`cfg scale` 符合 prompt 的程度, 值越高越会字面看待 prompt, 低则给模型较大的发挥空间, 但是实际模型表现上来看 CFG scale 低 (6-8) 饱和度低, 偏线稿, 偏杂乱, 高 (18-22) 则饱和度偏高, 偏 CG 风格.

>过高的 CFG 会引起颜色失真，CFG 应该在 5-15 之间

`denoise strength` img2img 专属参数, 从 0 到 1 取值, 值越高 AI 对原图的参考程度就越低 (同时增加迭代次数), 个人喜欢低 CFG 高 denoise 重绘图, 高 CFG 低 denoise 改细节.

[一个小指南：RedditAbout](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)


### 生成图片发生BUG的自救


#### 生成黑/绿图

[生成黑/绿图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs)

如果是 GTX 16xx 系列，启动参数需要加 `--precision full --no-half`, 因此如果显存不足还要加 `--medvram`。

如果是其他显卡而且加载了 VAE 时出现黑图，加入 `--no-half-vae` 参数[^2]。


#### RuntimeError Sizes of tensors must match

(img2img) 如果出现 `RuntimeError: Sizes of tensors must match`，请调整输入图像的分辨率


#### 彩虹混乱图

如果 AI 输出了混乱的彩虹色图片，则生成分辨率被设置得太低


#### 高分辨率出鬼图 / 低显存生成大分辨率图片

简单说就是使用低分辨率重新生成或者超分。见钩吻

你可以在下个章节看到具体操作流程。


#### RuntimeError: Unable to find a valid cuDNN algorithm to run convolution

前面那节有相关的参数建议。

生成报错解释：显存不足

先检查 CUDA 是否可用，打开命令窗，输入 python 并分行输入

```python
import torch
print(torch.cuda.is_available())
```

#### CUDA out of memory

原因：显存不足。`--lowvram` 和 `--medvram` 启动参数都可以改善此问题。


### ckpt 文件安全问题[^4]

ckpt 文件被加载时基本上可以执行任何内容，盲目加载有安全风险。请检查来源是否可靠再加载。
如果杀毒软件拦截，有可能创建者向文件中注入了恶意的 Python 代码。

可以通过此脚本检查风险：<https://rentry.org/safeunpickle2>


-----


## 进阶

### 关于模型？[^6]

![Roaming_info.png](https://user-images.githubusercontent.com/75739606/198679721-2a7b38b8-41f3-405c-9ea3-40f1b5e8cc7e.png)
<!--
![Roaming_info.png](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/infofrom_bili_uid_87077691.png)
-->
>来自 allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/


### 使用 WebUI 复现 NAI 官网

[相关讨论，应该读一读！](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017)

提示:由于 torch 及其相关框架的性质，尝试完全复原在不同机器上生成的图片是不明智的。所以不要纠结一些细节不能复现。


#### 需要做的事情

* 加载 VAE 和模型附带的 `config.yaml` (可选，有人说此操作空耗显存)

* `Stop At last layers of CLIP model` 设为 `2`

* `Eta noise seed delta` 设置为 `31337`


#### **不需要**做的事情

* hypernetwork。官网默认并不使用 hypernetwork


设置 `Stop At last layers of CLIP model` 是为了匹配 NAI 的一个[优化](https://blog.novelai.net/novelai-improvements-on-stable-diffusion-e10d38db82ac)。


### 半精度还是单精度？

如果能，尽量使用半精度，可以节省运算时间/RAM/VRAM，同时图片质量并不会和单精度差多少。~真要说差别可能和你电脑被宇宙射线打了差不多~。


### X/Y 图

创建具有不同参数的图像网格。使用X类型和Y类型字段选择应由行和列共享的参数，并将这些参数以逗号分隔输入X值/Y值字段。支持整数、浮点数和范围。

`Simple ranges` 简单范围

```
1-5 = 1, 2, 3, 4, 5
```

`Ranges with increment in bracket` 括号范围

```
1-5 (+2) = 1, 3, 5
10-5 (-3) = 10, 7
1-3 (+0.5) = 1, 1.5, 2, 2.5, 3
```

`Ranges with the count in square brackets` 方括号范围

```
1-10 [5] = 1, 3, 5, 7, 10
0.0-1.0 [6] = 0.0, 0.2, 0.4, 0.6, 0.8, 1.0
```

设置截图

![引用官方 Wiki 的设置图](https://raw.githubusercontent.com/wiki/AUTOMATIC1111/stable-diffusion-webui/images/xy_grid-medusa-ui.png)
>引用官方 Wiki 的设置图

### **Variations种子变化**

`Variation strength slider` 和 `Variation seed field` 允许您指定现有图片应更改多少以使其看起来不同。
在最大强度下，您将获得带有变异种子的图片，至少 - 带有原始种子的图片（使用先前采样器时除外）。

### **提示词模板**

“Save prompt as style” 按钮将当前的提示写入 `styles.csv`，该文件包含样式集合

提示右侧的下拉框将允许您从以前保存的样式中选择任何样式，并自动将其**附加**到输入中

要删除样式，请从 `styles.csv` 中手动将其删除并重新启动程序。


### xformers

xformers 分辨率越高加速效果越好。使用 xformers 会引入一些随机性，稍微影响生成的图像。

要启用，如果你是 Pascal、Turing 或者 Ampere 架构的卡（包括 GTX 1000，RTX 2000、3000 系列），添加 `--xformers` 参数到 `webui-user.bat` 中的 `COMMANDLINE_ARGS`。

!!! tip
    有人说在 700 和 900 系列卡上使用 xformers 的性能明显较差，请注意这一点。
    本人实测，2050 在启用 xformers 之后，速度慢了 50%


#### 在 Windows 上编译 Xformers

!!! info

    你可以在右边的链接下载预构建的 Xformers！https://rentry.org/25i6yn ，记得先查看 [GPU 架构](https://developer.nvidia.com/cuda-gpus)

确保 Python 版本为 3.10 或更高版本(使用 `Python --version`)，然后安装

安装 [VS Build Tools 2022](https://visualstudio.microsoft.com/zh-hans/downloads/?q=build+tools)，运行安装时只需要选择 `Desktop development with C++`

安装 [CUDA 11.3](https://developer.nvidia.com/cuda-11.3.0-download-archive)，（后期版本未测试），选择`custom`，VS集成可能不需要

* 确认 nvcc 可用

`nvcc --version`

* 克隆`xFormers` 存储库，在环境中激活它

```bash
git clone https://github.com/facebookresearch/xformers.git
cd xformers
git submodule update --init --recursive
```

* 创建虚拟环境且激活环境

```bash
python -m venv venv

#CMD
venv\Scripts\activate.bat
#Bash
source ./venv/bin/activate
#WindowsBash
source ./venv/Scripts/activate
```

* 为避免获取 CPU 版本时出现问题，请单独安装 pyTorch：

```
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu113
```

* 然后安装其余的依赖项

```bash
pip install -r requirements.txt
pip install wheel
pip install ninja
```

* 由于 CUDA 11.3 很旧，需要**强制启用**它以在 MS Build Tools 2022 上构建。

在 CMD 设置 `set NVCC_FLAGS=-allow-unsupported-compiler"`

或在 Bash 设置`export NVCC_FLAGS=-allow-unsupported-compiler`

* 查看你自己的 GPU 架构

[GPU 架构](https://developer.nvidia.com/cuda-GPUs)

比如说，如果你的 GPU 是 GTX 1070，基于该表，架构是 6.1
*CMD*  `set TORCH_CUDA_ARCH_LIST=6.1`

*BASH*  `export TORCH_CUDA_ARCH_LIST=6.1`

* 构建 xFormers，请注意构建需要很长时间（可能要 10-20 分钟），它最开始时可能会出现一些错误，但应该仍能正确编译。

* 安装在环境中(Conda)

```bash
python setup.py build
python setup.py bdist_wheel
```

找到 dist 文件夹并将文件 `*.whl` 复制到 `stable-diffusion-webui`
在 `stable-diffusion-webui` 目录中安装 `.whl`。

如果构建的 whl 名称不同，请在下面的安装命令中更改文件名

```bash
#CMD
./venv/scripts/activate
#Bash
source ./venv/bin/activate
#WindowsBash
source ./venv/Scripts/activate
pip install xformers-0.0.14.dev0-cp310-cp310-win_amd64.whl
```

30 系显卡正常启动加 `--xformers` 参数, 其他显卡加 `--force-enable-xformers` 参数

>COMMANDLINE_ARGS=


#### Windows 编译错误自查

>错误:`Filename too long` 和 `fatal error C1083: Cannot open compiler generated file: '': Invalid argument`

说明你的路径太长了。

>RuntimeError: CUDA error: no kernel image is available for execution on the device

现在更多 GPU 架构是自动支持的，尝试重新安装并使用 --xformers 参数。

如果你移动了 Xformers，那么应该删除里面的 venv 目录

[Windows](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases) (30 系之外要自己编译)

自己编译指路 [wiki/Xformers](https://rentry.org/sdg_faq#xformers-increase-your-its-more-cards-supported)， 还有[这篇文章](https://www.reddit.com/r/StableDiffusion/comments/xz26lq/automatic1111_xformers_cross_attention_with_on/)


### 使用CPU进行绘画

根据此 [pr](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2597)

可以通过 `--use-cpu all` 尽可能的使用 CPU 进行生成，虽然慢 100 倍。



### CLIP Interrogate

CLIP 可以从图像中提取令牌。

默认情况下，只有一个列表 - 艺术家列表（来自 artists.csv）。

不过你可以通过执行以下操作添加更多列表：
* interrogate 在与 WebUI 相同的位置创建目录
* 将文本文件放入其中，每行都有相关描述


你可以在[这里](https://github.com/pharmapsychotic/clip-interrogator/tree/main/data)查看使用哪个文本文件的例子。实际上，你可以直接用这个例子中的文件 —— 除了 `artists.txt` ，你已经有一份艺术家列表在 `artists.csv` 中了不是吗（或者用这个也行，随你）。每个文件都会使最后的描述增加一行字。如果你将 `.top3.` 放到文件名中，比如 `flavors.top3.txt` ，文件中相关度最高的三行将会被添加到提示词中（其他数量也行）。

<!--
这里的最后一句 “文件中相关度最高的三行”，关于 “相关度” 可能让人有点迷惑，后来的人可以看看有没有什么更贴切的翻译
For example of what text files to use, see https://github.com/pharmapsychotic/clip-interrogator/tree/main/data. 
In fact, you can just take files from there and use them - just skip artists.txt because you already have a list of artists in artists.csv (or use that too, who's going to stop you). 
Each file adds one line of text to the final description. 
If you add ".top3." to filename, for example, flavors.top3.txt, the three most relevant lines from this file will be added to the prompt (other numbers also work).
-->

### **Face restoration三次元人脸修复**

适用于三次元。

[https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration)

### 自定义.css

创建一个名为 `user.css` 的文件并放在 `webui.py` 旁，将自定义 CSS 代码放入 `user.css` 中。

下面的例子将会使得画廊更长：

<!--
此处将 taller 译作 更长。原文使用 taller 是因为在网页的情况下，元素在纵向上的长度是用 height 来描述的。
但是在中文语境下，译作 更高 会略显奇怪，故译作 更长。
如果有更好的翻译可以直接顶上
-->

```
#txt2img_gallery, #img2img_gallery{
    min-height: 768px;
}
```

### notification.mp3 提示音

放在 WebUI 的根目录的名为 `notification.mp3` 的音频文件将会在处理完成后播放。

### 开发自定义脚本

你可以在 `modules/scripts.py` 中找到 Script 类。

如果要创建你自己的自定义脚本，请创建一个实现类的 Python 脚本，并将其放到 scripts 文件夹中，使用以下示例或文件夹中已有的其他脚本作为指导。

Script 类有四个主要方法，这里通过一个简单的[示例脚本](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Developing-custom-scripts)进行更详细的描述，这个脚本可以旋转和/或翻转生成的图像。

### 修剪模型[^7]

将要修剪的 `.ckpt` 文件放在 `/stable-diffusion-webui` 文件夹，把 [脚本](https://raw.githubusercontent.com/harubaru/waifu-diffusion/main/scripts/prune.py) 另存本地，删除第 6 行和 第 8 行。然后在 prune.py 中的最后一行编辑 ckpt 的名称。

比如，`prune_it('wd-v1-2-full-emma.ckpt')`

然后运行这个脚本，修剪过程可能需要几分钟。

## 运行

### 4GB 显卡支持

针对具有低 VRAM 的 GPU 的优化。这应该可以在具有 4GB 内存的视频卡上生成 512x512 图像。

`--lowvram` 是 basujindal 对优化思想的重新实现。模型被分成模块，GPU 内存中只保存一个模块；当另一个模块需要运行时，前一个模块将从 GPU 内存中删除。这种优化的性质使处理速度变慢——与我的 RTX 3090 上的正常操作相比，速度慢了大约 10 倍。

`--medvram` 是另一个优化，通过不在同一批次中处理条件和无条件去噪，可以显着减少 VRAM 的使用。这种优化的实现不需要对原始的稳定扩散代码进行任何修改。

!!! info
    经过 10/10 的优化，RTX2050 的 4GB 显卡也可以使用 `--medvram` 。

当然也可以减半精度，或者生成一张 64x64 清理 VRAM

### 不间断生产

在 WebUI 的生成键右击即可出现**不间断生成**的选项。

### 图片信息 Png info

生成的图片自带令牌信息，拖放到查看页面即可查看 。




### NAI 4chan简化版本

4chan 版本魔改官后程序，会动态分配，显存不够内存来凑。

[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

[^3]:[16xx系显卡可以使用半精度生成图片的方式](https://t.me/StableDiffusion_CN/50749)

[^4]:[It's not a virus it's a checkpoint file](https://huggingface.co/Deltaadams/Hentai-Diffusion/discussions/12)

[^5]:[所有超网络的 X/Y](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3836360)

[^6]:[Roaming_info_for_latent_diffusion](http://allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/)

[^7]:[pruning-a-ckpt](https://rentry.co/voldy#-pruning-a-ckpt-)
