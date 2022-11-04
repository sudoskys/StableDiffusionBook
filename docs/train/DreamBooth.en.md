# DreamBooth

DreamBooth's model is a new approach to text-to-image "personalisation" (which can be adapted to the user's specific image generation needs) of diffusion models.

## Configuration requirements

Windows systems require at least 16, Linux systems require more than 8 GB of VRAM

## Training

### Linux

Use the code directly from the Colab notebook below.


[Nyanko Lepsoni s Colab notebook](https://colab.research.google.com/drive/17yM4mlPVOFdJE_81oWBz5mXH9cxvhmz8)

[RcINS s Colab notebook](https://colab.research.google.com/drive/1C1vVZ59S4kWfL7jIsczyLpmxbD4cOA-k)


The above notebook is from [Community Top](https://t.me/StableDiffusion_CN/196744)

[Project for Autodl](https://github.com/crosstyan/dreambooth-scripts-for-autodl)



### Windows

From [^18]

`Dreambooth` on Windows can use an optimized version of ShivamShrirao to save VRAM.

[diffusers/examples/dreambooth at main - ShivamShrirao/diffusers - GitHub](https://github.com/ShivamShrirao/diffusers/tree/main/examples /dreambooth)

However, the deployment method used on `Linux` will not work directly on windows due to the associated link libraries, and for the same reason the minimum 9.9G VRAM requirement on colab for this optimized version should be slightly higher on windows, so it is recommended to **use a video card with at least 12G of VRAM**.

>I have only tested it successfully on a 16G memory PC, 12G is theoretically possible, to be tested.

Please back up before modifying or overwriting files in the original library!

Prepare the environment `Git`, `Python`, `MiniConda` (or `MiniConda` ).

The following steps work in python 3.8, windows 10 22H2, other environments have not been tested

**Create a working directory in which to build the venv virtual environment for python 3.8**

```bash
    python -m venv --system-site-packages venv_dbwin
    venv_dbwin\Scripts\activate
    python.exe -m pip install --upgrade pip
```


Clone the optimized version of ShivamShrirao's dreambooth into a working directory and install the relevant dependencies ,using the [build-version](https://github.com/huggingface/diffusers/tree/7465397f33d5de75dcccc155e3fb9a232fcbb0a0), subsequent versions may not support this method.

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


Once this is done, the bitsandbytes support needs to be implemented on windows according to the method in [this Issue](https://github.com/TimDettmers/bitsandbytes/issues/30#issuecomment-1257676341)

Manually copy the `libbitsandbytes_cuda116.dll` file from [this Issue](https://github.com/DeXtmL/bitsandbytes-win-prebuilt) to the working directory in `venv_diffusers\Lib\site- packages\bitsandbytes` in the working directory, next to `libbitsandbytes_cuda116.so`.

Then change the script to apply it, either manually or, for convenience, by downloading the following file to replace it.

Overwrite `cextension.py` - https://pastebin.com/jjgxuh8V with the `venv_diffusers\Lib\site-packages\bitsandbytes` directory.

Overwrite `main.py` - https://pastebin.com/BsEzpdpw to the `venv_diffusers\Lib\site-packages\bitsandbytes\cuda_setup` directory.

**install PyTorch and Torchvision**

`pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116`

**Go to Python and test the call:**
```
    python
    >>>import bitsandbytes
```
如果没有报错证明安装成功（ `Ctrl + Z` 并回车退出 python）

**set accelerate**

```
    accelerate config

    In which compute environment are you running? ([0] This machine, [1] AWS (Amazon SageMaker)): 0
    Which type of machine are you using? ([0] No distributed training, [1] multi-CPU, [2] multi-GPU, [3] TPU [4] MPS): 0
    Do you want to run your training on CPU only (even if a GPU is available)? [yes/NO]:NO
    Do you want to use DeepSpeed? [yes/NO]:NO
    Do you wish to use FP16 or BF16 (mixed precision)? [NO/fp16/bf16]: fp16
```

按照上面的设置选项，也就是（0，0，NO，NO，fp16）


Follow the setting options above, which are (0, 0, NO, NO, fp16)

**Modify VRAM optimisation**

Since xformers are not available, use [this file](https://github.com/lucidrains/memory-efficient-attention-pytorch/blob/main/memory_efficient_attention_pytorch/flash_attention.py) as an optimization method instead.

Overwrite attention.py - https://pastebin.com/nmwTrGB9 with the `diffusers\src\diffusers\models` directory.

This completes the environment configuration and you are ready to start training. Please refer to ShivamShrirao's readme file and notebook file for specific parameters to use

!!! info 
    I trained using the following parameters.

    ```bash
        accelerate launch --num_cpu_threads_per_process 8 diffusers/examples/dreambooth/train_dreambooth.py --pretrained_model_name_or_path=models/diffusers_model --pretrained_vae_name_or_path=models/diffusers_model/vae --output_dir=models --concepts_list="concepts_list.json" --with_prior_preservation --prior_loss_weight=1.0 --seed=1337 --resolution=512 --mixed_precision="fp16" --lr_scheduler="constant" --use_8bit_adam --gradient_accumulation_steps=1 --train_batch_size=1 --max_train_steps=800 --save_interval=10000 --learning_rate=1e-6 --num_class_images=100 --lr_warmup_steps=0 --gradient_checkpointing
    ```

![image](https://user-images.githubusercontent.com/44570237/198906326-21b4f779-f870-4012-84c1-d5ac1dae0411.png)

If you only have one graphics card with no more than 12G of VRAM and need to use it for Windows, please close all programs or web pages that consume VRAM to reduce additional memory consumption before training.

diffusers cannot be trained directly using ckpt files, they need to be converted first, as shown in the example below.

```bash
    python diffusers\scripts\convert_original_stable_diffusion_to_diffusers.py --checkpoint_path model.ckpt --original_config_file v1- inference.yaml --scheduler_type ddim --dump_path models/diffusers_model
```

The training is also packaged and converted to ckpt, which can be used in the WebUI of the AUTOMATIC1111.

```
    python diffusers\scripts\convert_diffusers_to_original_stable_diffusion.py --model_path models/resultModel --checkpoint_path result. ckpt --half
```


>This is a temporary solution in anticipation of an official Windows adaptation.

## Simple guide

[1^]

>The following are all unconfirmed. If you have a different opinion, please feel free to ask.

- Preparing the training image

For your training images you will need:

At least 20 close up portraits, at least 4 medium distance torso shots, at least 4 long distance full body shots and at least 1 seated photo.

Higher quality (more detail) training images (cropped to 512x512).

If you need to make your Dreambooth model more diverse, try to use different environments, lighting, hairstyles, expressions, poses, angles and distances from the subject.

Avoid having hands stuck to the head in your renders by removing all images where hands are too close to or touching the head.

If you need to avoid a fisheye lens effect in your renders, remove all selfie images.

To avoid white borders in your renders, make sure there are no cropped borders in your images.

To avoid unnatural over-blurring, make sure the image does not contain a false heavy depth of field or vignetting.

- Consistent markings

After extensive testing, it has been found that the class identifier `class identifier picker` clearly helps to improve the quality of embedding and rendering of concepts. I use the class identifier to evoke any trained concept word.

If training a **reality** concept, the class prompt should be "a high resolution photo _____"


We initially only had a `class identifier picker` class image picker (one word) which was also used to generate `class images`. A separate `class image generator picker` has now been added (using multiple words) which allows us to further identify and constrain what we want from the class images.

This is very useful and gives us more flexibility in finding which class images improve the embedding of concepts into the latent space.


## Parametric analysis

[Analysis of experiments using Dreambooth to train stable diffusion](https://wandb.ai/psuraj/dreambooth/reports/Dreambooth-training-analysis--VmlldzoyNzk0NDc3)



## Other

[fast-stable-diffusion colabs](https://github.com/TheLastBen/fast-stable-diffusion)

Related [DreamBooth_Stable_Diffusion](https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb#scrollTo=K6xoHWSsbcS3) and
[fast-DreamBooth](https://colab.research.google.com/github/TheLastBen/fast-stable-diffusion/blob/main/fast-DreamBooth.ipynb)


[Dreambooth Gui](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2927)

[ShivamShrirao/diffusers](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

<!--
[飞桨dreambooth训练教程](https://docs.qq.com/doc/DUHVuZ3BNV0FkT1R6)
-->

https://github.com/XavierXiao/Dreambooth-Stable-Diffusion

[1^]:[testing_dreambooth_for_consistency_with_complex](https://www.reddit.com/r/StableDiffusion/comments/yhw7k8/testing_dreambooth_for_consistency_with_complex/)