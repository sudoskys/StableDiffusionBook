# Textual Inversion

<!--
VAE does not have a catastrophic effect on training.
-->

!!! tip
    Do not load VAE during training.
    
    If you don't want to load Vae, rename "xxx.vae.pt" to "xxx.vae.pt.disabled" or something else before starting webui.

    Check **Move VAE and CLIP to RAM when training hypernetwork. Saves VRAM.** The effect is to move VAE to RAM, but it will actually load.

![Textual_Inversion](https://user-images.githubusercontent.com/75739606/203366251-ae0ae585-8492-4970-ac9a-5ccf14a1897c.jpg)

## Apt/DreamArtist Training

TI is for Ai to understand "likes", and Apt is for Ai to understand "likes" and "dislikes".

So it's an efficient Textual Inversion

https://github.com/7eu7d7/DreamArtist-sd-webui-extension

[paper](https://arxiv.org/abs/2211.11337)

### A simple little guide
**DL (APT) preparation**
1. turn off --xformers
2. set Clip skip to 1
**Parameters**
1. forward, reverse word element tocken contrast value setting (from low to high due to character complexity `(3,6)(4,6)(5,7)(3,10)`)
2. learning rate `LR(0.0025,0.003)` Author's example uses `0.003`
3. `CGF = 3` 4.
4. aspect ratio `512:512` 5.
5. iteration steps `step = 8000 (LR = 0.003)`
6. updated experimental function EMA (positive)/EMA (negative) set to `0.97`
**Hint**
If generating anime images, the model uses the `animefull-latest 7g` model and the generation can be done with `anything`

>by DreamArtist s author

## Prepare the dataset

The dataset should be consistent in style and have the same concept of content. The more images, the better, if computing power allows. The content of the data can be illustrations, abstract paintings or emojis.

The quality of the dataset is demanding, if objects such as human figures appear in the background, they will be trained in. The training procedure is very sensitive.

## Requirements

At least 6GB of video memory, 12GB of video memory is required for comfortable use. Based on experimental data, 8GB of video memory should be selected for `-512x512` resolution; training with `-lowvram` and `-medvram` parameters is not recommended.

If you have enough VRAM, then using `-no-half --precision full` may prevent problems with overflow.

You can interrupt and resume training, but the optimizer state will not be saved, so this is not recommended.

## Setup instructions

Prepare 30 or more images of the target persona, each of which should be cropped to the same scale. (webui already supports cropping of rectangular images)

In the `Interrogate Options` setting, the `Interrogate: deepbooru score threshold` is the deepdanbooru (a model for extracting tags from images) tag confidence threshold. It is recommended to use 0.75, i.e. to retain results with a confidence level greater than 0.75.

For example, suppose the recognition result is

```
1.000 1girl
0.986 blush
0.981 long_hair
0.980 solo
0.918 japanese_clothes
0.912 rating:safe
0.798 bangs
0.795 wide_sleeves
0.731 blunt_bangs
0.637 long_sleeves
0.628 monochrome
```

The contents below `wide_sleeves` will be discarded.

`Interrogate: deepbooru sort alphabetically` is sorting Tag alphabetically, so we uncheck this because Tag has a big effect on the result.

`use spaces for tags in deepbooru` should be turned on.

`escape (\) brackets in deepbooru (so they are used as literal brackets and not for emphasis)` should be switched on.

Press `Apply setting` to save the settings.

## Create training

Open the `train` tab and create a new `embedding` model in the `Create embedding` tab.

`Number of vectors per token` is the width of the embedding, which depends on the dataset and can be set to 3 if there are less than a hundred.

The larger the width, the stronger the effect, but it requires tens of thousands of training rounds. According to the video by [usim-U](https://space.bilibili.com/99162407), setting 24 requires at least 300 high-quality pieces.

`Name` input box Enter the prompt you want the persona to appear in.

`Initialization text` You can enter `one girl` as a broad category, only one can be entered, and the character can be 1girl or 1boy, or a drawing style.

`Number of vectors per token` is the number of token bits this `embedding` will occupy, the more the better, but it will also reduce the number of token positions for other prompt.

New will create a pt file under `embedding`.

## Preprocessing

Open the `Preprocess images` tab.

Fill in the `Source directory` with your training images folder and only allow training images in it.

Fill in the `Destination directory` with the path where the images will be saved after preprocessing.

Select the image size for training, typically `512x512` for 8Gb graphics cards, larger is not necessarily better.

**Next there are four checkboxes**

Checking `Create flipped copies` will invert the image image to increase the amount of data.

Check `Use deepbooru caption as filename` to use deep learning to recognize Tag, and check to train `embedding` for NAI. **If you don't have this option, you need to add `-deepdanbooru` to the startup entry**

>Windows needs to add `COMMANDLINE_ARGS=` in the `webui-user.bat` line, or just `python launch.py --deepdanbooru`. (If the launch is stuck it is a network problem)

Checking `Use BLIP for caption` will add a caption to the filename using the BLIP model. Not really suitable for secondary images.

Check `Split oversized images into two` to split oversized images into two, not normally used.

Finally we check `Use deepbooru caption as filename` and `Create flipped copies`.

Click the button and wait for the processing to finish.

## Training

Training is a dynamic process!

In the `Train` sub-tab, select the model you want to train.

### Learning rate

`Learning rate` (hyperparameter: learning rate), the learning rate represents the rate at which information accumulates in the neural network over time, and this parameter greatly influences the speed at which training is affected. Generally, the lower the learning rate, the slower the learning (takes longer to converge), but generally the better the results.

Generally we set it to 0.005, but if you want to go faster, you can use 0.01 to speed it up. However, if we set it too high, the gradient descent will be too large to converge and will probably destroy the `embedding`, which will not be as effective as expected. If you set it too small, you will easily fall into a local optimum. Currently TI supports setting learning rates of `0.1:500, 0.01:1000, 0.001:10,000`, which are based on `learning rate:steps`, or `total steps*percentage` if the number of steps is decimal.

When training, you can start with a larger learning rate and then gradually reduce it by `0.1 - 0.02 - 0.005`, using the last best result for each test.

![CS231n](https://user-images.githubusercontent.com/75739606/197824268-273f074d-9622-41ad-9f1a-2980f180dbd9.png)
<! --
![CS231n](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/CS231n.png)
-->

### Parameters

`Log directory` is the log directory.

- Prompt template

`Prompt template file` is a text file with prompts, one per line, for training the model. The `textual_inversion_templates` folder in the directory explains what you can do with these files.

Refer to `style.txt` and `subject.txt` for training, e.g. `style.txt` for painting style and `subject.txt` for character training

Depending on the template file, you can use the following keywords in the file name and they will be replaced when processing.
```
[name]: name of the embedding
[filewords]: the name of the image file in the dataset
```

### Operations

`Preview prompt` Preview and generate a preview with this prompt when finished.
If it is empty, the prompt from the prompt will be used.

``Save a copy of embedding to log directory every N steps, 0 to disable``
Save a copy of embedding to log directory every N steps, 0 to disable

`Save an image to log directory every N steps, 0 to disable`
Save an image to log directory every N steps, 0 to disable

- Number of steps

`Max steps` determines how many `steps` are completed before training will stop.

A step is a single image (or batch of images, but batches are not currently supported) that is trained to the model and used to improve embedding. if you disable training and resume it later, the number of steps is retained.

For stylised models of character images, the recommended number of steps is 15000-40000

stylised model of the artist's drawing style, recommended number of steps is 40,000-80,000

!!! tip
    This is a reference, in fact 5000 and 7000 have also been successful.

    The key is the Loss rate. If the Loss does not decrease in 10 rounds, you can stop.

    If the Loss is greater than 0.3, the results are not very good

If too much is overfitting (which can be interpreted as the dead weight of Ai), keep an eye on it and stop if it is overfitting. If it doesn't work very well, go to an earlier model and continue training. **Keep tweaking** to find a good result.

`Save images with embedding in PNG chunks` is generating a pt file in the form of an image, which is very convenient for us to share the embedding.

Once everything is in place, click `Training` in the bottom right corner and wait.

The training is complete. If you have uninstalled VAE, rename the VAE weights back and restart the program.

## Other explanations

**[filewords]**

This represents the replacement of the [filewords] of the cue word template file as the name of the dataset file, which enables the insertion of **file names into the cue words**

1. By default, the file extension and all numbers and dashes at the beginning of the - file name are removed.

So this filename: `000001-1-a man in suit.png` will become the prompt text: `a man in suit`. The formatting of the text in the file name remains the same. 2.

2. The text in the filename can be changed using the `Filename word regex` and `Filename join string` options.

For example, using the word `regex = \w+` and the join string = `,`, the above file will produce the following text: `a, man, in, suit`.

The regular expression will extract the hint word from the text
`['a', 'man', 'in', 'suit', ]`

and places the concatenated string (',') between the words to create a text `a, man, in, suit`

You can also create a text file with the same filename as the image ( 000001-1-a man in suit.txt) and place the prompt text there. The filename and regular expression options will not be used.

**Move VAE and CLIP from VRAM when training. saves VRAM.**

Move VAE and CLIP from VRAM into RAM when training

This option on the Settings tab allows you to save some memory at the cost of slower preview image generation.

The result of the training is a .pt or a .bin file (the former is the format used by the original author, the latter is used as the diffusers library)

### subject_filewords.txt template

[^1]

Textual Inversion training cannot train things that are not in the model. It is also very sensitive to training photos.

If you don't get good results (failure to converge or crashing results). You need to replace the training data or use Dreambooth.

So how does the training work?

The vector being fed into the model is the training vector + the hint vector. You give it a hint, the hint is transformed into a bunch of vectors and fed into the model, the output is compared with the training image and the trained word vector is slightly corrected. This process is repeated during training.

Because the training vectors are not corrected using the content provided by the hint vector for training. So the document words should not contain features that belong to the content being trained.

If you have, say, a subject wearing a `black t-shirt` in all photos, you can effectively negate it from the training set by adding `black t-shirt` to the `filewords` of those images.

Unless you are trying to fix a photo, use `filewords` for style, not for subject.

<iframe src="//player.bilibili.com/player.html?aid=559085039&bvid=BV1ae4y1S7v9&cid=859894044&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>

## DreamArtist mini-guide

This content was written by konbaku yomu in [Chinese Community](https://t.me/StableDiffusion_CN/471273).

### Preparation

**training set** is **the most important**, because the DA characteristics of the least one shot can be refined, so I tested all the *original image + mirror image*. All the following tuning methods are not as important as the training set itself. ** training set is not selected well how to adjust the reference is useless ** .

The training set criteria are: frontal character (full body 0r half body) + high resolution + as simple as possible background + no special movements of the character itself (images containing yoga-like movements or elements of holding objects are not allowed, which will seriously affect the generalization and the formation of the image)

The basic parameters are set as follows:

- Close `--xformers`

- restart sd and set CLIP to 1 (default is 2)

- Use the original 7g anime model.

You can also do [local learning enhancement](https://t.me/StableDiffusion_CN/474128) by adding a grayscale map of the training set.

### Training

| **direction** | **reconstruction** | **learning rate**                        | **CFG** | **filewords read** | **cue word template** | **training set parameters** | **step range**                        | **EMA**                                                   |  |
|:-------------:|:------------------:|:----------------------------------------:|:-------:|:------------------:|:---------------------:|:---------------------------:|:-------------------------------------:|:---------------------------------------------------------:|:----:|
| character     | on                 | 3e-3 (unstable can be changed to 2.5e-3) | 3       | not recommended    | `subject.txt`         | default 512 can be          | 3000~6500 (depends on sample quality) | 0.97 (recommended by plugin)/0.95 (recommended by author) |      |
| style         | on                 | 5e-3                                     | 5       | recommended        | `style.txt`           | default 512 can be          | 1500~2000                             | 0.97 (plugin recommended)/0.95 (author recommended)       |      |

**About batch size and Accumulation steps**

Accumulation steps is equivalent to increasing the batchsize, but how much this setting, the training time will be several times more, the overall bs = (batch size * Accumulation steps), depending on their own graphics card to open, the default is 1 1

**Other**

1. the default word tocken is (3,6), according to the complexity of the role can be increased to (4,7) (5,8), the author even recommended (10,10)

2. when training characters loss > 0.2 basically direct failure, the probability of ghost map, stable loss should be less than 0.15, you can reduce the training loss by cropping the original background, the best way to improve the quality of the training set.

3. can try to increase the training stability by adding grayscale map (experimental).

4. plugin author's recommendation: you can change the cue word template file to style_filewords when training the painting style, the effect is good (experimental)

[^1]:[is_textual_inversion_salvageable](https://www.reddit.com/r/sdforall/comments/ykerg2/is_textual_inversion_salvageable/)