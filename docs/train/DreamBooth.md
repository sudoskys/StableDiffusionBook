# DreamBooth

DreamBooth 的模型是一种新的文本到图像“个性化”（可适应用户特定的图像生成需求）扩散模型方法。

使用时，只需将模型导出为ckpt，然后您可以将其加载到您想要的UI 中。

本节使用  Shivam Shirao 的 [版本](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

## 配置要求

Windows 系统至少需要 16, Linux 系统要求显存大于 8 GB

## 训练

### Linux

直接用下面的笔记本里的代码，不过需要英文基础。

[Nyanko Lepsoni 的 Colab 笔记本](https://colab.research.google.com/drive/17yM4mlPVOFdJE_81oWBz5mXH9cxvhmz8)

[RcINS 的 Colab 笔记本](https://colab.research.google.com/drive/1C1vVZ59S4kWfL7jIsczyLpmxbD4cOA-k)

以上笔记本来自 [社区置顶](https://t.me/StableDiffusion_CN/196744)

[适用于Autodl的项目](https://github.com/crosstyan/dreambooth-scripts-for-autodl)

### Windows

来自 [^18]

Windows 上的 `Dreambooth` 可以采用 ShivamShrirao 的优化版本来节省显存，

[diffusers/examples/dreambooth at main · ShivamShrirao/diffusers · GitHub](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

但是，由于相关链接库的原因，用于在 `Linux` 上的部署方法无法直接在 windows 上使用，由于同样的原因，该优化版本在 colab 上最低 9.9G 的显存需求在 windows 上应该稍高，因此推荐**至少使用显存12G**的显卡。

>笔者仅在16G显存PC上测试成功，12G理论可以，待测试。

修改或覆盖原始库中的文件前请备份！

准备环境 `Git`，`Python`，`MiniConda` (或 `MiniConda` )。

以下步骤在 python3.8，windows10 22H2 中操作，其他环境未测试

**创建工作目录 ，在目录下构建python3.8的venv虚拟环境**

```bash
    python -m venv --system-site-packages venv_dbwin
    venv_dbwin\Scripts\activate
    python.exe -m pip install --upgrade pip
```

克隆 ShivamShrirao 的优化版本 dreambooth 到工作目录中并安装相关依赖（使用的 [构建版本](https://github.com/huggingface/diffusers/tree/7465397f33d5de75dcccc155e3fb9a232fcbb0a0),后续版本可能无法支持本文方法）

```bash
git clone https://github.com/ShivamShrirao/diffusers
cd diffusers
pip install -e .
cd examples\dreambooth
pip install -U -r requirements.txt
pip install OmegaConf
pip install pytorch_lightning
pip install einops
pip install bitsandbytes==0.34
```

完成后，在windows上需要根据 [这个Issue](https://github.com/TimDettmers/bitsandbytes/issues/30#issuecomment-1257676341) 中的方法实现bitsandbytes支持

将 [这个仓库](https://github.com/DeXtmL/bitsandbytes-win-prebuilt) 中的 `libbitsandbytes_cuda116.dll` 文件手动拷贝到工作目录下的 `venv_diffusers\Lib\site-packages\bitsandbytes` 中，位于 `libbitsandbytes_cuda116.so` 的旁边；

然后更改脚本以应用，可以手动修改，为方便也可以下载以下文件替换：

将 `cextension.py` - https://pastebin.com/jjgxuh8V 覆盖到 `venv_diffusers\Lib\site-packages\bitsandbytes` 目录。

将 `main.py` - https://pastebin.com/BsEzpdpw 覆盖到 `venv_diffusers\Lib\site-packages\bitsandbytes\cuda_setup` 目录。

**安装 PyTorch 和 Torchvision**

`pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116`

**进入 Python 并测试调用：**

```
    python
    >>>import bitsandbytes
```
如果没有报错证明安装成功（ `Ctrl + Z` 并回车退出 python）

**设置 accelerate**
```
    accelerate config

    In which compute environment are you running? ([0] This machine, [1] AWS (Amazon SageMaker)): 0
    Which type of machine are you using? ([0] No distributed training, [1] multi-CPU, [2] multi-GPU, [3] TPU [4] MPS): 0
    Do you want to run your training on CPU only (even if a GPU is available)? [yes/NO]:NO
    Do you want to use DeepSpeed? [yes/NO]:NO
    Do you wish to use FP16 or BF16 (mixed precision)? [NO/fp16/bf16]: fp16
```

按照上面的设置选项，也就是（0，0，NO，NO，fp16）

**修改显存优化**

因为无法使用 xformers ，所以使用 [此文件](https://github.com/lucidrains/memory-efficient-attention-pytorch/blob/main/memory_efficient_attention_pytorch/flash_attention.py) 中的优化方法作为代替。

将 attention.py -  https://pastebin.com/nmwTrGB9 覆盖到 `diffusers\src\diffusers\models` 目录。

至此环境配置完成，可以开始训练了。具体使用参数请查阅 ShivamShrirao 的 readme 文件和 notebook 文件

!!! info 
    我使用以下参数训练。

    ```bash
        accelerate launch --num_cpu_threads_per_process 8 diffusers/examples/dreambooth/train_dreambooth.py --pretrained_model_name_or_path=models/diffusers_model --pretrained_vae_name_or_path=models/diffusers_model/vae --output_dir=models --concepts_list="concepts_list.json" --with_prior_preservation --prior_loss_weight=1.0 --seed=1337 --resolution=512 --mixed_precision="fp16" --lr_scheduler="constant" --use_8bit_adam --gradient_accumulation_steps=1 --train_batch_size=1 --max_train_steps=800 --save_interval=10000 --learning_rate=1e-6 --num_class_images=100 --lr_warmup_steps=0 --gradient_checkpointing
    ```

![image](https://user-images.githubusercontent.com/44570237/198906326-21b4f779-f870-4012-84c1-d5ac1dae0411.png)

峰值显存占用正好为 12G，如果您只有一个显卡，显存不高于 12G，而且同时需要用于 Windows 系统显示，请关闭所有占用显存的程序或网页等，减少额外显存消耗再进行训练。

diffusers 不能直接使用 ckpt 文件进行训练，需要先进行转换，示例如下。

```bash
    python diffusers\scripts\convert_original_stable_diffusion_to_diffusers.py  --checkpoint_path model.ckpt  --original_config_file v1-inference.yaml  --scheduler_type ddim  --dump_path models/diffusers_model
```

训练完成同样要进行打包转换为 ckpt,即可用于 AUTOMATIC1111 的 WebUI 中。

```
    python diffusers\scripts\convert_diffusers_to_original_stable_diffusion.py  --model_path models/resultModel  --checkpoint_path result.ckpt  --half
```

>这是一个临时的解决方案，期待 Windows 官方适配的到来。

## 简单指南

[1^]

>以下皆为未证实的内容。如果你有不同意见，欢迎提出。

- 准备训练图像

对于你的训练图片，你将需要:

至少20张近距离人像照片，至少4张中距离躯干照片，至少4张长距离的全身照，至少1张坐姿照片。

质量较高(细节多)的训练图片（裁剪为512x512）。

如果需要使你的  Dreambooth 模型更加多样化，尽量使用不同的环境、灯光、发型、表情、姿势、角度和与主体的距离。

避免在你的渲染图中出现手粘在头上的情况，请删除所有手太靠近或接触头部的图片。

如果需要避免渲染图中出现鱼眼镜头效果，可以删除所有自拍图片。

为了避免在渲染中出现白色边框，请确保你的图像中没有裁剪边框。

为了避免不自然的过度模糊，确保图像不包含假的重景深或虚化。

- 一致的标识

经过大量的测试，有人发现 类标识符`class identifier picker` 明显有助于提高概念的嵌入和渲染的质量。我使用类标识符来唤起任何训练过的概念词。

如果训练的是一个 **现实** 的概念，class prompt 应该是“一张高清照片_____”


我们最初只有一个 `class identifier picker` 类图像选择器 (一个词)，它也用于生成 `class images` 。现在已经添加了一个单独的 `class image generator picker` (使用多个单词)，它允许我们进一步确定和约束我们想从类图像中获得的内容。

这非常有用，让我们能够更灵活地找到哪些类图像能够改善概念嵌入到潜在空间中。

- 从检查点恢复训练

参数的 MODEL_NAME 改成上一次模型的位置。

## 参数分析

基本上所有的解释和例子都在上面给出的Colab笔记本中。

[使用 Dreambooth 训练稳定扩散的实验的分析](https://wandb.ai/psuraj/dreambooth/reports/Dreambooth-training-analysis--VmlldzoyNzk0NDc3)

### Subject images / Class images

介绍来自 [2^]

Subject images (或者你在笔记本上看到的实例图像)是你想要训练的图像，所以如果你想要自己的外观的模型，你可以取20到40张自己的图像并输入这些图像。实例名是一个唯一的标识符，它将在提示符中表示受训对象，个人使用 “namelastname”，大多数笔记本使用“sks”，但最好更改它。

你实际上是在告诉AI把你介绍到大数据库中，为了做到这一点，你选择一个类别，即最适合你所训练的类别，对于人们来说，通常使用 "person", "man"/"woman" 等。

在训练中使用Class images是为了防止物体的特征 “渗透” 到同一Class 的其他物体。如果没有 Class images 作为参考点，人工智能倾向于将你的脸与 Class 中出现的其他脸合并。其他像名人一样的人会有点像你。


- concepts_list.json

```
# You can also add multiple concepts here. Try tweaking `--max_train_steps` accordingly.

concepts_list = [
    {
        "instance_prompt":      "photo of zwx dog",
        "class_prompt":         "photo of a dog",
        "instance_data_dir":    "/content/data/zwx",
        "class_data_dir":       "/content/data/dog"
    },
#     {
#         "instance_prompt":      "photo of ukj person",
#         "class_prompt":         "photo of a person",
#         "instance_data_dir":    "/content/data/ukj",
#         "class_data_dir":       "/content/data/person"
#     }
]

# `class_data_dir` contains regularization images
```


## 其他

[fast-stable-diffusion colabs](https://github.com/TheLastBen/fast-stable-diffusion)

相关的还有 [DreamBooth_Stable_Diffusion](https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb#scrollTo=K6xoHWSsbcS3) 和
[fast-DreamBooth](https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb)


[Dreambooth Gui](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2927)

[ShivamShrirao/diffusers](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

<!--
[飞桨dreambooth训练教程](https://docs.qq.com/doc/DUHVuZ3BNV0FkT1R6)
-->

https://github.com/XavierXiao/Dreambooth-Stable-Diffusion


[1^]:[testing_dreambooth_for_consistency_with_complex](https://www.reddit.com/r/StableDiffusion/comments/yhw7k8/testing_dreambooth_for_consistency_with_complex/)

[2^]:[good_dreambooth_formula](https://www.reddit.com/r/StableDiffusion/comments/ybxv7h/good_dreambooth_formula/)
