# Txt2Img 

This page is a basic guide to Txt2Img for WebUi, if you want to learn how to write prompts, please read `Prompt Editing` related content.

If you are looking for some application examples, you should see the "Practical Guide" page.

## Basic parameters

`cfg scale` corresponds to the degree of prompt, the higher the value the more literal the prompt will be, the lower it will give the model more room to play, but the actual model performance in terms of CFG scale low (6-8) is low saturation, biased line drawing, biased clutter, high (18-22) is high saturation, biased CG style.

> High CFG will cause color distortion, CFG should be between 5-15

`denoise strength` img2img proprietary parameter, from 0 to 1, the higher the value the lower the AI reference to the original image (while increasing the number of iterations), personally I like low CFG high denoise heavy drawing, high CFG low denoise change the details.

If you have doubts about other parameters (e.g. `seed`), please check the WebUi parameters on the model debugging page.

## Negative cue words

The WebUi(SD) web application will **reject content related to negative prompt words** at generation time.

Negative cues are a way to use stable diffusion, allowing the user to specify what he does not want to see, without any additional burden or requirement on the model. The way it works is to use the user-specified text instead of the empty string as `unconditional_conditioning` when doing the sampling.

Here is the (simplified) code from [txt2img.py](https://github.com/CompVis/stable-diffusion/blob/main/scripts/txt2img.py).

```python
prompts = ["a castle in a forest"]
# batch_size = 1

c = model.get_learned_conditioning(prompts)
uc = model.get_learned_conditioning(batch_size * [""])

samples_ddim, _ = sampler.sample(conditioning=c, unconditional_conditioning=uc, [...]) )
```

This starts the sampler and iterates the following process.
```
    Denoise the image to make it look more like your cue (condition).
    Denoise the image to make it look more like an empty cue (unconditional condition).
    Observe the difference between the two and use it to produce a set of changes to the noisy picture (different samplers handle this part differently).
```
The sampler will then look at the difference between the noisy image that looks like your cue (a castle) and the noisy image that looks like your negative cue (grainy, foggy) and try to move the final result towards the former and away from the latter.

- A sample using negative hints

```python
prompts = ["a castle in a forest"]
# negative_prompts = ["grainy, fog"]

c = model.get_learned_conditioning(prompts)
uc = model.get_learned_conditioning(negative_prompts)

samples_ddim, _ = sampler.sample(conditioning=c, unconditional_conditioning=uc, [...])
```

For example, watermarks and textual content are rejected using the following prompt

```
lowres, bad anatomy, bad hands, text, error, missing fingers,
extra digit, fewer digits, cropped, worst quality, low quality,
normal quality, jpeg artifacts, signature, watermark, username, blurry
```

Also see this example

```
ugly, fat, obese, chubby, (((deformed))), [blurry], bad anatomy,
disfigured, poorly drawn face, mutation, mutated, (extra_limb),
(ugly), (poorly drawn hands fingers), messy drawing, morbid,
mutilated, tranny, trans, trannsexual, [out of frame], (bad proportions),
(poorly drawn body), (poorly drawn legs), worst quality, low quality,
normal quality, text, censored, gown, latex, pencil
```

## CFG Scale/Denoising strength fit/noise reduction

**CFG Scale**

``cfg scale`` is the reference to the prompt when predicting, the higher it is, the better the fit when predicting.

**Denoising strength Reducing noise**

`Denoising strength` determines how much the algorithm retains the content of the image, which can reduce the becoming of the drawing style, but also weakens the img2img capability. The higher the value, the less the AI references the original image (and increases the number of iterations).

For the image as a picture, a low `denoising` means correcting the original image, while a high `denoising` has no great relevance to the original image. Generally speaking, the threshold value is about 0.7, more than 0.7 and the original figure is basically irrelevant, 0.3 below is a slight change some.

The actual implementation, the specific implementation steps are Denoising strength * Sampling Steps.

## Image Prompt

By default, the program will add Prompt, Parameters, and Model Metadata information to the image. For the original image without compression, we can drag the file into the `PNG Info` tab to view the Prompt (Token).

Or use the [Online Tools](https://spell.novelai.dev/) to view it.

## Reverse Tokens

Here are some **Image Reverse Parameter Services** that can extract relevant parameters from images, not necessarily accurate.

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)

## Prompt Search Engine

You can visit the following links for some excellent examples of parameters.

- [PIXAI](https://pixai.art/)

- [StableDiffusion_Show](https://t.me/StableDiffusion_Show)

- [cyan_ai_sese](https://t.me/cyan_ai_sese)

- [LEXICA Search Engine](https://lexica.art/?q=Miku)

## Batch count&batch size

`batch count` Specifies how many batches of images to train.

`batchsize` translates to batch size in Chinese (batch size)

To put it simply, batch size will determine the number of samples we train at a time. The batch_size will affect how well and how fast the model is optimized.

Note also that `batch size` is designed to find the best balance between `memory efficiency` and `memory capacity`, since GPU resources are not fully utilized in a single 512x512 image generation. Larger images get less benefit from it.

!!! info
    Iteration is the action of repetitive feedback, where we want to train the neural network several times through iterations to reach the desired goal or result.
    The result of each iteration is used as the initial value for the next iteration.
    One iteration = one forward pass + one reverse pass

## Fix large size screen crash

`Highres.fix` is a convenient option to enable by checking this checkbox on the `txt2img` page.

By default, `txt2img` produces very chaotic images at very high resolutions. Whereas this plugin this makes it possible to avoid using small image compositions, partially render your image at lower resolutions, increase the resolution, and then add details at higher resolutions.

### Note `size`

For example, out of the picture size wide people may be more

!!! tip
    To match the pose, the lens and the character is not deformed, sometimes you need to limit the amount of words, when multiple characters to deal with spatial relationships and prompt masking priority. Number of people -> Character appearance -> Environment style -> Character state

Sizes above 1024 may give undesirable results! We recommend using small size + moderately high Step count + super-clear image resolution (see Advanced).

## Step iteration steps

More iterative steps may result in better generation, **more detail** and sharpening, but will result in longer generation times. In practice, the difference between 30 and 50 steps is almost indistinguishable.

Too many iterative steps may also be counterproductive, with little to no improvement.

When generating graphs, weaker denoising normally requires fewer iterative steps (this is how it works). You can change the setting in the settings to let the program perform exactly the number of iteration steps specified by the slider.

Higher-order samplers such as `DPM-Solver++` are more efficient and take fewer steps.

The exact execution steps in practical reasoning are Denoising strength * Sampling Steps.

## Samplers Samplers

First order currently good to use are `eular`, `eular a`, more detailed, and `Ddim`, labeled `++` samplers for higher order solvers, with fewer steps more detail and faster implementation.

**Newbies are recommended to use `eular a`**

`eular a` is creative, different steps can produce different pictures.
PS: adjust too high step (> 30) will not work better

`DDIM` converges fast, but relatively inefficient, because it takes many steps to get good results, **suitable for use in redrawing**

`LMS` and `PLMS` are derivatives of `eular`, which use a related but slightly different method (averaging over several steps to improve accuracy). Stable results can be obtained in about 30 steps

`PLMS` is an effective LMS (classical method) that can better handle the singularities in the neural network structure

`DPM2` is a fantastic method that aims to improve DDIM by reducing the number of steps to get good results. It requires two denoising runs per step and it is about twice as fast as DDIM. But if you are experimenting with debugging cue words, this sampler doesn't work very well

`Euler` is the simplest, and therefore one of the fastest

The higher-order sampler is `DPM-Solver++`, which you can try with `DPM++ 2S a`. Experiments have shown that DPM-Solver++ can generate high quality samples in only 15 to 20 steps for bootstrapping pixel space and potential space DPM. 

- Different results for different steps and samplers

| 预览一    | 预览二  |
| ----------------- | -------------- |
| <img src="https://user-images.githubusercontent.com/22421310/187063145-3d4f16d7-7bd6-4804-be1c-acf228ed2507.jpg" width="400" alt="效果"> | <img src="https://user-images.githubusercontent.com/75739606/197824518-f68188a3-0572-4b52-8fe7-289b6d7b640b.jpg" width="400" alt="效果"> |

![exp](https://user-images.githubusercontent.com/40903705/200149887-935a6f95-0bfa-4f8e-b6b1-0fb0bfe0b39e.jpg)

> from https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/4363

[英文 Wiki 介绍](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

[英文论坛介绍](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)

## Seed debugging

The actual seed integer is not important. It just initializes a random number generator that defines the starting point of the diffusion and is a random initial value.

A good seed can perform things like composition and color in various cues, samplers, and CFGs. But it is now of limited use.

With the same Step, cfg, Seed, parameters (prompts), produces essentially the same picture!

The same seed will produce the same image for the same model and back-end implementation, keeping all parameters the same. depending on the other parameters.

Note, however, that **different graphics cards may cause unexpectedly different results** (e.g. parameters like precision)

The 10xx series looks so different from all other cards, see [here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3873467)

### Merging Seeds

This technique is implemented by the UI. It is a way to merge different seeds from the same cue.

Click the `Extra` button next to the seed input to enable this feature.

Make changes to the initial noise to try to merge different features.
