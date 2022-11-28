# 异常排查

这里是一些关于设置不当引发的异常。因为不当书写提示词的排错指南 在 实践指南中。

## 生成黑/绿图

[生成黑/绿图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs)

如果是 GTX 16xx 系列，启动参数需要加 `--precision full --no-half`, 因此如果显存不足还要加 `--medvram`。

如果是其他显卡而且加载了 VAE 时出现黑图，加入 `--no-half-vae` 参数 [^2]。

## RuntimeError Sizes of tensors must match

(img2img) 如果出现 `RuntimeError: Sizes of tensors must match`，请调整输入图像的分辨率

## 彩虹混乱图

如果 AI 输出了混乱的彩虹色图片，则可能是生成分辨率被设置得太低。

## 高分辨率出鬼图 / 低显存生成大分辨率图片

使用 `Highres fix` 或 低分辨率 + 超分。

你可以在下个章节看到具体操作流程。

## RuntimeError: Unable to find a valid cuDNN algorithm to run convolution

前面那节有相关的参数建议。

生成报错解释：显存不足

先检查 CUDA 是否可用，打开命令窗，输入 python 并分行输入

```python
import torch
print(torch.cuda.is_available())
```

## CUDA out of memory

原因：显存不足。`--lowvram` 和 `--medvram` 启动参数都可以改善此问题。

## Api

如果需要启用 Api，启动参数加入 `--api`。如果你想让其他应用调用 Api, 追加 `--enable-insecure-extension-access`。
