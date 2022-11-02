
# 关于显卡

!!! tip
    注意显卡温度，有报道称显卡太热炸了。

**在这之前，请确定你已经配置并进入了环境。(可以使用 torch)**

先使用 `nvidia-smi` 判断显卡驱动是否可用。

再确定 CUDA 的配置情况：打开终端，启动 `python`，分行输入如下命令


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

如果出现了任何错误，请询问他人或使用搜索引擎解决。

## WebUi 多 GPU 支持

最简单的模式就是实现一个多数据并行处理的方法，通过 `--device-id` 参数启动多个实例。每个 GPU 加载一个模型，然后给她们分配工作。

考虑到这个项目所提供的功能众多，我（作者）认为可能需要一段时间才能在并行的情况下使用所有的功能。

[作者的回应](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/156#issuecomment-1241218733)

>Using memory from between two GPUs is not simple. I only have one so I can't research/develop this.


## WebUi 16xx系显卡使用半精度生成图片[^3]

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

