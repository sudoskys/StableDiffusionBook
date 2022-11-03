# WebUi

Please find out about the parameters of the WebUi(SD) web application beforehand.

## Introduction to the prompting method

For NAI users, please read the [official website Docs](https://docs.novelai.net/image/promptmixing.html)

!!! tip
    The two-sided tip method **not common**
    Use `()` for WebUi enhancements and `{}` for NAI enhancements, note that NAI does not support assigning weights to individual words

    If you want to convert weights easily, you can use the [conversion service](https://t.me/M2NM2NBot)

**Representation method**

Separation, the English half-word `,` is used as a separator, the presence of one or several spaces before and after the English comma does not affect the actual use.

Blending, WebUi uses `|` to separate multiple keywords to blend multiple elements, literally blending, and can be used in more than one way.

Augmentation, using `(hint:weight)` in WebUi weight augmentation, with augmentation ranging from `0.1 to :100`, allowing decimals. Whereas NAI uses `{}`

!!! tip
    `a ((((farm))), daytime` syntax may eat up the comma

Prompt editing, using `[some1:some2:num]`

`[fantasy:cyberpunk:16]` for replacing `fantasy` with the `cyberpunk` tag from step 16 onwards

`[to:when]` adds `to` to the prompt after a fixed number of steps ( when)

`[from::when]` Remove `from` from the prompt after a fixed number of steps ( when)

Escape, WebUi for bracketed hints such as `a (word)` use the `\` character to escape to `a \(word\)`, this applies to bracketed Tags to prevent unwanted enhancements.

Reduced weight, `[]` or `(word:0.952)`. NAI can only use `[]`

alternate prompt [^7], mixer. This allows you to create hybrids of animals, people or styles, switching one item per step (one step at a time), rendering in rounds, `[alison brie|emma stone|elizabeth olsen|scarlett johansson|anne hathaway|emma roberts], still film` This is the WebUi syntax, which in NAI represents the average weight mix (first half and second half).



!!! tip "NAI"

    NAI does not allow individual weights to be specified, but supports a mixed weight syntax of `cat:1|happy:-0.2|cute:-0:3`.

    Because NAI uses an implementation of WebUi prior to 29 September 2022, the weight enhancement syntax is the old `{}`, which the new WebUi changes to `()`.

    **commutation relations**
    NAI's [word] = WebUi(word:0.952)(0.952 = 1/1.05)

    NAI's {word} = WeUi's (word:1.05)

    NAI brackets have a weight of 1.05/pc, WebUi rounds have a weight of 1.1/pc


## How to write prompt

Write it in conjunction with a spell book.

- Natural language

Natural language can be used directly, WebUi(SD) has natural language processing capabilities (English sentences), and can also use  emoji

- parameter [^6]

Combine similar cue phrases as you wish and put these in order from most to least important.

You can use artists, studios, photographic terms, character names, styles, special effects, etc. These can be viewed in the manuals, which are available in the end pages.

```
The quality of the image
The subject of the pictures
Their appearance
Their emotions
Their clothes
Their poses
The background of the picture
```

[Guide to Writing Prompts for Text-to-image Ai: A Guide to Writing Prompts for Text-to-image AI](https://docs.google.com/document/d/ 1XUT2G9LmkZataHFzmuOtRXnuWBfhvXDAo8DkS--8tec/edit#)

**Writing length**

- The Tip should not be too long, there is a risk of failure if it exceeds 100.

You can write prompts with more than 75 words. Originally, due to GPT-3 model limitations, the prompts are not unlimited, the positive token is between 75-80, and the content after 75 words is truncated. But WebUi has grouping, so you can write many words.

But not stacking prompts is a good habit.

When the prompt exceeds 75 `tokens` (say 150 `tokens`), WebUi will group the prompt words and submit multiple sets of 75 `tokens`. The tokens will only have the context of the rest of the content in the same set. This means that you may have `bule hair` at the boundary between the first and second group, token `blue` will be in the first group and `hair` will be in the second group. This led to inaccurate results as the two words were separated.

The new version adds an option `Increase coherency by padding from the last comma within n tokens when using more than 75 tokens`

This setting lets the program try to alleviate this by finding if there is a last comma in the last N tokens, and if so, moving everything that passes through that comma together to the next collection.

!!! tip "example"

    There are entries for the words `... ,comma,blue hair,PADDING,... `

    The 75th word is `blue`

    **before using this option**

    Set 1:{[74]=Comma, [75]=blue}, Set 2:{[76]=hair, [77]=PADDING}

    **After using this option**

    Set 1:{[074]=Comma, [75]=PADDING}, Set 2:{[76]=blue, [77]=hair}

    If your hint is less than or equal to 75 tokens, no grouping will occur.


**Writing format**

To facilitate writing changes, the recommended format for multi-line writing is as follows

```
masterpiece,
1girl,  hatsune miku,
look at viewer,turning back,
blow wind, cyan hair, two side up, cyan eyes,eardrop,dress,
caustics, masterpiece, high resolution,
```

The first line specifies the style type, the second the character, the third the action, the fourth the scene and costume, and the fifth the other parameters.

The above order makes sense, but you can also adjust the order of the cues to produce different results. You can do this manually, or [use the X/Y diagram to automatically generate the various orders](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1607)

The difference between an underline and no underline is not significant and will be cleaned up by the program as dust.


**Stylisation**

Nai comes out with a boring style by default. You can create images with special effects or a specified drawing style by training a style model, specifying style keywords, etc.

[人偶教室的测试记录](https://www.yuque.com/longyuye/lmgcwy)

[风格化: 32种](https://www.bilibili.com/video/BV1TP411N71t/)

A more comprehensive guide is at the end of the page.




## Sequencing Compilation

The order in which the prompts are placed is the priority.(MAYBE)

The way webui breaks the tag limit of 75 is by dividing them into groups.

For the purpose of improving readability, it is recommended that the main body be placed first, followed by the words describing the costume, with the picture quality enhancement words interspersed between these descriptors. Generally, words that change the composition, such as action and nsfw words, are placed later or manually weighted down in order to improve the finish rate.

The above ordering is the same for each group of tags, so if the later tags are over 75, you should split the earlier ones.




## Batch count&batch size

`batch count` specifies how many batches of images to train.

`batchsize` specifies how many to train at a time.

In simpler terms, the batch size will determine the number of samples we train at a time. The batch_size will affect how well and how fast the model can be optimised.

Note also that `batch size` is designed to find the best balance between `memory efficiency` and `memory capacity`, as GPU resources are not fully utilised in a single 512x512 image generation. Larger images get less benefit from it.

!!! info
    Iteration is the action of repetitive feedback, where we want to train the neural network many times through iterations to reach the desired goal or result.
    The result of each iteration is used as the initial value for the next iteration.
    One iteration = one forward pass + one backward pass



## (prompt) impact factor [^6]

Using () in the prompt increases the model's attention to enclosed words, and [] decreases it. You can combine multiple modifiers

[Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)

Cheat sheet:

* `a (word)` - increase attention to `word` by a factor of 1.1
* `a ((word))` - increase attention to `word` by a factor of 1.21 (= 1.1 * 1.1)
* `a [word]` - decrease attention to `word` by a factor of 1.1
* `a (word:1.5)` - increase attention to `word` by a factor of 1.5
* `a (word:0.25)` - decrease attention to `word` by a factor of 4 (= 1 / 0.25)
* `a \(word\)` - use literal `()` characters in prompt

With `()`, a weight can be specified like this: `(text:1.4)`. If the weight is not specified, it is assumed to be 1.1. Specifying weight only works with `()` not with `[]`.

If you want to use any of the literal `()[]` characters in the prompt, use the backslash to escape them: `anime_\(character\)`.

On 2022-09-29, a new implementation was added that supports escape
characters and numerical weights. A downside of the new implementation
is that the old one was not perfect and sometimes ate characters: "a
(((farm))), daytime", for example, would become "a farm daytime" without
the comma. This behavior is not shared by the new implementation which
preserves all text correctly, and this means that your saved seeds may
produce different pictures. For now, there is an option in settings to
use the old implementation.

NAI uses my implementation from before 2022-09-29, except they have 1.05 as the multiplier and use `{}` instead of `()`. So the conversion applies:

* their `{word}` = our `(word:1.05)`
* their `{{word}}` = our `(word:1.1025)`
* their `[word]` = our `(word:0.952)` (0.952 = 1/1.05)
* their `[[word]]` = our `(word:0.907)` (0.907 = 1/1.05/1.05)



??? tip "How It Work"
    Each word has an associated vector of 768 values that 'points' in the direction of the concept (in 768 dimensions).

    If you scale this vector, the concept will become stronger or weaker.

    From [Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2905)



## Negative prompt

Allows you to use another prompt of things the model should avoid when generating the picture. This works by using the negative prompt for unconditional conditioning in the sampling process instead of an empty string.

SEE [HERE](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Negative-prompt)

Negative prompt is a way to use the Stable Diffusion in a way that allows the user to specify what he doesn't want to see, without any extra load or requirements for the model.

To use negative prompt, all that's needed is this:

```
# prompts = ["a castle in a forest"]
# negative_prompts = ["grainy, fog"]

c = model.get_learned_conditioning(prompts)
uc = model.get_learned_conditioning(negative_prompts)

samples_ddim, _ = sampler.sample(conditioning=c, unconditional_conditioning=uc, [...])
```

The sampler then will look at differences between image de-noised to look like your prompt (a castle), and an image de-noised to look like your negative prompt (grainy, fog), and try to move the final results towards the former and away from latter.


For example, watermarks and text content are rejected using the following prompt

```
lowres, bad anatomy, bad hands, text, error, missing fingers,
extra digit, fewer digits, cropped, worst quality, low quality,
normal quality, jpeg artifacts, signature, watermark, username, blurry
```

and


```
ugly, fat, obese, chubby, (((deformed))), [blurry], bad anatomy,
disfigured, poorly drawn face, mutation, mutated, (extra_limb),
(ugly), (poorly drawn hands fingers), messy drawing, morbid,
mutilated, tranny, trans, trannsexual, [out of frame], (bad proportions),
(poorly drawn body), (poorly drawn legs), worst quality, low quality,
normal quality, text, censored, gown, latex, pencil
```

[Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Negative-prompt#examples)




## CFG Scale/Denoising strength Fitting/noise reduction

**CFG Scale**

The higher the scale, the more faithful the program is to the prompt, and the better the fit.

**Denoising strength Reducing noise**

Denoising strength` determines how well the algorithm retains the content of the image, reducing the effect on the image style, but also weakening the img2img capability. The higher the value, the less the AI will refer to the original image (and increase the number of iterations).

A low `denoising` means that the original image is corrected, while a high `denoising` is not as relevant to the original image. Generally speaking, the threshold is around 0.7, above which it is largely irrelevant, and below 0.3 it is slightly modified.



## Prompt editing

Prompt editing allows you to start sampling one picture, but in the middle swap to something else. The base syntax for this is: `[some1:some2:num]`

`[fantasy:cyberpunk:16]` to replace `fantasy` with the `cyberpunk` tag from step 16 onwards

`[to:when]` adds `to` to the prompt after a fixed number of steps ( when)

`[from::when]` Remove `from` from the prompt after a fixed number of steps ( when)

![sample_Gradient](https://user-images.githubusercontent.com/75739606/197822841-f7323afa-8c6a-46a2-a8e2-a1c457bb31d5.jpg)
<!--
![sample_Gradient](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/sample_Gradient.jpg)
-->


## Prompt from images

By default the program will include the prompt, parameters and model information in the image. For uncompressed original images we can drag the file into the `PNG Info` tab for the prompt (Token) view.

Or use the [Online Tools](https://spell.novelai.dev/) to view it.


## Inverse prompt

Relevant parameters can be extracted from the images, which may not be accurate.

[LenKiMo_Bot](https://t.me/LenKiMo_Bot)

[DeepDanbooru](https://github.com/KichangKim/DeepDanbooru)


## Search Engines

[PIXAI](https://pixai.art/)

[StableDiffusion_Show](https://t.me/StableDiffusion_Show)

[cyan_ai_sese](https://t.me/cyan_ai_sese)

[LEXICA搜索引擎](https://lexica.art/?q=Miku)


## Prompt matrix

Separate multiple prompts using the `|` character, and the system will produce an image for every combination of them.
For example, if you use `a busy city street in a modern city|illustration|cinematic lighting` prompt, there are four combinations possible (first part of the prompt is always kept):

* `a busy city street in a modern city`
* `a busy city street in a modern city, illustration`
* `a busy city street in a modern city, cinematic lighting`
* `a busy city street in a modern city, illustration, cinematic lighting`


`cat :2 | dog` that is, a dog that is more like a cat

## Highres. fix

A convenience option to partially render your image at a lower resolution, upscale it, and then add details at a high resolution.

By default, txt2img makes horrible images at very high resolutions, and this makes it possible to avoid using the small picture's composition. Enabled by checking the "Highres. fix" checkbox on the txt2img page.


## Prompt's working model

The scientific principle of incantation.

![prompt_draw](https://user-images.githubusercontent.com/75739606/198675128-c2c849d0-d024-468b-80c4-374f13e933e3.png)
<!--
![prompt_draw](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/prompt_draw_fix.png)
-->
>By RcINS

In the program, the parsing of the prompt is handled by CLIP

[prompt_parser for WebUi](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/prompt_parser.py) implements Prompt editing etc.

CLIP cannot handle Chinese, Chinese characters are broken up.

**About the weighting implementation**: weighting increases usually take up a prompt position.


**About Prompt editing**: At a given Step, the WebUi program will replace the corresponding prompt to achieve a Prompt editing effect.

And so on.

The WebUi prompt syntax is converted into a prompt of the corresponding time, which is then passed to Ai via embedding.



## Prompt conflict (hint word)

For example, if `loli` contains more styles, the single `loli` tag within the spell should be removed when the user wants a specific style.


## Step

More iterative steps may result in better generation, more detail and sharpening, but will result in longer generation times. In practice, the difference between 30 and 50 steps is very small.

Too many iterative steps may also be counterproductive, with little or no improvement.

When performing img2img, weaker denoising normally requires fewer iteration steps (this is how it works).

You can change the setting in the settings to allow the program to perform exactly the number of iteration steps specified by the slider.

## Samplers

Currently good ones are `eular`, `eular a`, more subtle, and `Ddim`.

Recommend `eular a` and `Ddim`, **newcomers recommend `eular a`**

`eular a` is creative and different steps can produce different images.
PS: too high a step count (>30) will not give better results

`DDIM` converges quickly, but is relatively inefficient as it requires many steps to get good results, **suitable for use in redrawing**

`LMS` and `PLMS` are derivatives of `eular` and use a related but slightly different method (averaging over several steps to improve accuracy). About 30 steps gives stable results

`PLMS` is an effective LMS (classical method) that can better handle singularities in the structure of neural networks


`DPM2` is a fantastic method that aims to improve DDIM by reducing the number of steps to obtain good results. It requires two denoising runs per step and it is about twice as fast as DDIM. But if you're experimenting with debugging cue words, this sampler doesn't work very well

`Euler` is the simplest, and therefore one of the fastest

from [discussion](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)

and [wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#attentionemphasis)


- 举个例子


|预览一|预览二|
|--|--|
|<img src="https://user-images.githubusercontent.com/22421310/187063145-3d4f16d7-7bd6-4804-be1c-acf228ed2507.jpg" width="400" alt="效果">|<img src="https://user-images.githubusercontent.com/75739606/197824518-f68188a3-0572-4b52-8fe7-289b6d7b640b.jpg" width="400" alt="效果">|

>不同 step 和 采样器 的不同效果



## Seeds

The actual seed integer is not important. It simply initialises a random number generator that defines the starting point of the diffusion and is a random initial value.

A good seed can perform things like composition and colour in various cues, samplers and CFGs. But it is of limited use now.

With the same Step , cfg, Seed, prompts, producing essentially the same picture!

The same seeds will produce the same picture in the same model and back-end implementation, keeping all parameters the same. Depends on the other parameters.

But note that **different graphics cards may cause unexpectedly different results** (e.g. things like accuracy)

The 10xx series looks so different from all other cards, see [here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3873467)



## Extensions

Extensions are a more convenient form of user scripts.

Extensions all exist in their own subdirectory inside the `extensions` directory. You can use git to install an extension like this:

```
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients extensions/aesthetic-gradients
```

This installs an extension from `https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients` into the `extensions/aesthetic-gradients` directory.

Alternatively you can just copy-paste a directory into `extensions`.

For developing extensions, see [Developing extensions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Developing-extensions).

**See a list of all extensions at [here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts#stylepile)**
