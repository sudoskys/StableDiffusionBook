# Prompts Engineering

This is a small guide to further explain the theory of index pages.

This article is based on WebUi, if you use other Ui, please be aware of compatibility.

If you are a user of NAI, please read the [Official Docs](https://docs.novelai.net/image/promptmixing.html) additionally.

## Getting Started

The perception of writing Prompt is that the more samples that meet the expectation within a limited data range, the more the result meets the expectation.

### What to write?

Ai is understood by code input, defined by the Tokenizer, and you can test the relevant code of the UI to get the actual subword input.

- [NAI's prompt Tokenizer](https://novelai.net/tokenizer)
- [WebUiTokenizer](https://github.com/AUTOMATIC1111/stable-diffusion-webui-tokenizer)

**Word**

Common common words, preferably with well-known tags that can be found at the dataset source site (e.g. Danbooru). The style of the word should match the style of the image (e.g., you should not include the word `Leshanda Buddha` in the cue when reasoning about anime illustrations), otherwise confusing styles or noise will occur.

> For some anime models trained based on Stable Diffusion, it is better to use the tags that can be found in Danbooru.

**Natural Language**

Prompt can also use natural language directly, the language can be English, Japanese, special symbols or some Chinese, as determined by the dataset.

The accuracy of natural language depends on Clip's word separation, so do not use it if you are looking for precise results.

Avoid conjunctions like `with` or complex syntax when using natural language, they are redundant.

**emoji**

You can use the western face character `(^_^)`, emoji 🌻. emoji works very precisely as a one-character "word".

- Emoji has an impact on composition, for example `💐☺️💐`.
- Emoji performs well in terms of semantic accuracy because it is only one character.
- [Emoji reference](https://unicode.org/emoji/charts/emoji-list.html)

**Emoji**

For **models using Danbooru data**, we can use face characters to control emojis!
```
:-) smile :-( upset ;-) make eyes :-D happy :-P stick out tongue :-C very sad :-O surprised open mouth :-/ suspicious
```

- Face characters have influence in composition.

- Only western face characters are supported, see [Danbooru Face Characters section](https://danbooru.donmai.us/wiki_pages/tag_group%3Aface_tags) or [Wikipedia](https://zh.wikipedia.org/wiki/%E8%A1%A8%) for details E6%83%85%E7%AC%A6%E8%99%9F%E5%88%97%E8%A1%A8?oldformat=true)

**spaces**

- The small number of spaces before and after the comma does not affect the actual effect.

### How do I write it?

First think about what you want to draw, e.g. theme, appearance, mood, clothes, pose, background A category, then refer to the dataset tagging table (if available, e.g. Danbooru, Pixiv, etc.).

Then combine the similar cue phrases you want, using the English semi-colon `,` as a separator, and put these in order from most important to least important.

```
(quality), (subject)(style), (action/scene), (artist), (filters)
```
`(quality)` represents the quality of the image, e.g. `low res` is used in combination with `sticker` to "leverage" more datasets, `1girl` is used in combination with `high quality` to get high quality images.

`(subject)` represents the subject of the image and anchors the content of the image, which is an essential part of any cue. `(style)` is the screen style, optional.

`(action/scene)` represents the action/scene, which describes what the subject did where.

`(artist)` represents the artist's name or the name of the company that produced it.

`(filers)` represents some details, additions. You can use artist, studio, photography terms, character names, styles, effects, etc.

**Artistic style**

You can create images with special effects or specified painting styles by specifying style keywords.

[NovelAI usage tutorial and spell classroom](https://space.bilibili.com/8612008/channel/collectiondetail?sid=787691)

[Test records of the Mannequin Classroom](https://www.yuque.com/longyuye/lmgcwy)

[Stylization: 32 kinds](https://www.bilibili.com/video/BV1TP411N71t/)

In addition, you can also train style models to train your ideal style, see the `Model Training` chapter for more details.

More:[为文字转图像 Ai 提示编写指南：A Guide to Writing Prompts for Text-to-image AI](https://docs.google.com/document/d/1XUT2G9LmkZataHFzmuOtRXnuWBfhvXDAo8DkS--8tec/edit#)

### How long to write?

**Writing Length**

Due to the limitations of the GPT-3 model, the prompt is not infinite, and the positive token is usually between 75 and 80, with the content truncated after 75 characters. So the prompts should not be too long, more than 100 is a risk of failure.

**WebUi**

In WebUi, you **can** write prompts with more than 75 words. webUi overcomes this limitation by grouping prompt words. When the prompt exceeds 75 `tokens` (e.g. 150 `tokens`), WebUi will group the prompt words and submit multiple groups of 75 `tokens`. The token only has context for other content in the same collection.

But this leads to a problem: there may be `bule hair` at the border between the first and second group, the token `blue` will be in the first group and `hair` will be in the second group. This leads to inaccurate results, because `bule hair` is split up unreasonably!

To solve this problem, the new version adds an option `Increase coherency by padding from the last comma within n tokens when using more than 75 tokens`. This setting lets the program try to mitigate this situation by finding if there is a last comma in the last N tokens, and if so, moving everything that passes through that comma together to the next collection.

```
    There are entries for `... , Comma,blue hair,PADDING,... `
    
    The 75th word is `blue`
    
    Before using this option
    
    Set 1:{[74]=Comma, [75]=blue}, Set 2:{[76]=hair, [77]=PADDING}
    
    After using this option
    
    Set 1:{[074]=Comma, [75]=PADDING}, Set 2:{[76]=blue, [77]=hair}
    
    If your hint is less than or equal to 75 tokens, no grouping will occur.
```

!!! tip
    It is a good habit not to stack prompts, but if you do have a lot of content to write, you can increase the number of `steps` appropriately.

### Order

According to the image inference process, the order in which the prompts are placed can be considered a priority, with the preceding Prompt anchoring the main content of the screen. This explains why I emphasized the order of Prompt in the above.

![Description of image generation](https://jalammar.github.io/images/stable-diffusion/stable-diffusion-image-generation.png)

> thanks https://jalammar.github.io/illustrated-stable-diffusion/

If you use WebUi, WebUi renders long strings of Prompt in groups of 75 Tokens.

### Prompt conflicts

**classification conflicts**

    If you want a sticker, the sticker will definitely not be labeled `masterpiece, best quility,` but `normal quality` in the dataset.

    Similarly there is also pixel work, when writing prompt words, you should remove some conflicting hints.

**Style Conflict**

    Let's say you want to get a `flatcolor` style illustration of children playing on the beach.

    Then using `loli` is not a good idea, as it comes with a standard uniform drawing style attribute that can greatly affect the result.

    Solution: lower the weight, replace the prompt.

**dimensional conflicts**

    Some anime models are trained based on the original Stable Diffusion and should be avoided as much as possible.

Please be careful.

## Prompt Editing

Through prompt word editing, we can clearly tell the model "how much salt should be put in the dish I ordered".

[Original implementation](https://github.com/bloc97/CrossAttentionControl)

### Manual Segmentation

Use `/` or `+` to force splitting of prompt words. [^13]

It should be noted that adjacent repeated symbols will be filtered. The effect of ```+++``` is equivalent to `+`. The correct way is to type + and / separately.

For example, it is also necessary to achieve the separation of 9 token lengths: [^13]

```markdown
# The following three items have similar effects, and do not interfere with subsequent words very much. Influence around 7%:
red eyes,/,/,/,/, blue dress (separated by 9 grids, the keys are also close together, recommended)
red eyes, /+/+/+/, blue dress (9 grid separation, minimum interference)
red eyes, , , , , , , , ,blue dress (separated by 9 spaces, there must be spaces between commas or the effect will be halved)

# The following are not recommended methods, which still have a 25% impact on subsequent words and will interfere with the style of painting:
red eyes++++++++++++++++++++ blue dress (also 9 grids, three + signs are judged as a token)
red eyes/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​/​ /​/​/​/​/​/​/​/​/​/​blue dress (same as plus sign)

* Tip-From [^13]
```

### Attention Control

Reading the theoretical basis, we know that the basic weight of each word is different, and our expectations for the weight values ​​of different prompts are also different. At this time, we need to use attention control to adjust the relationship between them.

| Ui | Syntax | Example | Weight Variations | Notes |
|:----:| :----: | :----: |:----: |:----: |
| WebUi | `(prompt:num)` | Emphasis: `((cat))` `(cat:1.4)`, Weaken: `(cat:0.2)` `[cat]` | ()-> 1.1 |` The range of num` is `0.1 ~:100`, if it is a decimal, take the proportion of the number of steps|
| Nai | `{}` | `{{{cat}}}` `[cat]` | {}-> 1.05 | |
| original implementation | +/- | `+cat` `-cat` | |  |

1. WebUi weight syntax is `(prompt:num)` or `((prompt))`, the enhanced weight is , and the enhanced range is `0.1 ~ 100`, if it is a decimal, it will take the proportion of steps. For example, emphasis: `((cat))` `(cat:1.4)`, weakening: `(cat:0.2)` `[cat]`

    ```
    a (word) - increase the weight by a factor of 1.1

    a ((word)) - Increases the weight by a factor of 1.21 (= 1.1 * 1.1), a multiplicative relationship.

    a [word] - reduce the weight by a factor of 1.1

    a (word:1.5) - increase the weight by a factor of 1.5

    a (word:0.25) - reduces the weight by a factor of 4 (= 1 / 0.25)

    a \(word\) - use literal () characters in the prompt
    ```

    !!! tip
        The weight increase usually takes up one token bit. There is no need to add too many parentheses when the token bit is tight.

        Too many parentheses will cause characters to be eaten by the program, `a ((((farm))), daytime` will become `a farm daytime` without commas.

        WebUi For prompt words with brackets such as `a (word)` **Please use `\` characters to escape to `a \(word\)`**, this is suitable for Tags with brackets, to prevent unwanted enhancements Effect.

2. NAI uses `{}` for enhancement, and weight cannot be specified. For example `{{{cat}}}` `[cat]`

   NAI does not allow weights to be specified individually, but supports mixed weights such as `cat:1|happy:-0.2|cute:-0:3`, `(cat :2 | dog)` is a dog that is more like a cat.

   **Conversion relationship**

   [word] of NAI = WebUi(word: 0.952)(0.952 = 1/1.05)
    
   {word} of NAI = (word: 1.05) of WeUi
    
   NAI curly braces weight 1.05/piece, WebUi circle weight 1.1/piece

3. Reduce the weight, use `[]` or `(word:0.952)` alone. NAI can only use `[]`

!!! tip "Do not confuse"

    Prompt syntax for NAI and WebUi **not universal**

    Because NAI is using a pre-September 2022 implementation of WebUi, the weight augmentation syntax is the old `{}` and the new WebUi changed to `()`.
    
    NAI does not support specifying weights for prompt words.

    If you want to convert the weight conveniently, you can use [conversion service](https://t.me/M2NM2NBot)

??? tip "How It Works"
    Each word has an associated vector of 768 values ​​that "points" in the direction of the concept (in a 768-dimensional space).

    If you scale this vector, the concept becomes stronger or weaker.
    
    SEE [Here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2905)

![example image missing]

### Prompt word alternation

Alternate prompt [^7], Prompt Mixer. This technology is implemented by the UI. [^9]

```
[alison brie|emma stone|elizabeth olsen|scarlett johansson|anne hathaway|emma roberts], still film
```

It can create hybrids of animals, people or styles, switching a hint every `step` (step-by-step ring rendering).

![example image missing]

### Prompt matrix Matrix

WebUi uses `|` to separate multiple keywords to mix multiple elements, and the program will generate an image for each combination of them. This technology is implemented by the UI.

For example, if you use `a busy city street in a modern city|illustration|cinematic lighting`, there are four possible combinations (always keep the first part of the hint):

- a busy city street in a modern city
- a busy city street in a modern city, illustration
- a busy city street in a modern city, cinematic lighting
- a busy city street in a modern city, illustration, cinematic lighting

![example image missing]

You can further add : x after the keyword to specify the weight of a single keyword.

**NAI**

In NAI stands for average weight mix (first half and second half).

`cat :2 | dog` is a dog more like a cat

![example image missing]

### Prompt Schedule

Prompt Schedule can change prompts in the process of reasoning about images, this technology is implemented by UI.

The syntax is `[from:to:when]`

where `from` and `to` are prompts, and when is a number defining how long in the sampling period the switch should occur.

- `[fantasy:cyberpunk:16]` means from the 16th step onwards, replace `fantasy` with `cyberpunk` tag

- `[to:when]` adds `to` to the prompt (when) after a fixed number of steps

- `[from::when]` removes `from` from the prompt after a fixed number of steps (when)

for example:

- `[fantasy:cyberpunk:16]` means from the 16th step onwards, replace `fantasy` with `cyberpunk` tag

- `[male: female: 0.0]`, means you asked to draw a female from the start.

Effect:

![sample_Gradient](https://user-images.githubusercontent.com/75739606/197822841-f7323afa-8c6a-46a2-a8e2-a1c457bb31d5.jpg)

<!--
![sample_Gradient](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/sample_Gradient.jpg)
-->

## Become an assistant engineer

These guides can take you a step further.

- [Test for NAI](https://github.com/6DammK9/nai-anime-pure-negative-prompt)

- [https://github.com/Maks-s/sd-akashic](https://github.com/Maks-s/sd-akashic)

- [https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)

[^9]:[alternate prompt](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/1733)

[^13]:[Nga's experience](https://nga.178.com/read.php?pid=654605407&opt=128)