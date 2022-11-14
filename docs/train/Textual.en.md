
# Textual Inversion[^7]

[Official Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Textual-Inversion#training-embeddings)

!!! tip
    VAE does not have a catastrophic effect on HN training. To uninstall, rename "xxx.vae.pt" to "xxx.vae.pt.disabled" or another name before starting webui.

    Check **Move VAE and CLIP to RAM when training hypernetwork. Saves VRAM.** The effect is to transfer VAE to RAM, not to uninstall it.


## Apt/DreamArtist

TI is for Ai to understand "likes", while Apt is for Ai to understand "likes" and "dislikes".

So it's an efficient Textual Inversion

https://github.com/7eu7d7/DreamArtist-sd-webui-extension

## Prepare the dataset

The dataset should be consistent in style and have the same concept of content. If computing power allows, the more images the better. The content of the data can be illustrations, abstract paintings, or emoticons.


## Requirements

At least 6GB of VRAM, 12GB of VRAM is required for comfortable use. Based on experimental data, 8GB of VRAM should be selected for `-512x512` resolution; training with `-lowvram` and `-medvram` parameters is not recommended.

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


**The next four checkboxes** are


`Create flipped copies`

- When checked, this will invert the image image to increase the amount of data.

- `Use deepbooru caption as filename`

Check this box to train `embedding` for NAI. If you don't have this option, you need to add `--deepdanbooru` to the startup entry.

Windows needs to add it to `COMMANDLINE_ARGS=` in the `webui-user.bat` line, or just `python launch.py --deepdanbooru`. (If the launch is stuck it is a network problem)

- `Use BLIP for caption` Use the BLIP model to add captions to filenames. Not really suitable for secondary images.

- `Split oversized images into two` Splits oversized images into two, not normally used.


So we check `Use deepbooru caption as filename` and `Create flipped copies`.

Click the button and wait for the process to finish.





## Training

Training is a dynamic process!

In the `Train` sub-tab, select the model you want to train.

`Learning rate` (hyperparameter: learning rate), the learning rate represents the rate at which information accumulates in the neural network over time, and this parameter has a large impact on the speed at which training is affected.

Generally, the lower the learning rate, the slower the learning (takes longer to converge), but generally the better the results.

When training, you can start with a larger learning rate and then gradually reduce it to `0.1 - 0.02 - 0.005`, using the last best result for each test.

This is usually set to 0.005, but if you want to go faster, you can use 0.01 to speed it up. However, if you set it too high, the gradient will be too large to converge and may break `embedding`, which will not work as expected. If you set it too small, you will easily fall into a local optimum. Currently TI supports setting learning rates of `0.1:500, 0.01:1000, 0.001:10000`, which will follow a schedule.

![CS231n](https://user-images.githubusercontent.com/75739606/197824268-273f074d-9622-41ad-9f1a-2980f180dbd9.png)
<!--
![CS231n](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/CS231n.png)
-->

`Log directory` is the log directory

`Prompt template file` is a text file with prompts, one per line, for training the model.

The directory, `textual_inversion_templates`, explains what you can do with these files.

The training will use `style.txt` and `subject.txt`

**If you are training painting styles, use `style.txt`**

**If training characters, use `subject.txt`**

```
[name]: embedding name
[filewords]: words from the file name of the image from the dataset. See below for more info.
```
`Preview prompt` Preview, generate a preview with this prompt when finished.
If it is empty, the prompt from the prompt will be used.

``Save a copy of embedding to log directory every N steps, 0 to disable`
Save a copy of embedding to log directory every N steps, 0 to disable

`Save an image to log directory every N steps, 0 to disable`
Save an image to log directory every N steps, 0 to disable

`Max steps` determines how many `steps` are completed before training will stop.

A step is a single image (or batch of images, but batches are not currently supported) that is trained to the model and used to improve embedding. if you interrupt training and resume it later, the number of steps will be kept.


For stylised models of character images, the recommended number of steps is 15000-40000

stylised model of the artist's drawing style, recommended number of steps is 40,000-80,000

!!! tip
    This is a reference, in fact 5000 and 7000 have also been successful.

    The key is the Loss rate. If the Loss does not decrease in 10 rounds, you can stop.

    If the Loss is greater than 0.3, the results are not very good

If too much is overfitting (which can be interpreted as deadening of Ai), keep an eye on it and stop if it is overfitting. If it doesn't work very well, go to an earlier model and continue training. **Keep tweaking** to find a good result.

`Save images with embedding in PNG chunks` is to generate a pt file in image form. ~Character card~



Click on Training in the bottom right corner and wait.

Training is complete. If you have uninstalled VAE, rename the VAE weight file back and restart the program.


## Remarks


**Effect evaluation**

An attempt can be made to imitate the original work.

**[filewords]**

This is the Tag that represents the prompt template file and enables the insertion of **filenames into the prompt**.

First, by default, the file extension and all numbers and dashes ( ) at the beginning of the - file name are removed.

So this filename: `000001-1-a man in suit.png` will become the prompt text: `a man in suit`. The formatting of the text in the file name remains the same.

Second, the text in the filename can be changed using the `Filename word regex` and `Filename join string` options.

For example, using the word `regex = \w+` and the join string = `,`, the above file will produce the following text: `a, man, in, suit`.

The regular expression will extract the hint word from the text
`['a', 'man', 'in', 'suit', ]`

and places the concatenated string (',') between these words to create a text `a, man, in, suit`

You can also create a text file with the same filename as the image ( 000001-1-a man in suit.txt) and place the prompt text there. The filename and regular expression options will not be used.

**Move VAE and CLIP from VRAM when training. saves VRAM.**

Unloads VAE and CLIP from VRAM and loads them into RAM when training.

This option on the Settings tab allows you to save some memory at the cost of slower preview image generation.

The result of training is a .pt or a .bin file (the former is the format used by the original author, the latter is used as a diffusers library)

<iframe src="//player.bilibili.com/player.html?aid=559085039&bvid=BV1ae4y1S7v9&cid=859894044&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>


