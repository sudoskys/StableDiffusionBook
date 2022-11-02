# 基础

## 横条参数说明

`step` 迭代多少次, 取值和 `sampling method` 有关, `DDIM` 采样方法收敛较快, 具体差别见调参魔法 `Sampler vs. Steps Comparison (low to mid step counts)` 。

`batch count/batch size` 决定生成的图片数量, 显存够就加 batch size, 不够就 batch count, 得到的图片数量是两者之积 (小显存还是只动 count 就好)

`sample method`  采样方法。DDIM, Eula 也挺好用。 (带 a 的是 ancestral 的意思, step 增长出图不稳定)

`cfg scale` 符合 prompt 的程度, 值越高越会字面看待 prompt, 低则给模型较大的发挥空间, 但是实际模型表现上来看 CFG scale 低 (6-8) 饱和度低, 偏线稿, 偏杂乱, 高 (18-22) 则饱和度偏高, 偏 CG 风格.

>过高的 CFG 会引起颜色失真，CFG 应该在 5-15 之间

`denoise strength` img2img 专属参数, 从 0 到 1 取值, 值越高 AI 对原图的参考程度就越低 (同时增加迭代次数), 个人喜欢低 CFG 高 denoise 重绘图, 高 CFG 低 denoise 改细节.

[一个小指南：RedditAbout](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)


## 生成图片发生BUG的自救


### 生成黑/绿图

[生成黑/绿图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs)

如果是 GTX 16xx 系列，启动参数需要加 `--precision full --no-half`, 因此如果显存不足还要加 `--medvram`。

如果是其他显卡而且加载了 VAE 时出现黑图，加入 `--no-half-vae` 参数[^2]。


### RuntimeError Sizes of tensors must match

(img2img) 如果出现 `RuntimeError: Sizes of tensors must match`，请调整输入图像的分辨率


### 彩虹混乱图

如果 AI 输出了混乱的彩虹色图片，则生成分辨率被设置得太低


### 高分辨率出鬼图 / 低显存生成大分辨率图片

简单说就是使用低分辨率重新生成或者超分。见钩吻

你可以在下个章节看到具体操作流程。


### RuntimeError: Unable to find a valid cuDNN algorithm to run convolution

前面那节有相关的参数建议。

生成报错解释：显存不足

先检查 CUDA 是否可用，打开命令窗，输入 python 并分行输入

```python
import torch
print(torch.cuda.is_available())
```

### CUDA out of memory

原因：显存不足。`--lowvram` 和 `--medvram` 启动参数都可以改善此问题。


## ckpt 文件安全问题[^4]

ckpt 文件被加载时基本上可以执行任何内容，盲目加载有安全风险。请检查来源是否可靠再加载。
如果杀毒软件拦截，有可能创建者向文件中注入了恶意的 Python 代码。

可以通过此脚本检查风险：<https://rentry.org/safeunpickle2>




# 进阶

## 启动流程[^6]

![Roaming_info.png](https://user-images.githubusercontent.com/75739606/198679721-2a7b38b8-41f3-405c-9ea3-40f1b5e8cc7e.png)
<!--
![Roaming_info.png](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/infofrom_bili_uid_87077691.png)
-->
>来自 allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/


## 使用 WebUI 复现 NAI 官网

[相关讨论，应该读一读！](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017)

提示:由于 torch 及其相关框架的性质，尝试完全复原在不同机器上生成的图片是不明智的。所以不要纠结一些细节不能复现。


### 需要做的事情

* 加载 VAE 和模型附带的 `config.yaml` (可选，有人说此操作空耗显存)

* `Stop At last layers of CLIP model` 设为 `2`

* `Eta noise seed delta` 设置为 `31337`


### **不需要**做的事情

* hypernetwork。官网默认并不使用 hypernetwork


设置 `Stop At last layers of CLIP model` 是为了匹配 NAI 的一个[优化](https://blog.novelai.net/novelai-improvements-on-stable-diffusion-e10d38db82ac)。


## 半精度还是单精度？

如果能，尽量使用半精度，可以节省运算时间/RAM/VRAM，同时图片质量并不会和单精度差多少。~真要说差别可能和你电脑被宇宙射线打了差不多~。


## X/Y 图

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

## **Variations种子变化**

`Variation strength slider` 和 `Variation seed field` 允许您指定现有图片应更改多少以使其看起来不同。
在最大强度下，您将获得带有变异种子的图片，至少 - 带有原始种子的图片（使用先前采样器时除外）。

## **提示词模板**

“Save prompt as style” 按钮将当前的提示写入 `styles.csv`，该文件包含样式集合

提示右侧的下拉框将允许您从以前保存的样式中选择任何样式，并自动将其**附加**到输入中

要删除样式，请从 `styles.csv` 中手动将其删除并重新启动程序。


## xformers

xformers 分辨率越高加速效果越好。使用 xformers 会引入一些随机性，稍微影响生成的图像。

要启用，如果你是 Pascal、Turing 或者 Ampere 架构的卡（包括 GTX 1000，RTX 2000、3000 系列），添加 `--xformers` 参数到 `webui-user.bat` 中的 `COMMANDLINE_ARGS`。

!!! tip
    有人说在 700 和 900 系列卡上使用 xformers 的性能明显较差，请注意这一点。
    本人实测，2050 在启用 xformers 之后，速度慢了 50%


### 在 Windows 上编译 Xformers

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


### Windows 编译错误自查

>错误:`Filename too long` 和 `fatal error C1083: Cannot open compiler generated file: '': Invalid argument`

说明你的路径太长了。

>RuntimeError: CUDA error: no kernel image is available for execution on the device

现在更多 GPU 架构是自动支持的，尝试重新安装并使用 --xformers 参数。

如果你移动了 Xformers，那么应该删除里面的 venv 目录

[Windows](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases) (30 系之外要自己编译)

自己编译指路 [wiki/Xformers](https://rentry.org/sdg_faq#xformers-increase-your-its-more-cards-supported)， 还有[这篇文章](https://www.reddit.com/r/StableDiffusion/comments/xz26lq/automatic1111_xformers_cross_attention_with_on/)


## 使用CPU进行绘画

根据此 [pr](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2597)

可以通过 `--use-cpu all` 尽可能的使用 CPU 进行生成，虽然慢 100 倍。



## CLIP Interrogate

CLIP 可以从图像中提取令牌。

默认情况下，只有一个列表 - 艺术家列表（来自 artists.csv）。

不过你可以通过执行以下操作添加更多列表：
* interrogate 在与 WebUI 相同的位置创建目录
* 将文本文件放入其中，每行都有相关描述


你可以在[这里](https://github.com/pharmapsychotic/clip-interrogator/tree/main/data)查看使用哪个文本文件的例子。实际上，你可以直接用这个例子中的文件 —— 除了 `artists.txt` ，你已经有一份艺术家列表在 `artists.csv` 中了不是吗。

每个文件都会使最后的描述增加一行字。如果你将 `.top3.` 放到文件名中，比如 `flavors.top3.txt` ，文件中相关度最高的三行将会被添加到提示词中（其他数量也行）。

<!--
这里的最后一句 “文件中相关度最高的三行”，关于 “相关度” 可能让人有点迷惑，后来的人可以看看有没有什么更贴切的翻译
For example of what text files to use, see https://github.com/pharmapsychotic/clip-interrogator/tree/main/data.
In fact, you can just take files from there and use them - just skip artists.txt because you already have a list of artists in artists.csv (or use that too, who's going to stop you).
Each file adds one line of text to the final description.
If you add ".top3." to filename, for example, flavors.top3.txt, the three most relevant lines from this file will be added to the prompt (other numbers also work).
-->

## **Face restoration三次元人脸修复**

适用于三次元。

[https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration)

## 自定义.css

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

## notification.mp3 提示音

放在 WebUI 的根目录的名为 `notification.mp3` 的音频文件将会在处理完成后播放。

## 开发自定义脚本

你可以在 `modules/scripts.py` 中找到 Script 类。

如果要创建你自己的自定义脚本，请创建一个实现类的 Python 脚本，并将其放到 scripts 文件夹中，使用以下示例或文件夹中已有的其他脚本作为指导。

Script 类有四个主要方法，这里通过一个简单的[示例脚本](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Developing-custom-scripts)进行更详细的描述，这个脚本可以旋转和/或翻转生成的图像。

## 修剪模型[^7]

将要修剪的 `.ckpt` 文件放在 `/stable-diffusion-webui` 文件夹，把 [脚本](https://raw.githubusercontent.com/harubaru/waifu-diffusion/main/scripts/prune.py) 另存本地，删除第 6 行和 第 8 行。然后在 prune.py 中的最后一行编辑 ckpt 的名称。

比如，`prune_it('wd-v1-2-full-emma.ckpt')`

然后运行这个脚本，修剪过程可能需要几分钟。

# 运行

## 4GB 显卡支持

针对具有低 VRAM 的 GPU 的优化。这应该可以在具有 4GB 内存的视频卡上生成 512x512 图像。

`--lowvram` 是 basujindal 对优化思想的重新实现。模型被分成模块，GPU 内存中只保存一个模块；当另一个模块需要运行时，前一个模块将从 GPU 内存中删除。这种优化的性质使处理速度变慢——与我的 RTX 3090 上的正常操作相比，速度慢了大约 10 倍。

`--medvram` 是另一个优化，通过不在同一批次中处理条件和无条件去噪，可以显着减少 VRAM 的使用。这种优化的实现不需要对原始的稳定扩散代码进行任何修改。

!!! info
    经过 10/10 的优化，RTX2050 的 4GB 显卡也可以使用 `--medvram` 。

当然也可以减半精度，或者生成一张 64x64 清理 VRAM

## 不间断生产

在 WebUI 的生成键右击即可出现**不间断生成**的选项。

## 图片信息 Png info

生成的图片自带令牌信息，拖放到查看页面即可查看 。




## NAI 4chan简化版本

4chan 版本魔改官后程序，会动态分配，显存不够内存来凑。

[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

[^3]:[16xx系显卡可以使用半精度生成图片的方式](https://t.me/StableDiffusion_CN/50749)

[^4]:[It's not a virus it's a checkpoint file](https://huggingface.co/Deltaadams/Hentai-Diffusion/discussions/12)

[^5]:[所有超网络的 X/Y](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3836360)

[^6]:[Roaming_info_for_latent_diffusion](http://allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/)

[^7]:[pruning-a-ckpt](https://rentry.co/voldy#-pruning-a-ckpt-)
