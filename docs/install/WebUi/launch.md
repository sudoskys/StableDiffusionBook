 
### 启动

但是必须首先下载一个4GB模型

如果你运行报错，请读前面的自救提示。

不提供NV模型，你可以去 关于/底部图标 页面找到 `中文社区` 的频道，里面应该有你想要的哈。


**按照后面的教程装载入模型后**

Win 运行 `webui.bat`

Linux 用户采用
```
COMMANDLINE_ARGS="--medvram --always-batch-cond-uncond" REQS_FILE="requirements.txt" python launch.py
```

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

#### 中文/本地化

本地化作为单个 `.json` 文件提供。 将此文件放入 `localizations` 目录并在设置中选择即可。

在 22/10/26 下午，WebUi 添加了中文的翻译。

**创建本地化文件**

转到设置并单击 `Download localization template` 底部的按钮，下载一个可以编辑的本地化模板。

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

    10/30 社区报告：有人在扫描公开的 WebUi

    11/1 社区反映：共享链接可能会导致风险，**攻击者可以访问系统上的所有文件。**


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


[讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3889) 有人认为，通过在 Windows 设置上禁用硬件加速 GPU 调度，WebUi 性能提高了大约 10-50%


#### API 文档

使用 `--api` 参数运行程序，在浏览器访问 `{输出的网址}/docs` 就可以查看到 WebUi 的 [Api](https://github.com/AUTOMATIC1111/stable-diffusion-webui/tree/master/modules/api) 文档。[^4]

下面是一个同步类实现。

```python
import time
import json
import requests
import io
import base64
from PIL import Image, PngImagePlugin

class WebUiApi(object):
    def __init__(url):
        self.url=url

    def txt2img(payload,outpath:str=None,infotie:bool=True):
        payload_json = json.dumps(payload)
        response = requests.post(url=f'{self.url}/sdapi/v1/txt2img', data=payload_json).json()
        # response 响应包含 images、parameters 和 info,image 可能会含有多个图像。
        for i in response['images']:
            # 解码 base64
            image = Image.open(io.BytesIO(base64.b64decode(i)))
            # 元信息输出
            pnginfo = PngImagePlugin.PngInfo()
            if infotie:
               pnginfo.add_text("parameters", str(response['info']))
            # 保存，因为本地不会自动生成文件。
            if not outpath:
               print("Random file name")
               outpath=f"{str(time.time())}.png"
            image.save(outpath, pnginfo=pnginfo)

payload = {
    "prompt": "1girl",
    "steps": 20
}
# 其他参数会使用默认值
WebUiApi(url="http://127.0.0.1:7860").txt2img(payload=payload,outpath="1145.png",infotie=True)

# 实际使用的时候不应该保存到本地再发送，而是直接发送，避免存储图片作品造成 版权 问题。
```

跨平台多后端项目 [novelai-bot](https://github.com/koishijs/novelai-bot)

Discord 机器人项目 [aiyabot](https://github.com/Kilvoctu/aiyabot/blob/main/core/stablecog.py)



### 错误处理 Troubleshooting

翻译整理自[^3]

文件路径，路径不允许含有空格，确保您的程序文件夹路径中没有空格。该程序经过测试可在 Python 3.10.6 上运行，低版本 Python 可能会发生错误。

重新安装，请删除目录： `venv`, `repositories`。


#### 网络问题/没响应

- Git 报错

HTTP_PROXY 和 HTTPS_PROXY 环境变量，或者使用 clash 的tun模式. 或者把 git clone 的仓库源换成 huggingface

其他依赖报错也需要设置代理或者使用镜像，不然特慢。


#### 时间花销过大

设镜像或者挂代理，**依赖项 >2GB**,请做好准备，而且对于Windows,**依赖默认安装在C盘**！


#### 低显存显卡/CUDA out of memory

确保你拥有可以运行的最新CUDA 工具包和 GPU 驱动程序。

程序在具有少量 VRAM (<=4GB) 的视频卡上运行时，可能会出现内存不足错误。

**牺牲速度来支持低显存显卡**

如果你有 `4GB VRAM` 并且想要制作 512x512（或者可能高达 640x640）的图像，请使用 `--medvram`

如果你有 `4GB VRAM` 并且想要制作 512x512 图像，但使用 `--medvram` 遇到内存不足错误 ，请改用`--medvram --opt-split-attention`

如果你有 `4GB VRAM` 并想要制作 512x512 图像，但仍然出现内存不足错误，请改用 `--lowvram --always-batch-cond-uncond --opt-split-attention`

如果你有 `4GB VRAM` 并且想要制作比你可以使用的更大的图像`--medvram`，请使用 `--lowvram --opt-split-attention`

如果你有更多的 `VRAM` 并且想要制作比你通常制作的更大的图像（例如 1024x1024 而不是 512x512），请使用`--medvram --opt-split-attention`.

你也可以使用 `--lowvram` ，启动这个参数后，模型被分成模块，GPU内存中只保存一个模块； 当另一个模块需要运行时，前一个从 GPU 内存中删除，这种牺牲使处理运行速度大约慢 10 倍。

如果还不行，建议你升级设备。


#### 我 Python 呢？

如果你的 Python 版本不在 PATH 中，则在文件夹中创建或修改 webui.settings.bat 添加行 `set PYTHON=python `来说明 Python 可执行文件的完整路径（请看参数说明表格）

#### ERROR:asyncio:Accept failed on a socket

先检查端口冲突，有没有和什么软件冲突

这个错误有可能是 `Python38` 的 `asyncio` 库对 `Windows` 的兼容性问题。

尝试用 CMD 管理员身份运行`netsh winsock reset`，不行的话，切换端口。

如果还是不行，请切换至 Python 的 `3.10.6`，WebUi 的开发环境为此版本。


#### 虚拟环境

如果你使用 conda 可以不使用一键脚本，可以自己运行 launch 安装依赖。

因为仓库给出的一键安装脚本会创建虚拟环境，然后启动 [launch.py](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/launch.py)。

在运行时，一键安装程序会创建一个 python 虚拟环境，因此如果你在安装之前安装了一个模块，那么任何已安装的模块都不会引发冲突。

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

[^3]:[Wiki2](https://rentry.co/voldy)

[^4]:[Basic Documentation and Examples for using API](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3734)
