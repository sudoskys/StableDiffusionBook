# Practical Guide

This short practical guide will give you a quick overview of how to adjust the parameters appropriately to achieve the desired effect.

Currently, Ai does not handle details and multiple characters well, and the light and shadow structure will be messed up if handled one by one. If retouching, it is better to redraw. But it is practical to draw backgrounds and do previews.

**The core logic is to narrow the range of pre-selected data, adjust the balance between negative and positive prompts, and use part of the grammar to control the intensity of the prompt word effect and the process of effect.**


## Scene table

Viewed vertically

|characters|scenes|data-limited|events|
|----|------|-----|-----|
|expressions| | |expressions
|Hair|Wide and Narrow Selection|Paint Type|Abbreviations|
|eyes|lighting options|evaluation qualification|sfw/nsfw|
|clothing|background subjects|associative elements| Production Company |
|state|character event|  3D/2D  | Artwork and Brand   |
|Pose position ||||
|Lens position ||||


## Painting soldering!

With [AiPhotoShop - the online tool for infinite extents of canvas](https://www.painthua.com/) you can "weld" your work continuously, very conveniently. Just add the `--api` parameter to the start command and open the web page to use it.

[Github](https://github.com/BlinkDL/Hua)


## Curbing style pollution

Introduces how to generate images in WebUi with the subject as the focus, rather than the style of the drawing.[^12]

To get the desired result, we change `style` to `[style:10]` 

In Automatic 1111's WebUi cue word method, [x:10] tells stable diffusion until the tenth step before rendering the cue word.

By the time 10 steps are reached, a basic image has been formed, so styling pollutes the main component recklessly, instead acting like an IMG2IMG (which you can think of as a form of Img2img) over the existing image

You can experiment with this number depending on how many overall steps you've done. If you change the format to [x:0.2] (i.e. a number below 1) then it becomes a percentage, [x:0.6] tells SD to wait until 60% of the total number of steps before implementing x, and so on.


## The secret of diagram to diagram conversion

Whether it is 3D (3D models like DAZ) or line drawings, AI only recognises **colour**, not lines, and colour directly determines the result of a diagram to diagram conversion.


## Size selection

It should not be linked to picture quality; size affects the subject to some extent, as it potentially represents the category chosen (e.g. vertical figures, horizontal landscapes, small resolution emojis mostly).

Picture quality can be manipulated using the Super Score guide.


## Perspective

[Recommended use of terms contained in Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

![shot](https://user-images.githubusercontent.com/75739606/198682933-25c1b2df-c573-44ea-aba7-2df4609299e5.png)
<!--
![shot](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/shot.png)
-->
>citing an image from the Japanese Wiki, author unknown


## Associated

The core logic of cross-domain terminology is to **narrow the scope of the specified data** and start with **platforms, domains, events** outside of the screen content to enhance the effect.

Yes! You can use [Film Glossary](https://www.owlnet.rice.edu/~engl377/film.html) [FILM GLOSSARY Photography Terminology](http://userhome.brooklyn.cuny.edu/anthro) in the prompt /jbeatty/COURSES/glossary.htm), [Cinematic techniques](https://en.wikipedia.org/wiki/Category:Cinematic_techniques), and painting terms (types) to control the basics.

Crossover! You can even use various thrill-sport terms to produce some special effects .... such as airborne

for example, depth of field, aperture, composition, camera position, elements of movement, [a Chinese introduction to the glossary of artistic photography](https://gallerix.asia/pedia/photography-glossary/)

**BUT** this effect may have the added effect of introducing data into the picture that you don't want to see in the style (e.g. live action** rather than **but secondary). Get the measurements right. Add Step and style cues as appropriate to improve.

You can also use **platform names** to limit the scope of the dataset, for example, terms like pixiv.

**extended reading**

[Recommended use of terms contained in Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

[Danbooru tag group](https://danbooru.donmai.us/wiki_pages/tag_groups)

[Useful film terms ](https://en.wikipedia.org/wiki/Category:Cinematic_techniques)

[Types of shots](https://www.bhphotovideo.com/explora/video/tips-and-solutions/filmmaking-101-camera-shot-types) 

[TV terms](https://en.wikipedia.org/wiki/Category:Television_terminology) 

[Types of photography ](https://en.wikipedia.org/wiki/Category:Photography_by_genre)

[Photographic terms](https://zh.wikipedia.org/zh-cn/%E9%AB%98%E9%80%9F%E6%91%84%E5%BD%B1) 

[Extreme sports](https://en.wikipedia.org/wiki/Extreme_sport)

[Sports_by_type](https://en.wikipedia.org/wiki/Category:Sports_by_type)

[Art of composition](https://en.wikipedia.org/wiki/Composition_(visual_arts))


## Iterative sketches [^8]

Here's a discussion on how to optimise **hand sketches** by Ai drawing, *note not anime girls*.

In the first iteration you don't need too many Steps, the CFG can be very low (for better diversification results) and Denoising should be around 0.3-0.4 if you don't want to lose the sketch completely.

In the final iteration, increase the Steps and Denoising strength (but no more than 0.8, otherwise the image will be corrupted, especially at greater than 512*512) see [here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues /2213#issuecomment-1274137775), while increasing the CFG and size as needed.

You can always fix the hints (add or remove details that appear) and try a different sampler.

Also, shouldn't you use a fixed and unchanging seed for the initial generation?

If you provide a seed (instead of a random -1) your image will soon become oversaturated, oversharpened, overpixelated ..... Of course you can use a fixed seed if you want to fine tune it.


## Facial expressions Control

We can control the expressions in the picture using the Face Text!

:-) smile :-( unhappy ;-) wink :-D happy :-P tongue out :-C very sad :-O surprised open mouth :-/ suspicious

Only western face characters are supported, for details see [Danbooru Face Characters section](https://danbooru.donmai.us/wiki_pages/tag_group%3Aface_tags) or [Wikipedia](https://zh.wikipedia.org/wiki/%) E8%A1%A8%E6%83%85%E7%AC%A6%E8%99%9F%E5%88%97%E8%A1%A8?oldformat=true)


## Multi-figure/single-figure

Answer: sketching + IMG2IMG

Wide paintings of single figures are best sketched and coloured to determine the subject of the picture.

Multiple figures determine the number of figures, best to use sketches/coloured 3d arrangement + figure generation.

More than three figures makes it difficult to control the effect

Maybe There are no models for images with more than 6 people...


## Make a hand fix

Feed the image into inpaint, use roughly the same cue wording, put the cue about the `hand` in front, set the noise reduction depending on how much you want it to change (if you just want the hand to be more complete, tune it to 0.25 or less), then leave the steps the same as CFG and txt2img gen.

Or just mask the hand, fix it at full resolution, greatly reduce the padding (it uses the surrounding pixels to create the context, but is just re-creating the hand) and only hint at hand issues (detailed hand depiction etc)

The higher the CFG, the more it matches the cue words, and the higher the noise reduction the more it deviates from the original image.


## Identical figures & differentiation

Advanced Img2Img related content is required, the best way to do this is to prepare a 3D master model with colours and then this will ensure basic consistency.

You can also use a lot of cue words to limit the content of the character, come up with many sheets and pick the pieces that work.

If it's an expression or a background, you can use the repainting technique from the advanced tutorial.

If you want some examples of differencing, [5CH Japanese Wiki](https://seesaawiki.jp/nai_ch/d/%c7%ed%a4%ae%a5%b3%a5%e9%a5%c6%a5%af) provides an example.


## Reproduction

If you want to reproduce it on the generation, you need the same `seed`, cue words, and other parameters (even size). However, if the other side uses stylisation, then you can hardly reproduce it.

--Self-deceptive reproduction: image to image, noise reduction to below 0.3...--


## Frivolous details

- Emoticons

emoji(💰💶💷💴💵,🎊🪅🪄🎀,👩 🚀) Emoticons are also available and **very accurate**, I have tested them many times and Ai can recognise them.

[emoji-list](https://unicode.org/emoji/charts/emoji-list.html)


- Anime style

`anime colouring` will work well, like an anime screenshot. (If you encounter black and white, you need to specify the colour of the character parts)

<iframe width="560" height="315" src="https://www.youtube.com/embed/hGATDT-Y37g" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[some_art_styles_that_i_tried](https://www.reddit.com/r/StableDiffusion/comments/yllj3r/some_art_styles_that_i_tried/)


### Optimised artwork

- Rainbow messy pictures

If you are outputting chaotic rainbow coloured images, it is possible that the generation resolution has been set too low.

- Wavy noisy picture

Dense waves or patterns may be because the weights are set too high.

- Keyed image?

The rest of the background of a buttoned image will be processed as black.

If you need a background, the results are generally not very good and it is recommended to fill in the colours before manipulating them.

- Rough noise

Not enough detail, characters look like rice cakes. Needs proper enhancement step.



## Designing with Ai

- Backgrounds

Once you get a slightly more saturated photo, you can process it in AI and use it as a background.

Use `anime coloring` to enhance the anime style.

- Icons

[Look at this](https://www.bilibili.com/video/BV1384y1B7MX)

- Emotion packs

The emojis work really well. The `sticker` with the western face script is very well done!

The documentation writer has tried the following prompt and it produces high quality emojis.

Don't ask for quality prompt words for stickers.

**positive vocabulary**

```
1girl,sticker,Chibi,Q,
white background,
```
`masterpiece,best quality` may or may not be added, if quality is not enough

`lowres` can be added or not, add if not vintage enough

`Chibi` controls the character to be Q (big head, small body), add if not Q enough

`sticker` is to specify the style as a social media sticker, required

**negative vocabulary**

```
bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, jpeg artifacts, signature, watermark, username, blurry, bad feet
```

low quality can be added or not, if you want to be more flat, please add `3D` to negative hints.

Size required square.

- Character design

<iframe src="//player.bilibili.com/player.html?aid=559362671&bvid=BV14e4y1U7r9&cid=869144379&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
>BV14e4y1U7r9



[^4]:[Paper朱整理优化方法](https://pan.baidu.com/s/1VWr7OLvAbu1KIoTPEs2wwQ?pwd=y8lk)

[^5]:[参数图](https://m.weibo.cn/status/4823585938735546)

[^6]:[SD金矿](https://rentry.org/sdupdates#hall-of-fame)

[^7]:[风格模型训练](https://www.bilibili.com/video/BV1ae4y1S7v9/)

[^8]:[迭代草图](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2473)

[^9]:[交替单词](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^10]:[角色与画风tag训练十问](https://www.bilibili.com/video/BV1xt4y1F7Y2/)

[^11]:[WebUI即将引入重磅更新，大幅提升图像品质](https://www.bilibili.com/read/cv19102552)

[^12]:[a_simple_method_explained_in_the_comments_to](https://www.reddit.com/r/StableDiffusion/comments/ygyq2j/a_simple_method_explained_in_the_comments_to/)