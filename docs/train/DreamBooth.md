 


## DreamBooth

DreamBooth 的模型是一种新的文本到图像“个性化”（可适应用户特定的图像生成需求）扩散模型方法。

### 配置要求

Windows 系统至少需要 16, Linux 系统要求显存大于 8 GB

### 训练

#### Linux

直接用下面的笔记本里的代码，不过需要英文基础。

[Nyanko Lepsoni 的 Colab 笔记本](https://colab.research.google.com/drive/17yM4mlPVOFdJE_81oWBz5mXH9cxvhmz8)

[RcINS 的 Colab 笔记本](https://colab.research.google.com/drive/1C1vVZ59S4kWfL7jIsczyLpmxbD4cOA-k)

以上笔记本来自 [社区置顶](https://t.me/StableDiffusion_CN/196744)

[适用于Autodl的项目](https://github.com/crosstyan/dreambooth-scripts-for-autodl)

#### Windows

来自 [^18]

Windows上的Dreambooth 可以采用ShivamShrirao的优化版本来节省显存，

[diffusers/examples/dreambooth at main · ShivamShrirao/diffusers · GitHub](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

但是，由于相关链接库的原因，用于在Linux上的部署方法无法直接在windows上使用，由于同样的原因，该优化版本在colab上最低9.9G的显存需求在windows上应该稍高，因此推荐至少使用显存12G的显卡。笔者仅在16G显存PC上测试成功，12G理论可以，待测试。

修改或覆盖原始库中的文件前请备份！

准备环境 `Git`，`Python`，`MiniConda` (或 `MiniConda` )。

以下步骤在python3.8，windows10 22H2中操作，其他环境未测试

**创建工作目录 ，在目录下构建python3.8的venv虚拟环境**

    python -m venv --system-site-packages venv_dbwin
    venv_dbwin\Scripts\activate
    python.exe -m pip install --upgrade pip

克隆ShivamShrirao的优化版本dreambooth到工作目录中并安装相关依赖（使用的构建版本https://github.com/huggingface/diffusers/tree/7465397f33d5de75dcccc155e3fb9a232fcbb0a0 后续版本可能无法支持本文方法）

    git clone https://github.com/ShivamShrirao/diffusers
    cd diffusers
    pip install -e .
    cd examples\dreambooth
    pip install -U -r requirements.txt
    pip install OmegaConf
    pip install pytorch_lightning
    pip install einops
    pip install bitsandbytes==0.34

完成后，在windows上需要根据 https://github.com/TimDettmers/bitsandbytes/issues/30#issuecomment-1257676341 中的方法实现bitsandbytes支持

将 https://github.com/DeXtmL/bitsandbytes-win-prebuilt 中的libbitsandbytes_cuda116.dll 文件手动拷贝到工作目录下的 venv_diffusers\Lib\site-packages\bitsandbytes 中，位于 libbitsandbytes_cuda116.so 的旁边；

然后更改脚本以应用，可以手动修改，为方便也可以下载以下文件替换：

将 cextension.py: https://pastebin.com/jjgxuh8V 覆盖到venv_diffusers\Lib\site-packages\bitsandbytes目录。

将 main.py: https://pastebin.com/BsEzpdpw 覆盖到venv_diffusers\Lib\site-packages\bitsandbytes\cuda_setup目录。

**安装PyTorch和Torchvision**

    pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116

**进入python并测试调用：**

    python
    >>>import bitsandbytes

如果没有报错证明安装成功（Ctrl + Z并回车退出python）

**设置accelerate**

    accelerate config

    In which compute environment are you running? ([0] This machine, [1] AWS (Amazon SageMaker)): 0
    Which type of machine are you using? ([0] No distributed training, [1] multi-CPU, [2] multi-GPU, [3] TPU [4] MPS): 0
    Do you want to run your training on CPU only (even if a GPU is available)? [yes/NO]:NO
    Do you want to use DeepSpeed? [yes/NO]:NO
    Do you wish to use FP16 or BF16 (mixed precision)? [NO/fp16/bf16]: fp16

按照上面的设置选项，（0，0，NO，NO，fp16）

**修改显存优化**

因为无法使用xformers，所以使用https://github.com/lucidrains/memory-efficient-attention-pytorch/blob/main/memory_efficient_attention_pytorch/flash_attention.py
中的优化方法作为代替：

将 attention.py: https://pastebin.com/nmwTrGB9 覆盖到diffusers\src\diffusers\models目录:

至此环境配置完成，可以开始训练了。具体使用参数请查阅ShivamShrirao的readme文件和notebook文件

**Tips：**

我使用以下参数训练，

    accelerate launch --num_cpu_threads_per_process 8 diffusers/examples/dreambooth/train_dreambooth.py --pretrained_model_name_or_path=models/diffusers_model --pretrained_vae_name_or_path=models/diffusers_model/vae --output_dir=models --concepts_list="concepts_list.json" --with_prior_preservation --prior_loss_weight=1.0 --seed=1337 --resolution=512 --mixed_precision="fp16" --lr_scheduler="constant" --use_8bit_adam --gradient_accumulation_steps=1 --train_batch_size=1 --max_train_steps=800 --save_interval=10000 --learning_rate=1e-6 --num_class_images=100 --lr_warmup_steps=0 --gradient_checkpointing

![image](https://user-images.githubusercontent.com/44570237/198906326-21b4f779-f870-4012-84c1-d5ac1dae0411.png)

峰值显存占用正好为12G，如果您只有一个显卡，显存不高于12G，而且同时需要用于windows系统显示，请关闭所有占用显存的程序或网页等，减少额外显存消耗再进行训练；

这是一个临时的解决方案，期待windows官方适配的到来

diffusers不能直接使用ckpt文件进行训练，需要先进行转换，示例：

    python diffusers\scripts\convert_original_stable_diffusion_to_diffusers.py  --checkpoint_path model.ckpt  --original_config_file v1-inference.yaml  --scheduler_type ddim  --dump_path models/diffusers_model

训练完成同样要进行打包转换为ckpt,即可用于AUTOMATIC1111的WebUI中：

    python diffusers\scripts\convert_diffusers_to_original_stable_diffusion.py  --model_path models/resultModel  --checkpoint_path result.ckpt  --half



### 参数分析

[使用 Dreambooth 训练稳定扩散的实验的分析](https://wandb.ai/psuraj/dreambooth/reports/Dreambooth-training-analysis--VmlldzoyNzk0NDc3)

### 其他

[fast-stable-diffusion colabs](https://github.com/TheLastBen/fast-stable-diffusion)

相关的还有 [DreamBooth_Stable_Diffusion](https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb#scrollTo=K6xoHWSsbcS3) 和
[fast-DreamBooth](https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb)


[Dreambooth Gui](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2927)

[ShivamShrirao/diffusers](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

<!--
[飞桨dreambooth训练教程](https://docs.qq.com/doc/DUHVuZ3BNV0FkT1R6)
-->

https://github.com/XavierXiao/Dreambooth-Stable-Diffusion
