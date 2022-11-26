# Img2*

This page describes the basic operation of Img2Img. If you are confused about the parameters, please see the WebUi parameters on the Model Debugging page.

If you are looking for some application examples, you should see the "Practical Guide" page.

!!! tip
    Please read the previous section, Model Advance 1, for specific Img2Img and inpaint introduction operations.

## Basic parameters

`cfg scale` corresponds to the degree of prompt, the higher the value the more literal the prompt will be, the lower it will give the model more room to play, but the actual model performance in terms of CFG scale low (6-8) is low saturation, on the cluttered side, high (18-22) is high saturation, more intense style.

> High CFG will cause color distortion, CFG should be between 5-15

`denoise strength` img2img proprietary parameter, from 0 to 1 value, the higher the value the lower the AI reference to the original image (while increasing the number of iterations), personally I like low CFG high denoise heavy drawing, high CFG low denoise change the details.

If you have doubts about other parameters (e.g. `seed`), please check the WebUi parameters on the model debugging page.

## Img2Txt

Under the Generate button there is an `Interrogate CLIP` which when clicked will download the `CLIP` for reasoning about the Prompt of the image in the current image box and filling it to the prompt.

The CLIP interrogator has two parts: one is the BLIP model, which takes on the function of decoding and reasoning about the text description from the image. The other is the CLIP model, which transforms the text into vectors (during the inference of the model).

!!! tip
    The model used by Img2Txt is size `855MB`

## Img2Img

>application see practicalguide

In img2img tab, draw a mask over a part of the image, and that part will be in-painted.

We generally have two ways of restoring images: **PS and InPaint**, and there are very different ways of using them.

WebUi uses `--gradio-img2img-tool color-sketch` to start up a plugin that brings in an image to be coloured (not Inpaint in this case)

!!! tip "The difference"
    PS Re-painting into Img2Img will result in a change of style, whereas Inpaint will not.

### Resizing

*Just resize* - simply resizes the source image to the target resolution, resulting in an incorrect aspect ratio

*Crop and resize* - resize source image preserving aspect ratio so that entirety of target resolution is occupied by it, and crop parts that stick out

*Resize and fill* - resize source image preserving aspect ratio so that it entirely fits target resolution, and fill empty space by rows/columns from the source image

### Img2Img 3D rendering 2D

It is easier to adjust the skeleton of a 3D model than to find a sample drawing.

Pose can be combined with **3D modelling**, you can use MMD related software.

If it is a real life image, you need to increase the `CFG Scale` similarity and generate it together with prompts. The higher the noise reduction `denoising`, the lower the relevance.

We recommend using [DAZ](https://www.daz3d.com/get_studio) or [blender](https://www.blender.org/) or Unity. In tests on 3D models, **colour mainly affects how the AI paints**, so your model needs to be textured.

If you use blender, you can use [this video](https://youtu.be/MClbPwu-75o) shared by [model dolls](https://www.artstation.com/marketplace/p/VOAyv/stable-diffusion-3d) -posable-manekin-doll?utm_source=artstation&utm_medium=referral&utm_campaign=homepage&utm_term=marketplace)

### Img2Img **Outpainting**

Outpainting extends the original image and repairs the blank space created.
You can find this function in the img2img tab at the bottom, under Script -> Poor man's outpainting.

```
Outpainting, unlike normal image generation, seems to profit very much from large step count. A recipe for a good outpainting is a good prompt that matches the picture, sliders for denoising and CFG scale set to max, and step count of 50 to 100 with Euler ancestral or DPM2 ancestral samplers.
```

### Img2Img **Inpainting**

In the Inpainting tab, draw a mask over a part of the image that will be repainted.

The `Masked content` setting determines the content placed into the masked area prior to the repair. Generally select `original` to maintain potential spatial consistency, or `fill` if you don't want the repaired content to inherit the original colour distribution, to use most of the image's base colour, and `latent noise` to get a random colour dot pattern (to make the the generated content out of association).

AS:

| mask-sample  | fill  | original   | latent noise      | latent nothing       |
|---------------------------|----------------|-----------------------|-------------------------|-----------------------|
| ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-mask.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-fill.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-original.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-noise.png) | ![](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting-initial-content-latent-nothing.png) |

The horizontal bar `mask` determines the degree of blurring. original is `original`, fill is `fill base` and `fill` takes more steps to remove the unnatural feel.

`Inpaint at full resolution` is a full resolution fix. By default Inpaint resizes the resulting image **as a whole** to the target resolution specified in the *UI*. When `Inpaint at full resolution` is enabled, **only the masked area** is resized and pasted **back** to the original image after processing. This allows you to process large size images and allows the repair object to be rendered at a larger resolution.

There are currently several ways to carry out the redraw operation.

- Draw your own mask in a web editor (`Inpaint masked` means redrawing the painted area, `Inpaint not masked` means redrawing the area outside the painting)

- Erase part of the image in an external editor and upload a transparent image. The transparent area will become part of the mask.Tips: Some editors save fully transparent areas as black by default.

- Change the mode (bottom right of the image) to `Upload mask` and process as a separate black and white image for the mask (the white parts will be inpainted).

If `inpaint at full resolution` appears as a black block, there may be insufficient RAM, try unload vae.

![result](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/images/inpainting.png)
<!--
[开源调研-AI 绘画全参数讲解-002img2img 图像到图像](https://www.bilibili.com/video/BV1HK411Q7uk/?zw)

<iframe src="//player.bilibili.com/player.html?aid=474043788&bvid=BV1HK411Q7uk&cid=860273094&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

In this way, we can change the style of the character's clothing or any other detail.

<!--
[如何教会 Ai 画手](https://www.bilibili.com/video/av559044202/?zw)

<iframe src="//player.bilibili.com/player.html?aid=559044202&cid=859852841&page=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
-->

### Img2Img Loopback

Selecting the loopback script in img2img allows you to automatically feed output image as input for the next batch. Equivalent to saving output image, and replacing the input image with it. Batch count setting controls how many iterations of this you get.

Usually, when doing this, you would choose one of many images for the next iteration yourself, so the usefulness of this feature may be questionable, but I've managed to get some very nice outputs with it that I wasn't able to get otherwise.

### Img2Img with low vram

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

[^4]:[Paper 朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD 金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风 tag 训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI 即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)
