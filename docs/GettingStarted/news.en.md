# News

!!! tip

    The Stable Diffusion WebUi is currently iterating very quickly and there are a lot of updates every day, so it is recommended to pull the latest code every day.

## 12/5

Animate Anyone: Consistent and Controllable Image-to-Video Synthesis for Character Animation

https://humanaigc.github.io/animate-anyone/

## 11/?

LCM-LoRA generates high quality images with minimal inference steps and fast image generation.

Paper https://arxiv.org/abs/2311.05556

Intro Video https://www.bilibili.com/video/BV1Q94y1E7uc/

## 11/?

Novel AI V3 model released, outperforms V2 and any other model in all aspects. Human body is accurately captured.
Training data not disclosed, suspected to use copyrighted data/paint sets, twitter watermark clearly visible.

https://twitter.com/novelaiofficial

## 2/27

LoRA for convolution network

https://github.com/KohakuBlueleaf/LoCon

Using ControlNet right in Blender.

https://github.com/coolzilj/Blender-ControlNet

## 2/13

ControlNet is a neural network structure to control diffusion models by adding extra conditions.

https://github.com/Mikubill/sd-webui-controlnet

## 2/6

StableDiffusionWebUI accelerated using TensorRT

https://github.com/ddPn08/Lsmith

## 1/17

Ai 学习 GPU 性能分析图

https://twitter.com/umiyuki_ai/status/1615153720333078528/photo/1

Lora

https://github.com/microsoft/LoRA
https://github.com/CCRcmcpe/scal-sdt/blob/main/modules/lora.py

## 12/1

WebUi:[Newly created embeddings considered malicious](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/3878)

## 11/30

**DreamArtist**
DA updated dynamic cfg to automatically adjust cfg based on time step, supports nonlinearity. Dynamic cfg can speed up
convergence, possibly improve learning quality, and is better for large multi-graph datasets.

## 11/28

**DreamArtist**
DA updated local learning enhancement, can emphasize more on learning a certain part of features now. The augmented
graph is a grayscale graph with image values ranging from 0-255.0-127 means reduced attention from 1-0.01 equal points,
127-255 means enhanced attention from 1-5 equal points.

## 11/24

Stable Diffusion 2.0 Release

https://stability.ai/blog/stable-diffusion-v2-release

DiffusionDet: Diffusion Model for Object Detection

https://arxiv.org/abs/2211.09788

## 11/23

How to train Stable Diffusion (SD) "efficiently" and how to use SCAL-SDT itself (WIP).
[SCAL-SDT](https://github.com/CCRcmcpe/scal-sdt/wiki)

## 11/22

WebUi [Gradient accumulation, autocast fix, new latent sampling method, etc](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4886)

[Nerfgun3/bad_prompt](https://huggingface.co/datasets/Nerfgun3/bad_prompt)
> The idea behind this embedding was to somehow train the negative prompt as an embedding, thus unifying the basis of
> the negative prompt into one word or embedding.

## 11/21

The new [nijijourney](https://nijijourney.com/) starts internal testing.

The official channel is divided into four sub-sections: Japanese, Chinese, 한글, and English.

```
niji journey is developed in collaboration by Spellbrush and Midjourney
```

After preliminary testing, nijijourney has a good composition ability, but the effect is still waiting to be seen.

## 11/18

[Reverse thinking](https://twitter.com/needle/status/1593428231901700101)

## 11/16

Anything-V3.0.ckpt is a hybrid model fused from the ChineseMagicBook group of friends.

There is also a so-called hand repair model, a regenerated DreamBooth model trained from the work of a famous painter.

## 11/15

Telegram Chinese community on NAI Leak's Colab notebook received a DMCA.

https://lumendatabase.org/notices/29484955

https://lumendatabase.org/notices/29484957

> Once again: Aidraw Wiki does not distribute Cpkt files

## 11/12

[Stable-Diffusion-1B-Chinese](https://huggingface.co/IDEA-CCNL/Taiyi-Stable-Diffusion-1B-Chinese-v0.1)

## 11/11

* [Pull#4563](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4563) Add username and password in ngrok

```
Allows specifying the user of ngrok by username and password using the : --ngrok authtoken:username:password
and keep old args for not using username and password only use authtoken : --ngrok authtoken
```

## 11/10

* Add support for SSL/TLS (provide Gradio TLS options)
  https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4320

## 11/9

* 许可问题
  https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4222

## 11/8

* Colossal-AI: A Unified Deep Learning System for Big Model Era

https://github.com/hpcaitech/ColossalAI

## 11/7

* Dreambooth Extension for Stable-Diffusion-WebUI

https://github.com/d8ahazard/sd_dreambooth_extension

* ads_are_starting_to_appear_in_our_foss/

https://www.reddit.com/r/StableDiffusion/comments/ynshup/ads_are_starting_to_appear_in_our_foss/

## 11/6

* A fast dedicated high-order solver

faster and better?

DPM-Solver (and the improved version DPM-Solver++) is a fast dedicated high-order solver for diffusion ODEs with the
convergence order guarantee.

[Add support for the new DPM-Solver++ samplers added to k-diffusion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4304)

[dpm-solver](https://github.com/LuChengTHU/dpm-solver)
=== "Some Model"

    <div class="sample" markdown>

    ![exp](https://user-images.githubusercontent.com/40903705/200149887-935a6f95-0bfa-4f8e-b6b1-0fb0bfe0b39e.jpg){ align=left loading=lazy}

    Test From [Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/4363)

    *used locally trained hypernetwork in generation*
    
    </div>

=== "SD 1.5"

    <div class="sample" markdown>

    ![exp](https://user-images.githubusercontent.com/116967051/200134579-dc31f8d4-abd6-4ef0-9d2c-1582a53ec1b0.png){loading=lazy align=left width=300 data-title="DPM-Solver++" data-description="DPM-Solver++ can achieve great sample quality in only 15 to 20 steps."}

    Test From [Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4304#issuecomment-1304602296)

    DPM-Solver++ can achieve great sample quality in only 15 to 20 steps.

    If you're talking about samplers themselves, yes especially the 2M and 2M Karras. Very detailed at all steps starting from 15 and fastest in it/s.

    paper https://arxiv.org/abs/2211.01095

    </div>

* New Model

eDiffi: Text-to-Image Diffusion Models with Ensemble of Expert Denoisers

https://deepimagination.cc/eDiffi/

* From one of the original DreamBooth authors : Stop using SKS as the initializer word
  https://www.reddit.com/r/StableDiffusion/comments/yju5ks/from_one_of_the_original_dreambooth_authors_stop/

* AI art is popular
  https://www.reddit.com/r/StableDiffusion/comments/yh8j0a/ai_art_is_popular_and_makes_money_confessions_of/

## 11/5

WebUi released [tokenizer plugin](https://github.com/AUTOMATIC1111/stable-diffusion-webui-tokenizer) to see the
splitting of words.

## 11/3

* Found a great and useful tool.

[painting welding](https://github.com/BlinkDL/Hua)

![](https://raw.githubusercontent.com/BlinkDL/Hua/main/Hua-Demo.gif)

* 2

After testing, I found that emoji works really well. There are always surprises for scene effects.

```
masterpiece,best quality,1girl,
,light blue hair,solo,(anime coloring:1.1),
(🌻☀️🌈:1.2),hug Sunflower,

lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet
```

## 11/2

Adding custom backend APIs for Krita plugins and more

https://github.com/Interpause/auto-sd-paint-ext

In [this PR](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4142), WebUi fixes an issue with IMG2IMG not
releasing RAM after processing

## 11/1

disable access to extension stuff for non-local servers

https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/dc7425a56e7a014cbfa3b3d44ad2321e519fe378

Community feedback: Shared links can lead to risks and attackers can access all files on the system.

## 10/30

In [this commit](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/9f4f894d74b57c3d02ebccaa59f9c22fca2b6c90),
WebUi has added `allow skip current image in progress api ` , which skips the Api method for the current process.

In [this merge](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3722), WebUi has added a native progress
Api

## 10/29

[Discussion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3889) It has been suggested that by
disabling hardware accelerated GPU scheduling on Windows settings, WebUi performance has improved by about 10-50%

## 10/27

API content added to wiki

## 10/26

[中文翻译上线](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/dde8c435987dcd071c63d16f247ae832bce0101f)

[法语翻译上线](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3653)

[土耳其翻译上线](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3631)

WebUi
在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/1e428238db4e399b7a06ad5251cb16eef23a014d)
中将 override_settings 添加到 API

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/cb49800c08a9f6619733250401952e5571dc12f8)
后，img2img 使用了手机照片的 EXIF 方向。

WebUi
在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/de096d0ce752c96e45508dcc7b9e84f7dbe10cca)
中添加了权重初始化和更多激活函数。

WebUi
在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/cbb857b675cf0f169b21515c29da492b513cc8c4)
中允许了在 `--medvram` 下创建 embedding 的情况。

`Hint:will send cond model to GPU if lowvram/medvram is active`

## 10/25

[集成 Tag 工具/支持超网络的 Web](https://git.hudaye.work/MiuliKain/Kamiya-OpenUI) 开源项目说他们缺人。

在 [此次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3624) 中，从 `--use-cpu` 中删除了 `BSRGAN`
，将 `SwinIR` 添加到 `--use-cpu` 并修复 MPS 上的 `upscalers` 。

你可以在 [这里](https://upscale.wiki/wiki/Official_Research_Models) 找到 `BSRGAN` ，下载的模型放到 esrgan 文件夹就可以使用。

`#3505` 之后超网络训练不稳定 ，[问题](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/3505)

## 10/24

Colab 称昨天的封号是 [错误的滥用判定](https://github.com/googlecolab/colabtools/issues/3181)

WebUi [移除了图像浏览器](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3537)
，转换为 [插件](https://github.com/yfszzx/stable-diffusion-webui-images-browser)

## 10/23 更新

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/070fda592bf80fb348ffe8e17b7c71cc288db729)
中，WebUi 增加了日本语翻译。

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/be748e8b086bd9834d08bdd9160649a5e7700af7)
中，WebUi 增加了设置锁定启动参数。

```
parser.add_argument("--freeze-settings", action='store_true', help="disable editing settings", default=False)
```

有网友称，最近发布的的 Stable Diffusion 1.5 偏向 `三次元` 一些。

WebUi 增加了俄语翻译，添加了 img2img API。

在 [这次请求](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2067) 中，WebUi 更新了 ESRGAN 架构和模型以支持所有
ESRGAN 模型。

Colab 开始反滥用，[协议](https://research.google.com/colaboratory/faq.html#limitations-and-restrictions)
禁止穿透。但是有人指出 [Colab 并没有计划封禁 SD](https://github.com/googlecolab/colabtools/issues/3147)，确认误封。

Colab 的 [协议](https://research.google.com/colaboratory/faq.html?hl=zh-CN) 不允许利用多个帐号绕过访问权限或资源使用情况限制。

有人反映 某度好像会封锁 NAI 的模型。

## 10/22 更新

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/2b91251637078e04472c91a06a8d9c4db9c1dcf0)
中，仓库移除了美学权重。

这项功能已经被转为 [插件](https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients)。

## 10/21 更新

Implementation of Stable Diffusion with Aesthetic Gradients 美学权重

https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2585

## 10/20 更新

激活函数

https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3199

SD v1.5

https://huggingface.co/runwayml/stable-diffusion-v1-5

## 10/19 更新

各种模型链接正在失效，有余力可以分流备链。

## 10/14 更新

[Rce 漏洞曝光](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2571)，但是新版本中得到修复，请为实例设置密码。

## 10/13 更新

automatic1111 的 repo 从 r/stablediffusion 的固定指南中删除

## 10/12 更新

StabilityAI 从现在开始只发布 SFW
模型，from [Here](https://www.reddit.com/r/StableDiffusion/comments/y2dink/qa_with_emad_mostaque_formatted_transcript_with/is32y1d/)

## 10/11 更新

新版本 Webui 增加了 Eta noise seed delta ，设为 31337 后会贴近 NAI 官方

现在可以训练 **Hypernetworks**

## 10/10 更新

测试发现，新版本 WebUi 优化显存占用 (20xx—>10xx)，关闭浏览器和 TG 的硬件加速后，即使是 4GB 的 RTX2050 也可以启动 --medvram
模式！而且很快。

[Q & A](https://github.com/brycedrennan/imaginAIry/blob/master/docs/emad-qa-2020-10-10.md)

## 背景故事

NovelAI 是一个使用 AI 生成故事文本和**通过描述文字生成图片**的服务，而 Stable Diffusion 是由 Stability AI
发布的透过文字等生成图片的模型。

开发者 [AUTOMATIC](https://github.com/AUTOMATIC1111) 是 Stable-Diffusion-Webui 的主要开发者：此项目可以用于在使用 Stable
Diffusion 等模型时调整参数，极大地方便了尤其是没有计算机背景或 AI/ML 背景的模型用户。

前几日 NovelAI [称其部分软件和源码泄露](https://old.reddit.com/r/NovelAi/comments/xydjc6/)。在模型泄露后， AUTOMATIC 在
Webui 项目中添加了对 **Hypernetwork** 模型的支持，使得此项目可以和泄露模型共用。

此 Reddit 贴称， Stability AI 创始人 Emad Mostaque 谴责 AUTOMATIC 此行为，并称后者窃取了代码；

AUTOMATIC 则称自己没有窃取代码，并解释说他编写的代码是基于很久以前已经完成的研究和开发，并且是开源的。有问题的函数于 2021 年
12 月 21 日在 [此处](https://github.com/CompVis/latent-diffusion/commit/e66308c7f2e64cb581c6d27ab6fbeb846828253b)
发布，并称反倒是 NovelAI 使用了自己的代码 (https://imgur.com/a/Z2QsOEw)。

Stable Diffusion 社群管理员后又要求 AUTOMATIC 移除项目中的 Hypernetwork 支持，称 NovelAI 核心开发者认为相关代码必然与泄露源码有联系；但被以代码原创且
Hypernetwork **并非泄露模型独创** 的理由回绝。

而后， AUTOMATIC 被从 Stable Diffusion 的（Slack?）社群服务器中封禁。

来自 https://rentry.org/sd-tldr
