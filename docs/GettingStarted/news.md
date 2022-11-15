# 新闻

!!! tip
    目前Stable Diffusion WebUi迭代非常快，每天都会有大量更新，因此建议每天都拉取最新代码。

以下是新闻和测试内容。

## 11/15

关于NAI Leak 的 Colab s DMCA 通知

https://lumendatabase.org/notices/29484955

https://lumendatabase.org/notices/29484957

>再次说明：Aidraw Wiki 不分发设计版权争议的Cpkt文件

## 11/12

[Stable-Diffusion 中文模型](https://huggingface.co/IDEA-CCNL/Taiyi-Stable-Diffusion-1B-Chinese-v0.1)

## 11/11

- [Pull#4563](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4563) 在 ngrok 中添加用户名和密码

```
Allows specifying the user of ngrok by username and password using the : --ngrok authtoken:username:password
and keep old args for not using username and password only use authtoken : --ngrok authtoken
```

## 11/10

- Add username and password in ngrok
https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4563

- 添加对 SSL/TLS 的支持（提供 Gradio TLS 选项）
https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4320


## 11/9 

- 许可问题

https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4222

## 11/8

- Colossal-AI: A Unified Deep Learning System for Big Model Era 
不明所以的项目
https://github.com/hpcaitech/ColossalAI


## 11/7

- DreamBooth WebUI 插件

https://github.com/d8ahazard/sd_dreambooth_extension

- 有插件中出现了广告

https://www.reddit.com/r/StableDiffusion/comments/ynshup/ads_are_starting_to_appear_in_our_foss/


## 11/6

- 新的采样高阶算法

DPM-Solver (and the improved version DPM-Solver++) is a fast dedicated high-order solver for diffusion ODEs with the convergence order guarantee.

[Add support for the new DPM-Solver++ samplers added to k-diffusion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4304)

[dpm-solver](https://github.com/LuChengTHU/dpm-solver)

论文 https://arxiv.org/abs/2211.01095

=== "Some Model"

    <div class="sample" markdown>

    ![exp](https://user-images.githubusercontent.com/40903705/200149887-935a6f95-0bfa-4f8e-b6b1-0fb0bfe0b39e.jpg){ align=left loading=lazy}

    Test From [Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/4363)

    *生成过程中使用了本地训练的超网络*

    </div>

=== "SD 1.5"

    <div class="sample" markdown>

    ![exp](https://user-images.githubusercontent.com/116967051/200134579-dc31f8d4-abd6-4ef0-9d2c-1582a53ec1b0.png){loading=lazy align=left width=300 data-title="DPM-Solver++" data-description="DPM-Solver++ 只需 15 到 20 个步骤即可实现出色的样本质量，尤其是 2M 和 2M Karras。从 15 步开始，所有步数的结果都非常详细，而且速度很快。"}

    Test From [Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4304#issuecomment-1304602296)

    DPM-Solver++ 只需 15 到 20 个步骤即可实现出色的样本质量，尤其是 2M 和 2M Karras。从 15 步开始，所有步数的结果都非常详细，而且速度很快。


    </div>



- 新的模型

eDiffi：具有专家降噪器集合的文本到图像扩散模型 
https://deepimagination.cc/eDiffi/


- 来自 DreamBooth 的一位原始作者：停止使用 SKS 作为初始化词 

https://www.reddit.com/r/StableDiffusion/comments/yju5ks/from_one_of_the_original_dreambooth_authors_stop/

- AI艺术走红赚钱：一位AI艺术家的自白 

https://www.reddit.com/r/StableDiffusion/comments/yh8j0a/ai_art_is_popular_and_makes_money_confessions_of/

## 11/5

WebUi 发布 [tokenizer插件](https://github.com/AUTOMATIC1111/stable-diffusion-webui-tokenizer)，可以查看分词情况。

## 11/3

- 发现了一个很好用，很实用的工具。

[画作焊接](https://github.com/BlinkDL/Hua)

![](https://raw.githubusercontent.com/BlinkDL/Hua/main/Hua-Demo.gif)

- 2

经过测试，我发现 emoji 真的很好用。对于场景效果总是有惊喜。

```
masterpiece,best quality,1girl,
,light blue hair,solo,(anime coloring:1.1),
(🌻☀️🌈:1.2),hug Sunflower,

lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet
```

## 11/2

为 Krita 插件等添加自定义后端 API

https://github.com/Interpause/auto-sd-paint-ext

在 [这个PR](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4142) 中，WebUi 修复了 IMG2IMG 处理后不释放 RAM 的问题


## 11/1

disable access to extension stuff for non-local servers

https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/dc7425a56e7a014cbfa3b3d44ad2321e519fe378

社区反映：共享链接可能会导致风险，攻击者可以访问系统上的所有文件。


## 10/30

在 [这次提交中](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/9f4f894d74b57c3d02ebccaa59f9c22fca2b6c90) ，WebUi添加了`allow skip current image in progress api ` ，跳过当前流程的 Api 方法。


在 [这次合并](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3722) 中，WebUi 添加了原生的进度 Api



## 10/29

[讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3889) 有人认为，通过在 Windows 设置上禁用硬件加速 GPU 调度，WebUi 性能提高了大约 10-50%



## 10/27

文档增加了 API 内容



## 10/26

[中文翻译上线](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/dde8c435987dcd071c63d16f247ae832bce0101f)

[法语翻译上线](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3653)

[土耳其翻译上线](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3631)

WebUi 在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/1e428238db4e399b7a06ad5251cb16eef23a014d) 中将 override_settings 添加到 API

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/cb49800c08a9f6619733250401952e5571dc12f8) 后，img2img 使用了手机照片的 EXIF 方向。

WebUi 在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/de096d0ce752c96e45508dcc7b9e84f7dbe10cca) 中添加了权重初始化和更多激活函数。

WebUi 在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/cbb857b675cf0f169b21515c29da492b513cc8c4) 中允许了在 `--medvram` 下创建 embedding 的情况。

`Hint:will send cond model to GPU if lowvram/medvram is active`


## 10/25

[集成Tag工具/支持超网络的Web](https://git.hudaye.work/MiuliKain/Kamiya-OpenUI)开源项目说他们缺人。

在 [此次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3624) 中，从 `--use-cpu` 中删除了 `BSRGAN`，将 `SwinIR` 添加到 `--use-cpu` 并修复 MPS 上的 `upscalers`。

你可以在[这里](https://upscale.wiki/wiki/Official_Research_Models) 找到 `BSRGAN` ，下载的模型放到esrgan文件夹就可以使用。

`#3505`之后超网络训练不稳定 ，[问题](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/3505)


## 10/24

Colab 称昨天的封号是 [错误的滥用判定](https://github.com/googlecolab/colabtools/issues/3181)

WebUi [移除了图像浏览器](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3537)，转换为 [插件](https://github.com/yfszzx/stable-diffusion-webui-images-browser)


## 10/23 更新

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/070fda592bf80fb348ffe8e17b7c71cc288db729) 中，WebUi增加了日本语翻译。

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/be748e8b086bd9834d08bdd9160649a5e7700af7) 中，WebUi 增加了设置锁定启动参数。

```
parser.add_argument("--freeze-settings", action='store_true', help="disable editing settings", default=False)
```

有网友称，最近发布的的 Stable diffusion 1.5 偏向`三次元`一些。

WebUi 增加了俄语翻译，添加了 img2img API。

在 [这次请求](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2067) 中，WebUi 更新了 ESRGAN 架构和模型以支持所有 ESRGAN 模型。

Colab 开始反滥用，[协议](https://research.google.com/colaboratory/faq.html#limitations-and-restrictions)禁止穿透。但是有人指出 [Colab并没有计划封禁 SD](https://github.com/googlecolab/colabtools/issues/3147)，确认误封。

Colab的 [协议](https://research.google.com/colaboratory/faq.html?hl=zh-CN) 不允许利用多个帐号绕过访问权限或资源使用情况限制。

有人反映 某度好像会封锁NAI的模型。


## 10/22 更新

在 [这次提交](https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/2b91251637078e04472c91a06a8d9c4db9c1dcf0) 中，仓库移除了美学权重。

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

StabilityAI 从现在开始只发布 SFW 模型，from [Here](https://www.reddit.com/r/StableDiffusion/comments/y2dink/qa_with_emad_mostaque_formatted_transcript_with/is32y1d/)


## 10/11 更新
新版本 Webui 增加了 Eta noise seed delta ，设为 31337 后会贴近NAI官方

现在可以训练 **Hypernetworks**


## 10/10 更新
测试发现,新版本 WebUi 优化显存占用(20xx—>10xx)，关闭浏览器和TG的硬件加速后，即使是4GB的  RTX2050 也可以启动 --medvram 模式！而且很快.

[Q & A](https://github.com/brycedrennan/imaginAIry/blob/master/docs/emad-qa-2020-10-10.md)


## 背景故事

NovelAI 是一个使用 AI 生成故事文本和**通过描述文字生成图片**的服务，而 Stable Diffusion 是由 Stability AI 发布的透过文字等生成图片的模型。

开发者 [AUTOMATIC](https://github.com/AUTOMATIC1111) 是 Stable-Diffusion-Webui 的主要开发者：此项目可以用于在使用 Stable Diffusion 等模型时调整参数，极大地方便了尤其是没有计算机背景或 AI/ML 背景的模型用户。

前几日 NovelAI [称其部分软件和源码泄露](https://old.reddit.com/r/NovelAi/comments/xydjc6/)。在模型泄露后， AUTOMATIC 在 Webui 项目中添加了对 **Hypernetwork** 模型的支持，使得此项目可以和泄露模型共用。

此 Reddit 贴称， Stability AI 创始人 Emad Mostaque 谴责 AUTOMATIC 此行为，并称后者窃取了代码；

AUTOMATIC 则称自己没有窃取代码，并解释说他编写的代码是基于很久以前已经完成的研究和开发，并且是开源的。有问题的函数于 2021 年 12 月 21 日在 [此处](https://github.com/CompVis/latent-diffusion/commit/e66308c7f2e64cb581c6d27ab6fbeb846828253b) 发布，并称反倒是 NovelAI 使用了自己的代码(https://imgur.com/a/Z2QsOEw)。


Stable Diffusion 社群管理员后又要求 AUTOMATIC 移除项目中的 Hypernetwork 支持，称 NovelAI 核心开发者认为相关代码必然与泄露源码有联系；但被以代码原创且 Hypernetwork **并非泄露模型独创** 的理由回绝。

而后， AUTOMATIC 被从 Stable Diffusion 的（Slack?）社群服务器中封禁。

来自 https://rentry.org/sd-tldr


