# 新闻

!!! tip
    目前Stable Diffusion WebUi迭代非常快，每天都会有大量更新，因此建议每天都拉取最新代码。

以下是新闻和测试内容。


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


