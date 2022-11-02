## Magic Advanced Guide

Much of the content is taken from the repo s [Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

The basic principle is vaguely stated: the more similar samples within a well-defined range of data, the more accurate it is.

!!! tip
    Please make sure you read the first section above for the specific Img2Img and inpaint introductory operations.



## Img2Txt

Under the Generate button there is an `Interrogate CLIP` which when clicked downloads the `CLIP` which is used to generate a Tag for the image in the current image box and populate it with prompt words.

The CLIP interrogator has two parts: one is the BLIP model, which creates a text description from the image. The other is the CLIP model, which picks out a few lines from the list that are relevant to the image

!!! tip
    This file is [model_base_caption_capfilt_large.pth](https://storage.googleapis.com/sfr-vision-language-research/BLIP/models/model_base_ caption_capfilt_large.pth)

    The size is 855MB




## Img2Img Introduction

In img2img tab, draw a mask over a part of the image, and that part will be in-painted.

We generally have two ways of restoring images: **PS and InPaint**, and there are very different ways of using them.

WebUi uses `--gradio-img2img-tool color-sketch` to start up a plugin that brings in an image to be coloured (not Inpaint in this case)

!!! tip "The difference"
    PS Re-painting into Img2Img will result in a change of style, whereas Inpaint will not.




### Graphical parameters

Just resize : Resizes the image to the target resolution. The image will be squeezed unless the height and width exactly match

Crop and resize : Resizes the image so that the entire target resolution is filled by the image. Crop the excess.

Resize and fill: Resizes the image so that the entire image is within the target resolution. Fill blank areas with the colour of the image.



## Img2Img 3D rendering 2D

It is easier to adjust the skeleton of a 3D model than to find a sample drawing.

Pose can be combined with **3D modelling**, you can use MMD related software.

If it is a real life image, you need to increase the `CFG Scale` similarity and generate it together with cue words. The higher the noise reduction `denoising`, the lower the relevance.

We recommend using [DAZ](https://www.daz3d.com/get_studio) or [blender](https://www.blender.org/) or Unity. In tests on 3D models, **colour mainly affects how the AI paints**, so your model needs to be textured.

If you use blender, you can use [this video](https://youtu.be/MClbPwu-75o) shared by [model dolls](https://www.artstation.com/marketplace/p/VOAyv/stable-diffusion-3d) -posable-manekin-doll?utm_source=artstation&utm_medium=referral&utm_campaign=homepage&utm_term=marketplace)




## Img2Img PS repaint/graft repair/layout fill

**Size should be as close to the original size as possible, then select `Crop and resize` aka crop and resize**

Use PS software to add and delete elements and then re-produce them. This can solve the problem of drawing hands.

Ai also accepts other finished images for grafting (to solve the problem of lying down without a lower body)

For example, we can use PS to graft a hand to a character for Ai to touch up, or graft the lower body of another work for a bust without a lower body for AI to touch up.

Or paint a specific part of the body with a specified shape action (such as the coverage or shape of a garment) and hit it with a colour block (to prevent shadows, **please take a bright skin**).

![test_woman](https://user-images.githubusercontent.com/75739606/197823480-5de77d69-46d5-4817-948f-4e514e1f8204.jpg)
<!--
![info](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/00119_136826557_masterpiece%2C_best_quality%2C _1girl%2C_black_hair%2C_hat1.jpg)
-->
>An image [^5] showing a detailed comparison of the effect of different parameters in img2img under WebUI (prompt, steps, scale, various seeds, etc. are kept consistent)

The vertical axis is Denoising strength (online version of strength) and the horizontal axis is Variation strength



### Redrawing techniques/removal/replacement

- Start by tracing lines on the figure, then hit the colour block (if there are shadows, take **bright skin**).

- Vary the intensity and choose a lower denoising of around 0.3.

- Then use Img2Img Inpaint + the relevant cue word to fix it, and change it again if you're not happy with it until you are.

- The image is then hyper-scored and noise reduced (image texture removed).



## Img2Img **Outpainting**

Outpainting extends the original image and repairs the blank space created.
You can find this function in the img2img tab at the bottom, under Script -> Poor man's outpainting.

```
Outpainting, unlike normal image generation, seems to profit very much from large step count. A recipe for a good outpainting is a good prompt that matches the picture, sliders for denoising and CFG scale set to max, and step count of 50 to 100 with Euler ancestral or DPM2 ancestral samplers.
```

## Img2Img **Inpainting**

In the img2img tab, draws a mask over a part of the image that will be repainted.

For the `Masked content` setting, the Masked content field determines the content to be placed in the masked area before it is repaired.

`original` is generally selected to maintain potential spatial consistency.

AS:

| mask  | fill  | original   | latent noise      | latent nothing       |
|---------------------------|----------------|-----------------------|-------------------------|-----------------------|
| ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-mask.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-fill.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-original.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-noise.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-nothing.png) |

`mask` determines the degree of blurring, original is `original` and fill is `fill base`.

Tip: `fill` requires more steps to remove the unnatural look.

**Inpaint at full resolution**
Normally, inpainting resizes the image to the target resolution specified in the UI. With Inpaint at full resolution enabled, only the masked region is resized, and after processing it is pasted back to the original picture. This allows you to work with large pictures and allows you to render the inpainted object at a much larger resolution.

Options for inpainting:

* draw a mask yourself in the web editor
* erase a part of the picture in an external editor and upload a
  transparent picture. Any even slightly transparent areas will become
  part of the mask. Be aware that [some editors](https://docs.krita.org/en/reference_manual/layers_and_masks/split_alpha.html#how-to-save-a-png-texture-and-keep-color-values-in-fully-transparent-areas) save completely transparent areas as black by default.
* change mode (to the bottom right of the picture) to "Upload mask"
  and choose a separate black and white image for the mask
  (white=inpaint).



If `inpaint at full resolution` appears as a black block, there may be insufficient RAM, try unload vae.


![result](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting.png)
<!--
[开源调研-AI绘画全参数讲解-002img2img图像到图像](https://www.bilibili.com/video/BV1HK411Q7uk/?zw)

<iframe src="//player.bilibili.com/player.html?aid=474043788&bvid=BV1HK411Q7uk&cid=860273094&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

通过这种方法，我们可以更改角色衣物风格或者其他任何细节。
<!--
[如何教会Ai画手](https://www.bilibili.com/video/av559044202/?zw)

<iframe src="//player.bilibili.com/player.html?aid=559044202&cid=859852841&page=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

## Img2Img Loopback


Selecting the loopback script in img2img allows you to automatically feed output image as input for the next batch. Equivalent to saving output image, and replacing the input image with it. Batch count setting controls how many iterations of this you get.

Usually, when doing this, you would choose one of many images for the next iteration yourself, so the usefulness of this feature may be questionable, but I've managed to get some very nice outputs with it that I wasn't able to get otherwise.


## Img2Img with low vram

As mentioned earlier, the Img2Img image quality enhancement script can be used if you encounter confusing patterns or need to produce high resolution images with low vram.

In fact I **highly recommend** that you use the Extras feature to replay low resolution images, it works well and is a great experience!


### Scripts

But if you want to use the resolution enhancement provided by the script, here's the exact procedure for Img2Img

1. start webui with the `--medvram` or `--lowvram` parameter

2. Choose a smaller resolution to generate the image. Remember the resolution of the image you are generating. Once the image has been generated, copy the `Seed` of the image. 3.

3. Once the image has been generated, check to see if you are happy with the result. If you are happy, send the image directly to img2img (click `Send to img2img`)

4. At the bottom of the img2img screen, there is a `Script` option. Select `Script` as `SD Upscale` and make the Tile overlap as small as possible.

The input box is normally filled with the original prompt when you feed the Img2img image. If you find that the prompt has changed, please fill it manually

6. select the appropriate `Sampling Steps` and `Sampling method`.

7. Make sure your `Width` and `Height` match the **original drawing**.

8. Fill the Seed copied in step 2 with the Seed of img2img and generate

The Width and Height here are the sizes of the img2img images when overscaled, if they are not equal they will cause overlap problems

The SD Upscale option is in the Script section of Img2Img and is mainly used to increase the resolution.



[FROM Here](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#%E9%AB%98%E5%88%86%E8%BE%A8%E7%8E%87%E4%B8%8B%E5%87%BA%E6%80%AA%E5%9B%BE)

### Resolution boost with extras

The `webui` extras page has a self-contained extras function, which can be used with the `ESRGAN_4x` model

Of course `realesrgan` or `realcugan` will also work.

!!! tip "Related models"

    Download the files to the `SDwebUI folder \models`

    [LDSR](https://heibox.uni-heidelberg.de/f/578df07c8fc04ffbadf3/?dl=1), file size 1.9GB

    [BSGRAN 4x](https://github.com/cszn/KAIR/releases/download/v1.0/BSRGAN.pth) with a file size of 63.9M

    [ESRGAN_4x](https://github.com/cszn/KAIR/releases/download/v1.0/ESRGAN.pth) with a file size of 63.8MB

    [ScuNET GAN/PSNR](https://github.com/cszn/KAIR/releases/download/v1.0/scunet_color_real_gan.pth" to D:\stable-diffusio\models\ScuNET ScuNET.pth), with a file size of 68.6MB

    [SwinIR 4x](https://github.com/JingyunLiang/SwinIR/releases/download/v0.0/003_realSR_BSRGAN_DFOWMFC_s64w8_SwinIR-L_x4_GAN.pth), file size is 136MB

**Highres Fix/ What Upscaler should I use for overscoring?

The `SD Upscaler` improves resolution while paying attention to detail.

There was a time when `LSDR` was considered to be the best. Some people like swinir, some like `esrgan4x`, `ymmv`, `ESRGAN_4x` is recommended

If you want to generate anime characters, [realcugan](https://github.com/bilibili/ailab/tree/main/Real-CUGAN) is recommended


## Image denoising

We recommend using [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) for noise reduction.

![Effect](https://raw.githubusercontent.com/xinntao/Real-ESRGAN/master/assets/teaser.jpg)
>Effects


## AI with DLC

See the next section for more details.


## Note SIZE

For example, there may be more people if the picture size is wide

!!! tip
    It is important to match the pose so that the shot and the figure are not distorted, sometimes you need to limit the quantifiers, deal with spatial relations and prompt masking priorities when there are multiple figures. Number of people->Personal appearance->Environmental style->Personal state





## Quick prompt search


**[手抄本法术书](https://docs.google.com/spreadsheets/d/14Gg1kIGWdZGXyCC8AgYVT0lqI6IivLzZOdIT3QMWwVI/edit)**

**[Danbooru](https://gelbooru.com/index.php?page=tags&s=list)**

**[参数法术全典](https://docs.google.com/spreadsheets/d/e/2PACX-1vRa2HjzocajlsPLH1e5QsJumnEShfooDdeHqcAuxjPKBIVVTHbOYWASAQyfmrQhUtoZAKPri2s_tGxx/pubhtml#)**

**[Ai Artists](https://docs.google.com/spreadsheets/d/1SRqJ7F_6yHVSOeCi3U82aA448TqEGrUlRrLLZ51abLg/htmlview#)**


## Others

[https://github.com/Maks-s/sd-akashic](https://github.com/Maks-s/sd-akashic)

[https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)



[^4]:[Paper朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)
