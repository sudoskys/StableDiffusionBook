# Getting Started Painting

>Todo
>Upscaler 2 的预期用途 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3749
>


This section will introduce the parameters and related SDWebUi resources. If you can draw, the results will be more stable and considerable.

## Workstations

The toolbox that you can see when you open the page

### Reference

* [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups)

* [MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)

* [List_of_imagecomposition](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

* [ist_of_locations](https://danbooru.donmai.us/wiki_pages/tag_group%3Alocations)

* [List_of_attire](https://danbooru.donmai.us/wiki_pages/tag_group%3Aattire)

* [List_of_Aesthetics](https://aesthetics.fandom.com/wiki/List_of_Aesthetics)

* [List_of_colors_by_shade](https://en.wikipedia.org/wiki/List_of_colors_by_shade)

* [emoji-list](https://unicode.org/emoji/charts/emoji-list.html)

### Tool

* [QuickShare-pastebin](https://paste.rs/web)

- **[AiPhotoShop - the online tool for infinite extents of the canvas](https://www.painthua.com/)**

- [NAI<=>WebUi prompt word weight conversion Telegram bot](https://github.com/sudoskys/M2NM2NBot)

- [NAI's prompt Tokenizer](https://novelai.net/tokenizer)

#### Help design

* [Pic2Prompt0](https://www.latentspace.dev/)

* [Pic2Prompt1](https://magicstudio.com/pic2prompt/editor)

* [Pic2Prompt2](https://replicate.com/methexis-inc/img2prompt)

* [prompthero](https://prompthero.com/)

* [Prompt Search ](https://www.ptsearch.info/home/)

* [Prompt Search2](https://pagebrain.ai/promptsearch/?q=girl&page=1)

* [midjourney haha](https://www.midjourney.com/showcase/)

* [Social Generative AI](https://histre.com/integrations/generative/)

#### Processing

- [PNGINFO online viewing tool](https://spell.novelai.dev/)

- [Real-ESRGAN noise reduction and resolution enhancement tool](https://github.com/xinntao/Real-ESRGAN) 

- [DeepDanbooru by image recognition hints](https://github.com/KichangKim/DeepDanbooru)

### Reference works

* [arthub](https://arthub.ai/)

* [NAI-NOVELAI.IO ](https://novelai.io/)

* [NAI-Pixai](https://pixai.art/)

* [LEXICA](https://lexica.art/)

* [Krea](https://www.krea.ai/)

* [largest library of AI-generated images](https://libraire.ai/)

* [Top-sd-artists](https://www.urania.ai/top-sd-artists)

* [artiststostudy](https://artiststostudy.pages.dev/)

* [midjourney haha](https://www.midjourney.com/showcase/)

The tag reference should use [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups) and [MidJourney-Styles-and-Keywords-Reference](https:// github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)

For brushstrokes/painting tools/styling/painting techniques tag group see [Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

If you are used to reading Japanese tag references and techniques, you can search in [Japanese Wiki](https://seesaawiki.jp/nai_ch/d/%be%ec%bd%ea%a1%a6%c7%d8%b7%ca).

Later chapters covers: imitating style, specific shots, adding effects, fine-tuning and fine-tuning, PS grafting out of the picture, specific poses through 3D, redrawing, iteration

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

## How to write prompts

This is a general guide, the content is largely generic, there may be exceptions, please read the corresponding sections for the characteristics of different applications.

- Vocabulary
[^2]
Words may consist of one or more symbols. Common words are usually resolved as single marks with a common meaning or a small number of meanings. Misspellings and uncommon words are parsed into multiple tags that derive meanings from a variety of different words, and often inconsistent meanings.

Vowel changes are more likely to map you to a different unrelated word, with a more unique and stronger meaning and impact.

So 'bank, bankk bank' is likely to be interpreted as a close synonym of 'bank'. But bonk - has a completely different meaning.

- Case of word

Clip's tagger lowercase all words before tagging them. Other models such as BERT and T5 tokenize capitalized words differently from uncapitalized.

- Length

Except for WebUi, the prompt is not infinite due to GPT-3 model limitations, the possitive token is between 75-80 and the content after 75 characters is truncated.

- Special cases

Capitalization does not matter.

Extra spaces at the beginning and end of your prompt are simply discarded. Additional spaces between words are also discarded.
    
Underscores ("_") are not converted to spaces.

- Natural Language

Natural language can be used directly, WebUi(SD) has natural language processing capabilities (English sentences), and can also use face characters and emoji, as well as Unicode characters (e.g. Japanese characters).

At least some Unicode characters that are alternative versions of Latin characters get mapped to regular Latin characters. Full-width Latin characters as they're used in Japanese (e.g. ＡＢＣ) are confirmed to be converted. French accents (e.g. é and è) and German umlauts (e.g. ä and ö) are not mapped to their regular counterparts.

Human language is arbitrary and imprecise (except for emoji, which is exceptionally precise because it has only one character), and it has never evolved to the point where it can describe spatial information in detail.[^2]


Stable Diffusion is a latent Diffusion model involving latent space. But latent, by definition, means unobservable. 

Many gamblers follow their particular patterns of certain ritualistic behaviors believing that such patterns will increase the chance of a favorable outcome. Not only this type of false positive pattern doesn't work in reality, but it will also continue to reinforce and manifest itself, nudging a person further and further in the wrong direction.

- Punctuation

Use it. Separating keywords by commas, periods, or even null characters ("\0") improves image quality. It's not yet clear which type of punctuation or which combination works best - when in doubt just do it in a way that makes the prompt more readable to you. 

- Word order

[^2]
It appears that VAE uses a statistical method known as Bayes' theorem. When calculating the goings-on of tokens, the first few words seem to anchor the distribution of the remaining word tokens in the potential space.

The earlier tokens have more consistent positions, so the neural network is more likely to predict their relevance. In Bayesian inference, the first token or evidence in the matrix is important because it sets the initial probability condition. But later elements simply modify the probability conditions. Thus, at least in theory, the final token should not have more influence than the previous token.

But the way the parser understands things is opaque, so there is no way to **exactly** know whether lexical order has an "anchor" effect.


### Movement and Poses

If possible, choose prompts that are associated with only a small number of poses. 

A pose in this context means a physical configuration of something: the position and rotation of the image subject relative to the camera, the angles of the joints of humans/robots, the way a block of jello is being compressed, etc. The less variance there is in the thing that you're trying to specify the easier it is for the model to learn. 

Because movement by its very definition involves a dramatic change in the pose of the subject, prompts that are associated with movement frequently result in body horror like duplicate limbs. Also because human limbs and especially human hands and feet have a lot of joints they can assume many different, complex poses. This makes their visualization particularly difficult to learn, for humans and neural networks alike.

TLDR: good image of human standing/sitting is easy, good image of human jumping/running is hard. 

### Specificity

For the training of neural networks the quality of features is important: the stronger the connection between the inputs and the outputs is, the easier it is for a neural network to learn the connection. In other words, if a keyword has a very specific meaning it is much easier to learn how it connects to images than if a keyword has a very broad meaning. In this way, even keywords that are used very rarely like "Zettai Ryouiki" can produce very good results because it's only ever used in very specific circumstances. On the other hand, "anime" does not produce very good results even though it's a relatively common word, presumably because it is used in many different circumstances even if no literal anime is present.

Choosing specific keywords is especially important if you want to control the content of your images. Also: the less abstract your wording is the better. If at all possible, avoid wording that leaves room for interpretation or that requires an "understanding" of something that is not part of the image. Even concepts like "big" or "small" are problematic because they are indistinguishable from objects being close or far from the camera. Ideally use wording that has a high likelihood to appear verbatim on a caption of the image you want.


### Attention / Emphasis

There is a lot of confusion around attention, meaning ways to increase or decrease the weight of specific parts of a prompt.
Some people assert that putting a keyword in round brackets  increases its effect
while putting a keyword in square brackets decreases its effect;
Using more brackets supposedly results in a stronger change.
However, others can frequently **not** reproduce this effect in their own prompts.

As it turns out the reason for the discrepancy is that  *different scripts process brackets differently* .
[This fork of webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) for example explicitly processes brackets while
[this fork](https://github.com/hlky/stable-diffusion-webui) would only get the effect of brackets that the model implicitly learned from the training data(a different syntax is used instead).

For this reason: **make sure to check whether the syntax of a prompt that you copy from someone else matches the syntax of the script that you use.**

In a [quantitative analysis](https://github.com/JohannesGaessler/stable-diffusion-tools/tree/master/emphasis) square brackets did not have a consistent effect unless explicitly processed.

The repetition of a certain keyword seems to increase its effect regardless of the specific script that is being used.

Worth noting, the impact of any single token will be diminished the more tokens exist in your prompt. You'll also notice, because of this, that styles will fade out when adding new tokens. I strongly suggest increasing the token strength on style words as your prompt length increases, in order to maintain a consistent style, particularly in certain models.[^2]


#### dyes

Each prompt is like a dye, they have different "affinities" and if the more common prompts, such as `loli` (placed alongside other prompts) have a greater impact than other prompts.

For example, if you want to generate anime images and use the star `startrail` tag, there will be more star elements from real photos than the anime stars you expect to appear.

Many words just don't carry the same weight in terms of benchmarks, so it's important to make reasonable adjustments to the effect.


### Image size

Image size obviously affects content, if the height of the image increases it is more likely to produce an individual or separate thing. If the image width is increased, it is more likely to produce multiple people or landscapes.

## Extension

[WebUi: Adding custom backend APIs for Krita plugins and more](https://github.com/Interpause/auto-sd-paint-ext)



[^1]:[installgentoo wiki](https://wiki.installgentoo.com/wiki/Stable_Diffusion)

[^2]:[demystifying_prompting_what_you_need_to_know/](https://www.reddit.com/r/StableDiffusion/comments/yjwuls/demystifying_prompting_what_you_need_to_know/)