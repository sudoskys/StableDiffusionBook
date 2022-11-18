# DreamBooth

>TODO/HELP This page may contain inaccurate content

DreamBooth's model is a new approach to text-to-image "personalisation" (which can be adapted to the user's specific image generation needs) of diffusion models.

To use it, simply export the model as a ckpt and you can then load it into the UI you want.

This section uses Shivam Shirao's [version](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)

## Configuration requirements

Windows systems require at least 16, Linux systems require more than 8 GB of VRAM

## Training

### WebUi extension

https://github.com/d8ahazard/sd_dreambooth_extension

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

## Simple Guide

Enable prior_preservation to start DreamBooth training. The lower the prior_loss_weight, the harder it is to fit, but also the harder it is to learn.

[3^] [4^]

### Datasets

The creation of the dataset is the most important part of getting good, stable results in Dreambooth training.

- Content requirements

Always use high quality samples, content such as motion blur or low resolution can be trained into the model and affect the quality of the work.

When training for a specific style, pick samples that have good consistency. Ideally, only pick images of the artist you are training. Avoid fan art or anything with a different style, unless your goal is something like stylistic fusion.

For themes, samples with black or white backgrounds are extremely helpful.

>Transparent backgrounds are fine, but sometimes they leave a white outline around the subject, so I don't recommend using transparent backgrounds at the moment.

If you need to make your Dreambooth model more diverse, try to use different environments, lighting, hairstyles, expressions, poses, angles and distances from the subject.

Be sure to include images with normal backgrounds (e.g. of the object in a scene). Using only images with simple backgrounds will be less effective.

Avoid having hands stuck to the head in your renders by removing all images where the hands are too close to or touching the head.

If you need to avoid a fisheye lens effect in your renders, remove all selfie images.

To avoid unnatural over-blurring, make sure the image does not contain a false heavy depth of field or bokeh.


- Adjustments

Once you have collected the photos for your dataset, crop and resize all images to a 512x512 square and remove any watermarks, logos, people/limb cut off by the edges of the image, or anything else you don't want to be trained on.

Save the images in PNG format and place all images in the train folder.


- Resuming training from a checkpoint

The MODEL_NAME of the parameter is changed to the position of the last model.


### Basic parameters

Basically all explanations and examples are in the Colab notebook given above.

[Analysis of experiments using Dreambooth to train stable diffusion](https://wandb.ai/psuraj/dreambooth/reports/Dreambooth-training-analysis--VmlldzoyNzk0NDc3)

* Instance Image 

The target you are training on
    
* Instance Prompt 

The default implementation is to share a prompt globally, which may work for a few shots, i.e. DreamBooth (original paper method) but not when you have more targets to train, you can turn on the combine_prompt_from_txt option to have a prompt (usually txt) for each instance. The prompt (usually txt) is DreamBooth (alternative method). The Instance Prompt should contain a unique identifier [V]. See the Token section below.
    
* Class/Regularization 

The Image should be an auto-generated image, used to detect the AI's prior knowledge. No non-AI generated images should be placed. If you are sure about this why not use Native Training? (Adulteration of homogeneous images in clas images was an early discovery detour and is no longer encouraged)

* Class Prompt 

Feel free, it's automatically generated anyway, it's recommended to generate it separately from other inference front-ends that support CLIP SKIP 2 and drop it into the class img set, again reading from a separate txt.

* learning_rate

learning_rate

DreamBooth itself has a very strong copy and paste effect. Use class/regularization to suppress this effect.

### Native Training

Unlike DreamBooth, Native Training uses your training set for training directly and does not require a class image.

Turn off the `prior_preservation` option to start training natively, which is the recommended way to train a drawing style. However, you will need to prepare an Instance Prompt for each graph, just like a traditional hypernetwork with the same file name, usually txt.

Native Training requires a large dataset, but the amount varies, in the range [100, 10000], more is better (but hand-picking is still recommended)

### Tip

#### Token and Tagging

| What your training set is about | Instance prompt must contain | Class prompt should describe |
| --------------------------------- | ------------------------------ | ------------------------------------------------ |
| A object/person | `[V]` | The object's type and/or characteristics |
| A artist's style | `by [V]` | The common characteristics of the training set |

Suppose the character you want to train is called `[N]`, you should not use `[N]` directly as a representative feature word, but rather use a concept that exists in [the glossary](https://huggingface.co/openai/clip-vit-large-patch14/raw/main/vocab.json) but Instead, you should use the word `[V]`, which is present in [the glossary]() but has no equivalent or is not obvious. The token length can be verified at [NovelAI Tokenizer](https://novelai.net/tokenizer) to help determine this.

> Note: The example word `sks` used in the original paper is the same as the real firearm [SKS](https://en.wikipedia.org/wiki/SKS), which is not a suitable word to be used. However, if you are trained enough you may be able to override its influence.

We recommend using [crosstyan/blip_helper](https://github.com/crosstyan/blip_helper) to label your images. Or use [DeepDanbooru](https://github.com/KichangKim/DeepDanbooru) and [BLIP](https://github.com/salesforce/BLIP)



#### Augmentation

* There are many ways to manipulate data: the most common are inversion, rotation, brightness and cropping.

Metaphysical fragmentation, or separate cropping of backgrounds/heads etc., may help


#### Aspect Ratio Bucketing

`aspect_ratio_bucket` is set to `enable: true`. See [Aspect Ratio Bucketing](https://github.com/NovelAI/novelai-aspect-ratio-bucketing).

ARB for short, the original version can only use `1:1` images, turning on ARB makes it possible to train non-`1:1` images, but not at any scale.

```
[[256 1024], [320 1024], [384 1024], [384 960], [384 896], [448 832], [512 768], [512 704], [512 512], [576 640], [640 576], [704 512], [768 512], [832 448], [ 896 384], [ 960 384], [1024 384], [1024 320], [1024 256]]
```

Images that are not in the bucket will be cropped.

ARB does not phase well with DreamBooth and is only recommended for Native Training.


#### Train Text Encoder

I would call this a bad Text Encoder, and do not recommend its use.

There is a metaphysical argument that it should be turned off after a certain percentage/epoch/step of training to prevent overplaying.

* Your initial instance prompt should be long, outlining your training goal (but not too long, not covering your usual words) (e.g. girl I would replace with woman, 1boy with male)
* Firstly, the text prompt should be read in as a message. It's not effective because too many words are distracting.
* Secondly, the instance prompt cannot be filled in with just one `[V]` or that word will be lost too.
* Try frying at high heat

If you've refined the call, add the word you've trained the instance prompt with, depending on how much flavour you want.

It might also work well for character training.


#### Multiple Concept

With DreamBooth it is possible to train multiple concepts/characters/actions/objects. However, if you train two characters, you can't reason about them at the same time, the features of both will be mixed up.


If you check the `--concept_list` parameter with other versions of the DreamBooth training method, you can read in a similar `json` file.


- Choosing the number of training steps

In general, the number of training steps = (reference image x 100)

- concepts_list.json

```
# You can also add multiple concepts here. try tweaking `--max_train_steps` accordingly.

concepts_list = [
    {
        "instance_prompt": "photo of zwx dog",
        "class_prompt": "photo of a dog",
        "instance_data_dir":"/content/data/zwx",
        "class_data_dir": "/content/data/dog"
    },
# {
# "instance_prompt": "photo of ukj person",
# "class_prompt": "photo of a person",
# "instance_data_dir":"/content/data/ukj",
# "class_data_dir":"/content/data/person"
# }
]

# ``class_data_dir`` contains regularization images
```

### Explanation of Subject images / Class images

Introduction from [2^]

Subject images (or instance images as you see them in your notebook) are the images you want to train on, so if you want your own looking model, you can take 20 to 40 of your own images and input these. The instance name is a unique identifier that will indicate the trained object in the prompt, personally I use "namelastname", most notebooks use "sks" but it is best to change it.

You are in effect telling the AI to introduce you to the large database, to do this you choose a class, i.e. the one that best fits the class you are training, for people it is common to use "person", "man"/"woman" etc.

The purpose of using Class images in training is to prevent the features of the object from "bleeding over" to other objects in the same Class. Without Class images as a reference point, the AI tends to merge your face with other faces that appear in the Class. Other people who look like celebrities will somewhat resemble you.


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

[2^]:[good_dreambooth_formula](https://www.reddit.com/r/StableDiffusion/comments/ybxv7h/good_dreambooth_formula/)


[3^]:[dreambooth-training-guide](https://github.com/nitrosocke/dreambooth-training-guide)

[4^]:[crosstyan-s-guide](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

