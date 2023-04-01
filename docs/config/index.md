# 配置

本页面讨论 WebUi 的界面参数和功能特性。具体的绘画参数调节和指导，请看下一章节。

## 基础

以下是一些基础参数的说明：

### 横条参数说明

- `Step`：迭代次数，取值与采样方法有关。采用 DDIM 采样方法收敛较快。具体差别请参考调参魔法 "Sampler vs. Steps Comparison (low to mid step counts)"。

- `Batch count/Batch size`：决定生成的图片数量。如果显存足够，则可以增加 Batch Size；否则，只能增加 Batch Count，得到的图片数量是两者之积（对于显存较小的情况，建议只修改 Batch Count）。

- `Sample method`：采样方法。DDIM、Eula 也非常好用。（带 a 的是 ancestral 的意思，step 增长出图不稳定）

- `CFG scale`：符合 prompt 的程度，值越低模型越有创意力。实际模型表现上来看，CFG Scale 低（6-8）时，效果偏杂乱；而高（18-22）时，饱和度偏高。

- `Denoise strength`：img2img 专属参数，取值范围为 0 到 1。值越高，AI 对原图的参考程度就越低（同时增加迭代次数）。你可以在 CFG 较低时调高 Denoise 来重新绘图，在 CFG 较高时调整到低 Denoise 改变画面细节。

### 图片信息 Png info

生成的图片文件自动内嵌提示词信息，拖放到 `Png Info` 页面即可查看。

### 4GB 显卡支持

针对具有低 VRAM 的 GPU 的优化。这应该可以在具有 4GB 内存的视频卡上生成 512x512 图像。

`--lowvram` 是 basujindal 对优化思想的重新实现。模型被分成模块，GPU 内存中只保存一个模块；当另一个模块需要运行时，前一个模块将从 GPU 内存中删除。这种优化的性质使处理速度变慢——与我的 RTX 3090 上的正常操作相比，速度慢了大约 10 倍。

`--medvram` 是另一个优化，通过不在同一批次中处理条件和无条件去噪，可以显着减少 VRAM 的使用。这种优化的实现不需要对原始的稳定扩散代码进行任何修改。

!!! info
    经过 10/10 的优化，RTX2050 的 4GB 显卡也可以使用 `--medvram` 。

当然也可以减半精度，或者生成一张 64x64 清理 VRAM
### ckpt 文件安全问题 [^4]

当加载 ckpt 文件时，需要谨慎对待，因为它可能包含恶意代码。因此，在加载前应该仔细检查其来源的可靠性。

为了避免这种情况的发生，WebUi 自带有安全检查器，您可以通过添加`--disable-safe-unpickle`参数来禁用此检查。您使用以下脚本来自主检查模型的风险：<https://rentry.org/safeunpickle2>

[Pickle 的介绍](https://docs.python.org/3/library/pickle.html)，Python object serialization，相关项目 [pickle_inspector](https://github.com/lopho/pickle_inspector) 。

## 进阶

### 启动流程 [^6]

![Roaming_info.png](https://user-images.githubusercontent.com/75739606/198679721-2a7b38b8-41f3-405c-9ea3-40f1b5e8cc7e.png)
<!--
![Roaming_info.png](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/infofrom_bili_uid_87077691.png)
-->
> 来自 allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/

### 半精度还是单精度？

如果能，尽量使用半精度，可以节省运算时间/RAM/VRAM，同时图片质量并不会和单精度差多少。~真要说差别可能和你电脑被宇宙射线打了差不多~。

### X/Y 图

创建具有不同参数的图像网格。使用 X 类型和 Y 类型字段选择应由行和列共享的参数，并将这些参数以逗号分隔输入 X 值/Y 值字段。支持整数、浮点数和范围。

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
> 引用官方 Wiki 的设置图

#### Prompt S/R 替换

提示 S/R 是 X/Y 图的的一种操作模式。

S/R 是搜索/替换的意思，输入一个单词或短语的列表，它从列表中取第一个并将其视为关键词，并将该关键词的所有实例替换为列表中的其他条目的所有实例替换为列表中的其他条目。

例如，提示 `a man holding an apple, 8k clean` 和 S/R 提示 `an apple, a watermelon, a gun` 结合，你会得到三个提示。

* `a man holding an apple, 8k clean`
* `a man holding a watermelon, 8k clean`
* `a man holding a gun, 8k clean`

列表使用的语法与 CSV 文件中的一行相同，所以如果你想在你的条目中加入逗号，你可以
在你的条目中加入逗号，你必须将文本放在引号中，并确保引号之间没有空格。
确保引号和分隔逗号之间没有空格。

* `darkness, light, green, heat` - 4 items - `darkness`, `light`, `green`, `heat`
* `darkness, "light, green", heat` - WRONG - 4 items - `darkness`, `"light`, `green"`, `heat`
* `darkness,"light, green",heat` - RIGHT - 3 items - `darkness`, `light, green`, `heat`

### **Variations 种子变化**

这个参数用于种子合并操作。

`Variation strength slider` 和 `Variation seed field` 允许您指定现有图片应更改多少以使其看起来不同。
在最大强度下，您将获得带有变异种子的图片，至少 - 带有原始种子的图片（使用先前采样器时除外）。

### **提示词模板**

“Save prompt as style” 按钮将当前的提示写入 `styles.csv`，该文件包含样式集合

提示右侧的下拉框将允许您从以前保存的样式中选择任何样式，并自动将其 **附加** 到输入中

要删除样式，请从 `styles.csv` 中手动将其删除并重新启动程序。

### xformers

xformers 分辨率越高加速效果越好。使用 xformers 会引入一些随机性，稍微影响生成的图像。

如果你是 Pascal、Turing 或者 Ampere 架构的卡（包括 GTX 1000，RTX 2000、3000 系列），添加 `--xformers` 参数到 `webui-user.bat` 中的 `COMMANDLINE_ARGS`，**不需要自己再次编译安装。**

!!! tip
    有人说在 700 和 900 系列卡上使用 xformers 的性能明显较差，请注意这一点。
    本人实测，2050 在启用 xformers 之后，速度慢了 50%

#### 在 Windows 上编译 Xformers

!!! info

    你可以在右边的链接下载预构建的 Xformers！https://rentry.org/25i6yn ，记得先查看 [GPU 架构](https://developer.nvidia.com/cuda-gpus)

!!! info
    
    截止 2023.01.23，已经不再需要手动编译 Xformers。见 [wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Xformers#xformers)
    Note: Both Windows and linux should no longer need to build. On 1/23/23 webui shifted from a user-built wheel to an [official wheel](https://pypi.org/project/xformers/0.0.16rc425/#history) along with other package upgrades, shown in [this PR](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/5939/commits/c091cf1b4acd2047644d3571bcbfd81c81b4c3af)

确保 Python 版本为 3.10 或更高版本（使用 `Python --version`)，然后安装

安装 [VS Build Tools 2022](https://visualstudio.microsoft.com/zh-hans/downloads/?q=build+tools)，运行安装时只需要选择 `Desktop development with C++`

安装 [CUDA 11.3](https://developer.nvidia.com/cuda-11.3.0-download-archive)，（后期版本未测试），选择 `custom`，VS 集成可能不需要

* 确认 nvcc 可用

`nvcc --version`

* 克隆 `xFormers` 存储库，在环境中激活它

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

* 由于 CUDA 11.3 很旧，需要 **强制启用** 它以在 MS Build Tools 2022 上构建。

在 CMD 设置 `set NVCC_FLAGS=-allow-unsupported-compiler"`

或在 Bash 设置 `export NVCC_FLAGS=-allow-unsupported-compiler`

* 查看你自己的 GPU 架构

[GPU 架构](https://developer.nvidia.com/cuda-GPUs)

比如说，如果你的 GPU 是 GTX 1070，基于该表，架构是 6.1
*CMD*  `set TORCH_CUDA_ARCH_LIST=6.1`

*BASH*  `export TORCH_CUDA_ARCH_LIST=6.1`

* 构建 xFormers，请注意构建需要很长时间（可能要 10-20 分钟），它最开始时可能会出现一些错误，但应该仍能正确编译。

* 安装在环境中 (Conda)

```bash
python setup.py build
python setup.py bdist_wheel
```

找到 dist 文件夹并将文件 `*.whl` 复制到 `stable-diffusion-webui`
在 `stable-diffusion-webui` 目录中安装 `.whl`。

如果构建的 whl 名称不同，请在下面的安装命令中更改文件名

```bash
MD
./venv/scripts/activate
#Bash
source ./venv/bin/activate
#WindowsBash
source ./venv/Scripts/activate
pip install xformers-0.0.14.dev0-cp310-cp310-win_amd64.whl
```

30 系显卡正常启动加 `--xformers` 参数，其他显卡加 `--force-enable-xformers` 参数

> COMMANDLINE_ARGS =

#### Windows 编译错误自查

> 错误：`Filename too long` 和 `fatal error C1083: Cannot open compiler generated file: '': Invalid argument`

说明你的路径太长了。

> RuntimeError: CUDA error: no kernel image is available for execution on the device

现在更多 GPU 架构是自动支持的，尝试重新安装并使用 --xformers 参数。

如果你移动了 Xformers，那么应该删除里面的 venv 目录

[Windows](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases) (30 系之外要自己编译）

自己编译-> [wiki/Xformers](https://rentry.org/sdg_faq#xformers-increase-your-its-more-cards-supported)， 还有 [这篇文章](https://www.reddit.com/r/StableDiffusion/comments/xz26lq/automatic1111_xformers_cross_attention_with_on/)

### 使用 CPU 进行绘画

根据此 [pr](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2597)

可以通过 `--use-cpu all` 尽可能的使用 CPU 进行生成，虽然慢 100 倍。

### CLIP Interrogate

CLIP 可以从图像中提取令牌。

默认情况下，只有一个列表 - 艺术家列表（来自 artists.csv）。

不过你可以通过执行以下操作添加更多列表：
* interrogate 在与 WebUI 相同的位置创建目录
* 将文本文件放入其中，每行都有相关描述

你可以在 [这里](https://github.com/pharmapsychotic/clip-interrogator/tree/main/data) 查看使用哪个文本文件的例子。实际上，你可以直接用这个例子中的文件 —— 除了 `artists.txt` ，你已经有一份艺术家列表在 `artists.csv` 中了不是吗。

每个文件都会使最后的描述增加一行字。如果你将 `.top3.` 放到文件名中，比如 `flavors.top3.txt` ，文件中相关度最高的三行将会被添加到提示词中（其他数量也行）。

<!--
这里的最后一句 “文件中相关度最高的三行”，关于 “相关度” 可能让人有点迷惑，后来的人可以看看有没有什么更贴切的翻译
For example of what text files to use, see https://github.com/pharmapsychotic/clip-interrogator/tree/main/data.
In fact, you can just take files from there and use them - just skip artists.txt because you already have a list of artists in artists.csv (or use that too, who's going to stop you).
Each file adds one line of text to the final description.
If you add ".top3." to filename, for example, flavors.top3.txt, the three most relevant lines from this file will be added to the prompt (other numbers also work).
-->

### **Face restoration 真人人脸修复**

适用于真人。

[https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration)

### 自定义 css 样式

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

Script 类有四个主要方法，这里通过一个简单的 [示例脚本](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Developing-custom-scripts) 进行更详细的描述，这个脚本可以旋转和/或翻转生成的图像。

### 修剪模型 [^7]

将要修剪的 `.ckpt` 文件放在 `/stable-diffusion-webui` 文件夹，把 [脚本](https://raw.githubusercontent.com/harubaru/waifu-diffusion/main/scripts/prune.py) 另存本地，删除第 6 行和 第 8 行。然后在 prune.py 中的最后一行编辑 ckpt 的名称。

比如，`prune_it('wd-v1-2-full-emma.ckpt')`

然后运行这个脚本，修剪过程可能需要几分钟。

### 不间断生产

在 WebUI 的生成键右击即可出现 **不间断生成** 的选项。

[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

[^3]:[16xx 系显卡可以使用半精度生成图片的方式](https://t.me/StableDiffusion_CN/50749)

[^4]:[It's not a virus it's a checkpoint file](https://huggingface.co/Deltaadams/Hentai-Diffusion/discussions/12)

[^5]:[所有超网络的 X/Y](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3836360)

[^6]:[Roaming_info_for_latent_diffusion](http://allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/)

[^7]:[pruning-a-ckpt](https://rentry.co/voldy#-pruning-a-ckpt-)
