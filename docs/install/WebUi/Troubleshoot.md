# 错误处理

以下是关于错误处理和自检的使用文档：

如果您在使用过程中遇到任何错误，请按照以下步骤操作：

1. 保留最后几行的错误信息，然后截图。
2. 记录系统信息，包括操作系统、显卡型号、程序版本/来源以及是否为整合包。
3. 打开 bing.com 搜索错误信息，初步确定问题范围：是系统问题/环境问题/程序问题/配置问题。
4. 如果仍无解决方案，请打开 https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues 搜索关键词，查看是否有相同错误。
5. 如果求助他人后仍未解决，请恢复备份或重新配置。

## 自检

在使用本程序之前，请确保您已经完成了以下自检工作：

- 确认文件路径中不包含空格。
- Python 版本需大于等于 3.10.6。请注意低版本 Python 可能会出现错误。
- 如果需要重新安装，请删除目录 `venv` 和 `repositories`。

## 安装网络问题/没有响应

如果您在安装时遇到网络问题或没有响应，请尝试以下解决方案：

- 如果 Git 报错，请设置 HTTP_PROXY 和 HTTPS_PROXY 环境变量，或者使用 clash 的 tun 模式。另外，您也可以将 git clone 的仓库源换成 huggingface。
- 如果依赖报错，请设置代理或使用镜像，否则速度会特别慢。

## 安装时间过长

如果您安装时花费的时间过长，请尝试以下解决方案：

1. 如果网络不好，可以设置镜像或挂代理。
2. 请注意，依赖项可能大于 2GB。另外，对于 Windows 操作系统，依赖默认安装在 C 盘上。

## 低显存显卡/CUDA 内存不足

如果您拥有少量 VRAM（<=4GB）的显卡，在运行程序时可能会出现内存不足的错误。为了支持低显存显卡，您可以牺牲一定的处理速度。

以下是解决方案：

- 确保您拥有可以运行的最新 CUDA 工具包和 GPU 驱动程序。
- 如果您想制作 512x512（或者可能高达 640x640）的图像，并且拥有 4GB VRAM，请使用 `--medvram`。
- 如果您想制作 512x512 的图像，但使用 `--medvram` 时遇到内存不足错误，请改用 `--medvram --opt-split-attention`。
- 如果您想制作 512x512 的图像，并且仍然出现内存不足错误，请改用 `--lowvram --always-batch-cond-uncond --opt-split-attention`。
- 如果您想制作比您可以使用的更大的图像（例如，`--medvram`），请使用 `--lowvram --opt-split-attention`。
- 如果您想制作比您通常制作的更大的图像（例如 1024x1024 而不是 512x512），请使用 `--medvram --opt-split-attention`。
- 您也可以使用 `--lowvram`，但请注意这会使处理运行速度慢约 10 倍。

如果上述解决方案仍不能解决问题，请考虑升级您的设备。

## Python 版本

如果您的 Python 版本未在 PATH 中，请在文件夹中创建或修改 `webui.settings.bat` 并添加以下内容：

```
set PYTHON=python
```

请将 `python` 替换为您的 Python 可执行文件的完整路径。


## ERROR:asyncio:Accept failed on a socket

如果出现这个错误，可能是端口冲突或者与其他软件发生了冲突。

此外，也可能是 `Python38` 的 `asyncio` 库在 `Windows` 上的兼容性问题。解决方式可以尝试以下几种：

1. 使用管理员身份运行 CMD，并执行命令 `netsh winsock reset`

2. 切换端口

3. 切换至 Python 的 `3.10.6` 版本，因为 WebUi 的开发环境为此版本

## 虚拟环境

如果使用 conda 可以不使用仓库提供的一键安装脚本，可以自己运行 launch.py 安装依赖。

注意，仓库给出的一键安装脚本会创建虚拟环境，然后启动 [launch.py](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/launch.py)。

在运行时，一键安装程序会创建一个 Python 虚拟环境，因此如果在安装之前已经安装了某些模块，那么任何已安装的模块都不会引发冲突。

如果需要防止创建虚拟环境而使用系统 python，请编辑 `webui.bat` 替换 `setVENV_DIR=venv` 为 `set VENV_DIR=`。

## api-ms-win-core-path-l1-1-0.dll is missing

在 Windows 7 上运行时，可能会出现 `api-ms-win-core-path-l1-1-0.dll` 缺失的错误。这是因为许多程序需要新版本的 Windows 的系统文件。

以下是解决方法：

1. 下载 [api-ms-win-core-path-blender-0.3.1.zip](https://github.com/nalexandru/api-ms-win-core-path-HACK/releases/download/0.3.1/api-ms-win-core-path-blender-0.3.1.zip)

2. 解压缩，并将 `x86.dll` 复制到 `C:\Windows\SysWOW64`，将 `x64.dll` 复制到 `C:\Windows\System32`

3. 重启电脑

## an illegal memory access was encountered ....CUDA kernel errors...

通常情况下，这个错误表示显存溢出或者 GPU 硬件问题。

如果使用 deepdanbooru 时出现这个问题，可以尝试重新启动或安装 CPU 版本的 deepdanbooru。

有关此问题的更多信息请参见 [Issue](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/1766)。

## Caused by ProxyError-Cannot connect to proxy-RemoteDisconnected-Remote end closed connection without response

如果出现此错误，可能是代理设置问题。解决方法如下：

1. 检查全局变量是否有协议头（需要有）

2. 如果使用的是 Clash，请开启 TUN 模式

3. 如果没有开启代理，请尝试更新到最新版本

有关此问题的更多信息请参见 [Issue](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/491)。

## Failed to establish a new connection

如果出现这个错误，可能是代理环境变量设置有误。解决方法如下：

可以在“我的电脑”中右键点击属性，进入环境变量，删除代理环境变量。但注意操作要小心。

See [Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2684)

**生成图片问题见下一章**

[^3]:[Wiki2](https://rentry.co/voldy)
