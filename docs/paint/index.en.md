# Getting Started Painting

>Todo
>Upscaler 2 的预期用途 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3749
>


This section will introduce the parameters and related SDWebUi resources. If you can draw, the results will be more stable and considerable.

## Workstations

The tag reference should use [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups) and [MidJourney-Styles-and-Keywords-Reference](https:// github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)

For brushstrokes/painting tools/styling/painting techniques tag group see [Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

If you are used to reading Japanese tag references and techniques, you can search in [Japanese Wiki](https://seesaawiki.jp/nai_ch/d/%be%ec%bd%ea%a1%a6%c7%d8%b7%ca).

This section covers: imitating style, specific shots, adding effects, fine-tuning and fine-tuning, PS grafting out of the picture, specific poses through 3D, redrawing, iteration

>Note: WebUi's settings page requires pressing `Apply setting` to save the settings.



## Basic flow


![WorkFlow](https://user-images.githubusercontent.com/75739606/199486119-d6eb3f94-72be-498b-8862-0775c45e8950.png)
<!--
![WorkFlow](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/draw_workflow.svg)
-->
This diagram demonstrates the flow of circular iteration
There are two types of iterations, circular iterations and linear iterations. Linear iterations are suitable for diversity testing, while **loop iterations** are a better choice for optimization.

`Changing hints back and forth + seeds` is not a good choice.

The basic direction of the current research is

- prompt + PS/Inpaint (fine-tuning/grafting)

- prompt + 3D reference

## Parameter Introduction

From [^1]

- Prompt: textual description of what you want to generate.
    
- Negative prompt: textual description of things that you don't want in the image.
    
- Sampling Steps: diffusion models work by making small steps from random Gaussian noise towards an image that fits the prompt. This is how many such steps should be done. More steps means smaller, more precise steps from noise to image. Increasing this directly increases the time needed to generate images. Diminishing returns, depends on sampler.
    
- Sampling method: which sampler to use. Euler a (short for ancestral) produces great variety with a low number of steps, but it's very difficult to make small adjustments. The non-ancestral samplers all produce mostly the same images as the number of steps increases, use LMS if you're unsure.
    
- ddim_eta: amount of randomness when using DDIM.
    
- Batch count/n_iter: how often to generate a set of images.
    
- Batch size: how many images to generate at the same time. Increasing this value can improve performance but you also need more VRAM. Total number of images is this multiplied with batch count.
    
- CFG Scale (classifier-free guidance scale): how strongly the images match your prompt. Increasing this value will result in images that resemble your prompt more closely (according to the model) but it also degrades image quality after a certain point. Can be somewhat counteracted with more sampling steps.
    
- Width: width of individual images in pixel. To increase this value you need more VRAM. Image coherence on large scales (model was trained on 512x512) becomes worse as the resolution increases. Very small values (e.g. 256 pixels) also degrade image quality.
    
- Height: same as Width but for individual image height.
    
- Seed: starting point for RNG. Keep this the same to generate the same (or almost the same) images multiple times. There are no seeds that are inherently better than others but if you vary your input parameters only slightly seeds that produced good results previously will likely still produce good results.

# Repo

[WebUi: Adding custom backend APIs for Krita plugins and more](https://github.com/Interpause/auto-sd-paint-ext)



[^1]:[installgentoo wiki](https://wiki.installgentoo.com/wiki/Stable_Diffusion)