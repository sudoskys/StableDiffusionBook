# DreamBooth

>TODO/HELP 此页面可能含有较多不实内容

DreamBooth 的模型是一种新的文本到图像“个性化”（可适应用户特定的图像生成需求）扩散模型方法。

使用时，只需将模型导出为ckpt，然后您可以将其加载到您想要的UI 中。

本节使用  Shivam Shirao 的 [版本](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

配置衍生自 [ShivamShrirao/diffusers](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)


## 准备

Windows 系统至少需要16, Linux 系统要求显存大于 8GB

然后在下面选一个训练方法。

### WebUi 扩展

https://github.com/d8ahazard/sd_dreambooth_extension

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

**进入 Python 并测试调用**

```
    python
    >>>import bitsandbytes
```
如果没有报错证明安装成功（ `Ctrl + Z` 并回车退出 python）

#### 临时指南

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

启用 prior_preservation 以开始 DreamBooth 训练. prior_loss_weight 越低则越难过拟合, 但是也越难学到东西.

[3^] [4^]

### 数据集

数据集的创建是在 Dreambooth训练 中获得良好、稳定结果的最重要部分。

- 内容要求

一定要使用高质量的样本，运动模糊或低分辨率等内容会被训练到模型里，影响作品质量。

当为一个特定的风格进行训练时，挑选具有良好一致性的样本。理想情况下，只挑选你要训练的艺术家的图像。避免粉丝艺术或任何具有不同风格的东西，除非你的目标是像风格融合。

对于主题，黑色或白色背景的样本有极大的帮助。

>透明的背景也可以，但有时会在主体周围留下白色轮廓，所以目前我不建议使用透明背景。

如果需要使你的  Dreambooth 模型更加多样化，尽量使用不同的环境、灯光、发型、表情、姿势、角度和与主体的距离。

请确保包括有正常背景的图片（例如，对象在一个场景中的图片）。只使用带简单背景的图片，效果会比较差。

避免在你的渲染图中出现手粘在头上的情况，请删除所有手太靠近或接触头部的图片。

如果需要避免渲染图中出现鱼眼镜头效果，可以删除所有自拍图片。

为了避免不自然的过度模糊，确保图像不包含假的重景深或虚化。

- 调节

一旦你收集了数据集的照片，将所有图片裁剪并调整为512x512的正方形，并删除任何水印、商标、被图片边缘切断的人/肢体，或其他你不希望被训练的内容。以PNG格式保存图像，并将所有图像放在 train 文件夹中。

- 从检查点恢复训练

参数的 MODEL_NAME 改成上一次模型的位置。


### 基本参数

基本上所有的解释和例子都在上面给出的Colab笔记本中。

[使用 Dreambooth 训练稳定扩散的实验的分析](https://wandb.ai/psuraj/dreambooth/reports/Dreambooth-training-analysis--VmlldzoyNzk0NDc3)

* Instance Image 

你所训练的目标
    
* Instance Prompt 

默认实现为全局共享一个 prompt, 这对于 few shot 是可能有效的, 即 DreamBooth (original paper method) 但当你的训练目标增多之后则不适用, 可以开启 combine_prompt_from_txt 选项, 为每个 instance 准备一个 prompt (通常为 txt) 即为 DreamBooth (alternative method). Instance Prompt 之中应该包含一个唯一标识符 [V]. 见下方 Token 一节.
    
* Class/Regularization 

Image 应该为 自动生成 即 auto-generated 的图像, 用于检测 AI 的先验知识. 不应该放任何非 AI 生成的图像. 如果你确定这么做为什么不去使用 Native Training 呢? (掺杂同风格图在 clas image 属于早期探索的弯路, 目前已经不再鼓励)

* Class Prompt 

随意, 反正是自动生成出来的, 建议从其他支持 CLIP SKIP 2 的推理前端单独生成好之后丢到 class img 集内, 同样可以从独立的 txt 中读取内容.

* learning_rate 学习率

DreamBooth 本身具有十分强烈的 copy and paste 效果. 使用 class/regularization 可以适当压制该效果.

### Native Training

Native Training 为原生训练, 与 DreamBooth 不同的是, Native Training 会直接使用你的训练集进行训练, 不再需要 Class Image.

关闭 `prior_preservation` 选项以开始以原生方式进行训练, 是训练画风的推荐方式 在此训练中没有 Instance/Class Image 之分, 所有的图都会被用于训练.., 但是你需要为每个图准备一个 Instance Prompt, 就像传统的 hypernetwork 一样同文件名称, 通常为 txt.

Native Training 需要较多的数据集, 但这个量众说纷纭, 大约在 [100, 10000] 这个区间, 多多益善 (但仍然建议人工挑选)

### Tip

#### Token and Tagging

| What your training set is about | Instance prompt must contain | Class prompt should describe                   |
| --------------------------------- | ------------------------------ | ------------------------------------------------ |
| A object/person                 | `[V]`                    | The object's type and/or characteristics       |
| A artist's style                | `by [V]`                 | The common characteristics of the training set |

假设你想训练的人物叫做 `[N]`, 你不应该直接使用 `[N]` 作为代表特征词, 而是应该使用在[该词汇表](https://huggingface.co/openai/clip-vit-large-patch14/raw/main/vocab.json) 中存在但是没有对应概念或者说对应概念不明显的词 `[V]`. 具体可在 [NovelAI Tokenizer](https://novelai.net/tokenizer) 验证其 token 长度以辅助确定.

> 注: 原论文中使用的示例词 `sks` 和现实中的枪械 [SKS](https://en.wikipedia.org/wiki/SKS) 相同, 属于不适合被使用的词汇. 但是如果你的训练程度足够高的话说不定可以 override 其影响.

推荐使用 [crosstyan/blip_helper](https://github.com/crosstyan/blip_helper) 去给你的图像打标. 或者使用 [DeepDanbooru](https://github.com/KichangKim/DeepDanbooru) 和 [BLIP](https://github.com/salesforce/BLIP)


#### Augmentation

* 处理数据的方式有许多: 最常见的有反转, 旋转, 亮度和裁切.

玄学打碎, 或者对背景/大头等单独裁切, 也许会有帮助


#### Aspect Ratio Bucketing

`aspect_ratio_bucket` 调成 `enable: true`. 见 [Aspect Ratio Bucketing](https://github.com/NovelAI/novelai-aspect-ratio-bucketing)

Aspect Ratio Bucketing 简称 ARB, 原版训练均只能使用 `1:1` 的图像, 开启 ARB 使得训练非 `1:1` 的图像成为可能, 但并非任意比例尺的图像.


```
[[ 256 1024], [ 320 1024], [ 384 1024], [ 384  960], [ 384  896], [ 448  832], [ 512  768], [ 512  704], [ 512  512], [ 576  640], [ 640  576], [ 704  512], [ 768  512], [ 832  448], [ 896  384], [ 960  384], [1024  384], [1024  320], [1024  256]]
```

不在 bucket 内的图像将会被裁切.

ARB 与 DreamBooth 一起使用的相性不好, 仅推荐 Native Training 时使用.


#### Train Text Encoder

我愿称之为玩坏 Text Encoder, 不推荐使用.

有玄学说法是在达到训练的某个百分比/epoch/step 之后应该关闭以防止过度玩坏.

* 你一开始写的 instance prompt 要长一些, 概括你的训练目标 (但是又不要太长, 不要覆盖你常用的词) (像是 girl 我会换成 woman, 1boy 换成 male)
* 第一, text prompt 读进去是寄. 因为词数太多了影响分散, 效果不明显.
* 第二, instance prompt 不能只填一个 `[V]` 否则那个词也废掉了.
* 试着大火爆炒

炼出来调用的话看情况加你训练的 instance prompt 的词, 看你想要多少味道.

或许训练人物的时候也是效果拔群.


#### Multiple Concept

用 DreamBooth 是可以训练多个概念/人物/动作/物体的. 但是若训练两个人物则推理时不能使其同时出现, 两者的特征会被混合起来.

如果用其他版本的 DreamBooth 训练方法检查 `--concept_list` 参数, 可以读入一个类似的 `json` 文件.

训练步数的选择一般来说是，训练步骤 =（参考图像 x 100）

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

### 解释 Subject images / Class images

介绍来自 [2^]

Subject images (或者你在笔记本上看到的实例图像)是你想要训练的图像，所以如果你想要自己的外观的模型，你可以取20到40张自己的图像并输入这些图像。实例名是一个唯一的标识符，它将在提示符中表示受训对象，个人使用 “namelastname”，大多数笔记本使用“sks”，但最好更改它。

你实际上是在告诉AI把你介绍到大数据库中，为了做到这一点，你选择一个类别，即最适合你所训练的类别，对于人们来说，通常使用 "person", "man"/"woman" 等。

在训练中使用Class images是为了防止物体的特征 “渗透” 到同一Class 的其他物体。如果没有 Class images 作为参考点，人工智能倾向于将你的脸与 Class 中出现的其他脸合并。其他像名人一样的人会有点像你。


## 其他

[fast-stable-diffusion colabs](https://github.com/TheLastBen/fast-stable-diffusion)

[DreamBooth_Stable_Diffusion](https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb#scrollTo=K6xoHWSsbcS3) 

[fast-DreamBooth](https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb)

[Dreambooth Gui](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2927)


<!--
[飞桨dreambooth训练教程](https://docs.qq.com/doc/DUHVuZ3BNV0FkT1R6)
-->

https://github.com/XavierXiao/Dreambooth-Stable-Diffusion


[1^]:[testing_dreambooth_for_consistency_with_complex](https://www.reddit.com/r/StableDiffusion/comments/yhw7k8/testing_dreambooth_for_consistency_with_complex/)

[2^]:[good_dreambooth_formula](https://www.reddit.com/r/StableDiffusion/comments/ybxv7h/good_dreambooth_formula/)


[3^]:[dreambooth-training-guide](https://github.com/nitrosocke/dreambooth-training-guide)

[4^]:[crosstyan-s-guide](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)