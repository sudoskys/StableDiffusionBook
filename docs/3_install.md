# 安装


## 安装 Stable-Diffusion-webui 开源框架

此教程参考了 crosstyan[^2] 的 [FAQ](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#prompt-editingmixing)

WebUi的官方代码仓库地址为 https://github.com/AUTOMATIC1111/stable-diffusion-webui

[官方英文安装教程](https://github.com/AUTOMATIC1111/stable-diffusion-webui#installation-and-running)

[官方给定的依赖列表](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Dependencies)


### 硬件要求

WebUi 最低能用N卡显存为 2GB

WebUi 最低可用N卡显存为 4GB


在开始前，打开CMD， 输入 `nvidia-smi` 看看机器的英伟达显卡是否正常运作。

![效果.png](https://s1.ax1x.com/2022/10/10/xtdNNR.png)
>这里是9月份效果(现在快了不少)


### 安装

!!! tip
    推荐有能力的同学阅读一下[官方Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki)的内容。

    注意！Ai 需要的依赖大小很大，需要做好心理准备。总计依赖大小约几G，在中国环境可能很慢！
    
    而且对于Windows,**依赖默认安装在C盘**，请确保空间充足。

    不推荐使用 整合包，会导致升级困难。

WSL 不可以迁移，如果 C 盘不够，寄！

#### **Windows安装**

在Windows上的安装流程如下。

**安装 Python**

安装[Python 3.10.6](https://www.python.org/downloads/windows/)，安装时选中“Add Pythone to PATH”

>为什么整合包不需要安装 Python？他们的脚本中指定了同目录下的环境，阅读之后的文档就可以知道


!!! tip "如果你在安装依赖时遇到了网络问题，需要按照以下步骤切换至国内镜像"

    打开 CMD 命令行（Win+R 运行 cmd），输入`Python` 回车，如果能进入 Python 就 ctrl+z (或通过输入 `exit()` )退出。
    然后输入以下命令设置镜像。
    ```bash
    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
    ```


**安装 Git**

在 [GitScm](https://git-scm.com/download/win) 下载 Git安装包并安装，然后在开始菜单中找到 `git bash` 

或打开想要放置的目录，右键打开快捷菜单中寻找 `Git Base Here`

亦或在想要运行的目录下打开 CMD


在命令框中运行 `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git` 拉取仓库，这里如果如果失败请设置代理。

!!! tip 
    如果你不会设置代理，可以使用以下命令拉取镜像
    
    `git clone https://hub.fastgit.org/AUTOMATIC1111/stable-diffusion-webui.git `


**尝试安装**

将 4G 或 7G 或 其他模型 `model.ckpt` 放在 `models` 目录中，`WebUi` 目前支持在界面顶部选择模型。

（可选）将`GFPGANv1.4.pth`放在基本目录中，旁边是 `webui.py`

先确认你的显卡是可用的(看上文)，然后以普通非管理员用户身份从 Windows资源管理器 运行 `webui-user.bat` 。脚本会自动下载依赖（**一般需要 22 分钟，默认安装在C盘Python下，下载的资源为1.98GB，解压后大小2GB以上**）并输出 启动网页界面 的地址。

在此区间你可以打开资源管理器，关注网络速度。在没有完成下载之前，脚本不会继续输出任何提示。(等了两小时？可以试试挂代理)

大约 30 分钟后安装完毕，程序会输出一个类似 `http://127.0.0.1:7860/` 的地址，点开即可(注意是http,且不指定端口的话可能会变动)。



!!! tip
    运行需要模型！
    
    如果你是瞄准二次元来的，请想办法搞到某个模型咯，至于模型选哪个，请看下一章节。如果你是整合包，也可以移植整合包其中的`models`文件夹。（甚至你可以直接去下载拆分整合包....？但是直接使用整合包迟早要走自己安装的流程，因为不能更新）

    如果对其他模型感兴趣，（没错就是艺术，大概是梵高那类的）这里提供一个模型网站 [Stable Diffusion Models](https://rentry.org/sdmodels#)，可以通过下载 `2.Stable Diffusion v1.4 [4af45990] [7460a6fa] [06c50424]` 这一个模型来运行 `Stable Diffusion` 的原始模型


#### Windows 从整合包迁移


使用上面的命令拉取代码但先不运行，然后将 Bat 文件和里面指定的文件夹复制到全新环境，把 `models` 和 `outputs` 以及 `embeddings` 复制到新文件夹，自行处理冲突。

!!! tip "命令"

    此处的命令指的是安装完Git之后的内容

    即为 `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git`
    
    或在无代理的情况下 `git clone https://hub.fastgit.org/AUTOMATIC1111/stable-diffusion-webui.git`

然后在新代码目录下，尝试运行原来的 Bat，如果报错，就根据提示排查是不是 NotFound 什么依赖，或者根据下面的命令说明修改 Bat 文件。

如果你不想迁移你的整合包，你也可以参考 [带图片的教程](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#%E6%87%92%E4%BA%BA%E5%8C%85%E6%94%B9-git-%E4%BB%93%E5%BA%93%E8%BF%9B%E8%A1%8C%E6%9B%B4%E6%96%B0)

#### Linux 安装

先安装依赖
```bash
# Debian-based:
sudo apt install wget git python3 python3-venv
# Red Hat-based:
sudo dnf install wget git python3
# Arch-based:
sudo pacman -S wget git python3
```

一键脚本

```bash
bash <(wget -qO- https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh)
```

完毕。

!!! info
    如果可以，尽量使用 `miniconda` （anaconda 特别巨大....），创建一个 Python 3.10.6 的虚拟环境。
   
    ```bash
    conda create -n aidraw python=3.10.6

    conda activate aidraw

    COMMANDLINE_ARGS="--medvram" REQS_FILE="requirements.txt" python launch.py
    ```


#### 更新框架

使用Git下载的源代码，可以直接使用 `git pull` 更新代码。推荐经常更新，并关注社区讨论。

如果是整合或者其他，请向上游（谁分发的）索取。


### 错误处理

翻译整理自[^3]



#### 首先...

路径不允许含有空格，确保您的文件夹路径没有空格。


#### 网络问题/没响应

- Git 报错

HTTP_PROXY 和 HTTPS_PROXY 环境变量，或者使用 clash 的tun模式. 或者把 git clone 的仓库源换成 huggingface

其他依赖报错也需要设置代理或者使用镜像，不然特慢。


#### 时间花销过大

设镜像或者挂代理，**依赖项 >2GB**,请做好准备，而且对于Windows,**依赖默认安装在C盘**！


#### 低显存显卡/CUDA out of memory

确保你拥有可以运行的最新CUDA 工具包和 GPU 驱动程序.

在具有少量 VRAM (<=4GB) 的视频卡上运行时，可能会出现内存不足错误。可以通过命令行参数启用各种优化，牺牲一些/很多速度来支持使用更少的 VRAM：

如果你有 4GB VRAM 并且想要制作 512x512（或者可能高达 640x640）的图像，请使用 `--medvram`

如果你有 4GB VRAM 并且想要制作 512x512 图像，但使用 `--medvram` 遇到内存不足错误 ，请改用`--medvram --opt-split-attention`

如果你有 4GB VRAM 并想要制作 512x512 图像，但仍然出现内存不足错误，请改用 `--lowvram --always-batch-cond-uncond --opt-split-attention`

如果你有 4GB VRAM 并且想要制作比你可以使用的更大的图像`--medvram`，请使用 `--lowvram --opt-split-attention`

如果你有更多的 VRAM 并且想要制作比你通常制作的更大的图像（例如 1024x1024 而不是 512x512），请使用`--medvram --opt-split-attention`.

你也可以使用 `--lowvram` ，启动这个参数后，模型被分成模块，GPU内存中只保存一个模块； 当另一个模块需要运行时，前一个从 GPU 内存中删除，这种牺牲使处理运行速度大约慢 10 倍。 

如果还不行....


#### 我 Python 呢？

如果你的 Python 版本不在 PATH 中，则在文件夹中创建或修改 webui.settings.bat 添加行 `set PYTHON=python `来说明 Python 可执行文件的完整路径（请看下面的参数说明！

#### ERROR:asyncio:Accept failed on a socket

先检查端口冲突，有没有和什么软件冲突

这个错误有可能是 `Python38` 的 `asyncio` 库对 `Windows` 的兼容性问题。

尝试用 CMD 管理员身份运行`netsh winsock reset`，不行的话，切换端口。

如果还是不行，请切换至 Python 的 `3.10.6`，WebUi 的开发环境为此版本。


#### 虚拟环境

如果你使用 conda 可以不使用一键脚本，可以自己运行 launch 安装依赖。

因为仓库给出的一键安装脚本会创建虚拟环境，然后启动 [launch.py](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/launch.py)。

在运行时，一键安装程序会创建一个 python 虚拟环境，因此如果你在安装之前安装了一个模块，那么任何已安装的模块都不会影响你

如果需要防止创建虚拟环境而使用系统 python，编辑 `webui.bat` 替换 `setVENV_DIR=venv`为`set VENV_DIR=`

#### api-ms-win-core-path-l1-1-0.dll is missing[^3]

Windows 7上运行很可能会报错: `api-ms-win-core-path-l1-1-0.dll is missing`，这是因为许多程序需要新版本的 Windows 的系统文件。

如果没有出现该错误，不需要执行下面的操作。 

这些文件已经被移植来与 W7 兼容，并且可以在 [这里](https://github.com/nalexandru/api-ms-win-core-path-HACK/releases/download/0.3.1/api-ms-win-core-path-blender-0.3.1.zip) 下载。它的 [Github 页面](https://github.com/nalexandru/api-ms-win-core-path-HACK/)

解压缩并将 `x86.dll` 复制到 `C:\Windows\SysWOW64` ，将 `x64.dll` 复制到 `C:\Windows\System32` 并重启。

#### an illegal memory access was encountered ....CUDA kernel errors...

[相关问题Issue](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/1766)

在多数情况下，这代表显存溢出，也有可能是 GPU 硬件问题。

据说使用 deepdanbooru 的话会有这个提示，可以尝试重新启动 或 安装cpu版本的deepdanbooru.



**生成图片问题见下一章**



### 启动(但是必须首先下载一个4GB模型)

如果你运行报错，请读前面的自救提示。

不提供NV模型，你可以去 关于/底部图标 页面找到 `中文社区` 的频道，里面应该有你想要的哈。


**按照后面的教程装载入模型后**

Win 运行 `webui.bat`

Linux 用户采用
```
COMMANDLINE_ARGS="--medvram --always-batch-cond-uncond" REQS_FILE="requirements.txt" python launch.py
```

#### 共享链接

- 使用 `--share` 选项来在线运行

你会得到一个 `xxx.app.gradio` 链接，这是在协作中使用该程序的预期方式。

!!! danger "RCE"

    当使用 `--share` 参数时，你的实例就可以在公开互联网访问了。
    但是最近有人报告了一个可能是危险的[代码漏洞](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2571)

    所以，你可以使用参数为 `gradio` 共享实例设置身份验证：`--gradio-auth username:password` (可选择提供多组用户名和密码，以逗号分隔)

    例子
    `--share --gradio-auth admin:admin,user1:user_password`

    使用该例子将会创建两个用户，一个是账号密码为admin的用户，另外一个是账号为user1,密码为user_password的用户
    
    
    如果攻击者可以访问 ui，他们可能能够远程运行 python 脚本。

你会得到一个 `xxx.app.gradio` 链接，这是在协作中使用该程序的预期方式，而且你可以使用参数为所述 `gradio` 共享实例设置身份验证：`--gradio-auth username:password`
可选择提供多组用户名和密码，以逗号分隔。

你可以通过添加 `--freeze-settings`  启动参数来锁定设置，防止他人编辑。

- 端口转发

参数 `--listen` 使服务器监听网络连接。

这将允许本地网络上的计算机访问 UI，如果配置端口转发，公网上的计算机也可以访问（当然你得有公网。

注意，监听 `0.0.0.0` 就意味着对本地网卡所有ipv4监听，访问请使用 `localhost` 替换 `0.0.0.0`

- 监听端口

参数 `--port xxxx` 使服务器监听特定端口。

低于 1024 的所有端口都需要 `root`权限，因此建议使用高于 1024 的端口。

如果可用，则默认为端口 7860

- 主题

使用 `--theme` 参数，使用主题


#### 自定义运行

进一步熟悉这个程序，你会发现可以修改 Bat运行脚本 添加参数！具体参数列表请读下文。

生成报错？请读 `绘画调试` 章节。

首先，你可以运行 `python webui.py --help` 查看所有命令参数，或者在[源码文件](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/shared.py)中读到它们。 

Tip:
自定义程序运行方式的推荐方法是编辑webui-user.bat(Windows) 和webui-user.sh(Linux)


*环境定制*

`set PYTHON=b:/soft/Python310/Python.exe`

`set PYTHON` 允许设置自定义 Python 路径

`set VENV_DIR=C:\run\var\run`
set VENV_DIR允许您选择虚拟环境的目录。默认为`venv`。特殊值`-`在不创建虚拟环境的情况下运行脚本。

`set COMMANDLINE_ARGS=--ckpt a.ckpt`
set COMMANDLINE_ARGS设置命令行参数webui.py运行
示例使用模型a.ckpt而不是model.ckpt

*GPU指定*

选择要使用的默认Gpu `--device-id 0`，来代替旧版本的 `CUDA_VISIBLE_DEVICES=0`，可以选择第二个 GPU 允许同时运行两个实例，从而能够以更简洁的方式简单地选择设备。

查看 GPU 序号，可以使用 `nvidia-smi`


**优化命令参数**

来自 [官方Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Optimizations)


| 命令行参数    | 解释 |
| ----------- | ----------------------------------------- |
| `--xformers`                    | 使用[xformers](https://github.com/facebookresearch/xformers)库。极大地改善了内存消耗和速度。Windows 版本安装由[C43H66N12O12S2 维护](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases)的二进制文件|
| `--force-enable-xformers`       | 无论程序是否认为您可以运行它，都启用 xformers。不要报告你运行它的错误。     |
| `--opt-split-attention`         | Cross attention layer optimization 优化显着减少了内存使用，几乎没有成本（一些报告改进了性能）。黑魔法。默认情况下`torch.cuda`，包括 NVidia 和 AMD 卡。|
| `--disable-opt-split-attention` | 禁用上面的优化|
| `--opt-split-attention-v1`      | 使用上述优化的旧版本，它不会占用大量内存（它将使用更少的 VRAM，但会限制您可以制作的最大图片大小）。|
| `--medvram`                     | 通过将稳定扩散模型分为三部分，使其消耗更少的VRAM，即cond（用于将文本转换为数字表示）、first_stage（用于将图片转换为潜在空间并返回）和unet（用于潜在空间的实际去噪），并使其始终只有一个在VRAM中，将其他部分发送到CPU RAM。降低性能，但只会降低一点-除非启用实时预览。|
| `--lowvram`                     | 对上面更彻底的优化，将 unet 拆分成多个模块，VRAM 中只保留一个模块,破坏性能|
| `*do-not-batch-cond-uncond`     | 防止在采样过程中对正面和负面提示进行批处理，这基本上可以让您以 0.5 批量大小运行，从而节省大量内存。降低性能。不是命令行选项，而是使用`--medvram`or 隐式启用的优化`--lowvram`。   |
| `--always-batch-cond-uncond`    | 禁用上述优化。只有与`--medvram`或`--lowvram`一起使用才有意义 |
| `--opt-channelslast`            | 更改 torch 内存类型，以稳定扩散到最后一个通道,效果没有仔细研究。|

#### 本地化

本地化作为单个 `.json` 文件提供。 将此文件放入 `localizations` 目录并在设置中选择即可。


**创建本地化文件**

转到设置并单击 `Download localization template` 底部的按钮，下载一个可以编辑的本地化模板。 

#### 令牌参数生成第一幅图片

咳咳，这里使用的是某个模型，示例仅仅适用于 `那个模型`。

令牌请读后面的内容，这里给出一个实例来供你完成测试。

顶上那个是选择模型，暂时用不到。

- 第一个框是正向令牌

可以使用 颜文字 和 emoji，使用 () 来增强权重，具体规则见后

```
((masterpiece)), best quality, illustration, 1 girl, beautiful,beautiful detailed sky, catgirl,beautiful detailed water, cinematic lighting, dramatic angle, Grey T-Shirt, (few clothes),(yuri),(medium breasts),white hair,cat ears,masturbation,bare shoulders ,(gold eyes),wet clothes
```

- 第二个框是反向令牌

指定需要过滤什么标签

```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet
```

**第一次调整参数**

选定分辨率参数，越大越慢越卡。

生成后的结果结尾类似为
```
Steps: 28, Sampler: Euler a, CFG scale: 7, Seed: 2706937631, Size: 1024x512, Model hash: 925997e9
```
通过一样的参数和 Seed(-1就是随机)，可以生产一样的图像，这用于微调！
还有占用信息
```
Time taken: 33.97s
Torch active/reserved: 1975/2144 MiB, Sys VRAM: 7890/8134 MiB (93.61%)
```

**快捷设置**

不想来回设置 Clip？

添加`sd_hypernetwork`和`CLIP_stop_at_last_layers`到设置页面的`Quicksettings list`，保存并重新启动 webui，你就可以在Ui顶部看到一个快速切换选项啦～


## 使用 Colab 搭建

<!--
Colab 开始反滥用，目前 Colab 的[协议](https://research.google.com/colaboratory/faq.html#limitations-and-restrictions) `禁止穿透` ，如果你使用，会承担风险。

但是有人指出 [Colab并没有计划封禁 SD](https://github.com/googlecolab/colabtools/issues/3147)
-->

Colab 资源每日重置。

!!! tip
    为什么不放笔记本地址？

    因为 Colab 是谷歌开放的一款免费的研究工具，不应将其用于生产环境。

    在 10/23 晚，有人反映 Colab 开始反滥用。


具体笔记本地址请关注中文社区 t.me@StableDiffusion_CN_WIKI


## 在 AMD 上安装

[读Wiki此页](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)


## 在 Apple Silicon 上安装

[读Wiki此页](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Installation-on-Apple-Silicon)


--------


## 安装 Novel Ai 的原版后端

阅读此教程:[NovelAI 原版网页UI+后端部署教程](https://telegra.ph/NovelAI-%E5%8E%9F%E7%89%88%E7%BD%91%E9%A1%B5UI%E5%90%8E%E7%AB%AF%E9%83%A8%E7%BD%B2%E6%95%99%E7%A8%8B-10-10)


下面是一些简短的提示信息。

### 硬件需求

建议显存低于24G的就不要尝试使用官方版本前后端.

一台拥有一张至少有11G 显存的NVIDIA GPU的linux系统的x86设备。

#### 软件需求

- NVIDIA驱动(CUDA 11.6 Toolkit)

- Docker 19+

- nvidia-container-toolkit

#### 安装docker


```
curl -fsSL https://get.docker.com | bash
```

**国内镜像**

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```


#### 驱动相关

**安装显卡驱动**

使用 `nvidia-smi` 查看显卡状态。

使用 `docker run --help | grep -i gpus` 确认nvidia-container-toolkit安装成功。



#### 运行相关

- 容器状态

查询出容器ID `docker ps`

查看容器LOG `docker logs [Container ID]`

出现 “Application startup complete.” 即代表程序已经就绪

**请求API**


参考代码，具体参照leak的前端后端项目，以及其中的`sd-private\hydra-node-http\main.py`

`prompt` 中 `masterpiece, best quality `, 开头对应原版 `web Add Quality Tags`  选项，不建议删除，后面直接跟自己 `prompt` 即可

uc 部分对应 *web Undesired Content*，建议保留默认

sampler 是采样方法，可选 `plms/ddim/k_euler/k_euler_ancestral/k_heun/k_dpm_2/k_dpm_2_ancestral/k_lms`

seed 是种子，自己随机一个整数数字，不然一直会出一样的结果

n_samples 代表要生成几张图片


```python
import requests
import json
import base64
import random

endpoint = "http://10.10.12.67/generate"
data = {
  "prompt": "masterpiece, best quality, brown red hair,blue eyes,twin tails,holding cat",
  "seed": random.randint(0,2**32),
  "n_samples": 1,
  "sampler": "ddim",
  "width": 512,
  "height": 768,
  "scale": 11,
  "steps": 28,
  "uc": "lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry"
}


req = requests.post(endpoint, json=data).json()
output = req["output"]

for x in output:
  img = base64.b64decode(x)

  with open("output-" + str(output.index(x)) + ".png", "wb") as f:
    f.write(img)
```



[^1]:[NovelAI原版部署教程](https://telegra.ph/NovelAI%E5%8E%9F%E7%89%88%E9%83%A8%E7%BD%B2%E6%95%99%E7%A8%8B-10-07)

[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

[^3]:[Wiki2](https://rentry.co/voldy)

