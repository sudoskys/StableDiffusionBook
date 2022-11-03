# 错误处理

翻译整理自[^3]

文件路径，路径不允许含有空格，确保您的程序文件夹路径中没有空格。该程序经过测试可在 Python 3.10.6 上运行，低版本 Python 可能会发生错误。

重新安装，请删除目录： `venv`, `repositories`。


## 网络问题/没响应

- Git 报错

HTTP_PROXY 和 HTTPS_PROXY 环境变量，或者使用 clash 的tun模式. 或者把 git clone 的仓库源换成 huggingface

其他依赖报错也需要设置代理或者使用镜像，不然特慢。


## 时间花销过大

1. 网络不好的话，设镜像或者挂代理。

2. **依赖项 >2GB**,请做好准备。而且对于Windows,**依赖默认安装在C盘**


## 低显存显卡/CUDA out of memory

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


## 我 Python 呢？

如果你的 Python 版本不在 PATH 中，则在文件夹中创建或修改 webui.settings.bat 添加行 `set PYTHON=python `来说明 Python 可执行文件的完整路径（请看参数说明表格）

## ERROR:asyncio:Accept failed on a socket

先检查端口冲突，有没有和什么软件冲突

这个错误有可能是 `Python38` 的 `asyncio` 库对 `Windows` 的兼容性问题。

尝试用 CMD 管理员身份运行`netsh winsock reset`，不行的话，切换端口。

如果还是不行，请切换至 Python 的 `3.10.6`，WebUi 的开发环境为此版本。


## 虚拟环境

如果你使用 conda 可以不使用一键脚本，可以自己运行 launch 安装依赖。

因为仓库给出的一键安装脚本会创建虚拟环境，然后启动 [launch.py](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/launch.py)。

在运行时，一键安装程序会创建一个 python 虚拟环境，因此如果你在安装之前安装了一个模块，那么任何已安装的模块都不会引发冲突。

如果需要防止创建虚拟环境而使用系统 python，编辑 `webui.bat` 替换 `setVENV_DIR=venv`为`set VENV_DIR=`

## api-ms-win-core-path-l1-1-0.dll is missing[^3]

Windows 7上运行很可能会报错: `api-ms-win-core-path-l1-1-0.dll is missing`，这是因为许多程序需要新版本的 Windows 的系统文件。

如果没有出现该错误，不需要执行下面的操作。

这些文件已经被移植来与 W7 兼容，并且可以在 [这里](https://github.com/nalexandru/api-ms-win-core-path-HACK/releases/download/0.3.1/api-ms-win-core-path-blender-0.3.1.zip) 下载。它的 [Github 页面](https://github.com/nalexandru/api-ms-win-core-path-HACK/)

解压缩并将 `x86.dll` 复制到 `C:\Windows\SysWOW64` ，将 `x64.dll` 复制到 `C:\Windows\System32` 并重启。

## an illegal memory access was encountered ....CUDA kernel errors...

[相关问题Issue](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/1766)

在多数情况下，这代表显存溢出，也有可能是 GPU 硬件问题。

据说使用 deepdanbooru 的话会有这个提示，可以尝试重新启动 或 安装cpu版本的deepdanbooru.



**生成图片问题见下一章**

[^3]:[Wiki2](https://rentry.co/voldy)