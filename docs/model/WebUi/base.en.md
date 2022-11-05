# Configure

## Basic

>Most of the content in this section is from the [Wiki page](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/), so for English users, it may be better to read the original documentation.

### Parameter intro

The number of iterations of `step` is related to the `sampling method`, the `DDIM` method converges faster, see `Sampler vs.

`batch count/batch size` determines the number of generated images, if the VRAM is enough, add the batch size, if not, add the batch count, the number of images obtained is the product of both (small VRAM or just count is good)

DDIM, Eula also works well. (with a is the meaning of ancestral, step growth out of the map is not stable)

`cfg scale` conforms to the degree of prompt, the higher the value will literally look prompt, low will give the model more room to play, but the actual model performance on the CFG scale low (6-8) saturation is low, biased line drawing, biased clutter, high (18-22) then saturation is high, biased CG style.

>Too high CFG will cause color distortion, CFG should be between 5-15

`denoise strength` img2img proprietary parameter, from 0 to 1, the higher the value the lower the AI reference to the original image (while increasing the number of iterations), personally I like low CFG high denoise heavy drawing, high CFG low denoise change the details.

[A small guide: RedditAbout](https://www.reddit.com/r/StableDiffusion/comments/xbeyw3/can_anyone_offer_a_little_guidance_on_the/)



### Security issues with ckpt models [^4]

The ckpt file can execute essentially anything when loaded, and loading it blindly is a security risk. Please check if the source is reliable before loading.
If antivirus software intercepts it, it is possible that the creator has injected malicious Python code into the file.

You can check for risks with this script: <https://rentry.org/safeunpickle2>




## Advanced tutorials

### Start-up process [^6]


![Roaming_info.png](https://user-images.githubusercontent.com/75739606/198679721-2a7b38b8-41f3-405c-9ea3-40f1b5e8cc7e.png)
<!--
![Roaming_info.png](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/infofrom_bili_uid_87077691.png)
-->
>From allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/




### Replicating the NAI official website using WebUI

[Related discussion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017)

Tip:Due to the nature of torch and its associated frameworks, it is not wise to try to fully recover images generated on a different machine. So don't get hung up on details that can't be reproduced.



#### Things to do

* Load VAE and the `config.yaml` that comes with the model (optional, some people say this operation consumes VRAM)

* `Stop At last layers of CLIP model` set to `2`

* `Eta noise seed delta` set to `31337`


#### **Doesn't need **to do

* hypernetwork. the website does not use hypernetwork by default


TIPS:

The `Stop At last layers of CLIP model` is set to match one of the NAI [optimizations](https://blog.novelai.net/novelai-improvements-on-stable-diffusion-e10d38db82ac).


### Half-precision or single-precision?

If you can, try to use half-precision, it saves computing time/RAM/VRAM, while the image quality is not much worse than single-precision. ~The difference is probably the same as if your computer was hit by cosmic rays.


### X/Y graphs

Creates a grid of images with varying parameters. Select which parameters should be shared by rows and columns using X type and Y type fields, and input those parameters separated by comma into X values/Y values fields. For integer, and floating point numbers, and ranges are supported.


Simple ranges

```
1-5 = 1, 2, 3, 4, 5
```

Ranges with increment in bracket

```
1-5 (+2) = 1, 3, 5
10-5 (-3) = 10, 7
1-3 (+0.5) = 1, 1.5, 2, 2.5, 3
```

Ranges with the count in square brackets

```
1-10 [5] = 1, 3, 5, 7, 10
0.0-1.0 [6] = 0.0, 0.2, 0.4, 0.6, 0.8, 1.0
```



![pic](https://raw.githubusercontent.com/wiki/AUTOMATIC1111/stable-diffusion-webui/images/xy_grid-medusa-ui.png)
>FromWebUiWiki

#### Prompt S/R

Prompt S/R is one of more difficult to understand modes of operation
for X/Y Plot. S/R stands for search/replace, and that's what it does -
you input a list of words or phrases, it takes the first from the list
and treats it as keyword, and replaces all instances of that keyword
with other entries from  the list.

For example, with prompt `a man holding an apple, 8k clean`, and Prompt S/R `an apple, a watermelon, a gun` you will get three prompts:

* `a man holding an apple, 8k clean`
* `a man holding a watermelon, 8k clean`
* `a man holding a gun, 8k clean`

The list uses the same syntax as a line in a CSV file, so if you want
to include commas into your entries you have to put text in quotes and
make sure there is no space between quotes and separating commas:

* `darkness, light, green, heat` - 4 items - `darkness`, `light`, `green`, `heat`
* `darkness, "light, green", heat` - WRONG - 4 items - `darkness`, `"light`, `green"`, `heat`
* `darkness,"light, green",heat` - RIGHT - 3 items - `darkness`, `light, green`, `heat`



### **Variations**

A Variation strength slider and Variation seed field allow you to specify how much the existing picture should be altered to look like a different one. At maximum strength, you will get pictures with the Variation seed, at minimum - pictures with the original Seed (except for when using ancestral samplers).

### **Styles**


Press the "Save prompt as style" button to write your current prompt to styles.csv, the file with a collection of styles. A dropbox to the right of the prompt will allow you to choose any style out of previously saved, and automatically append it to your input. To delete a style, manually delete it from styles.csv and restart the program.

if you use the special string {prompt} in your style, it will substitute anything currently in the prompt into that position, rather than appending the style to your prompt.


### xformers


xformers The higher the resolution the better the acceleration. Using xformers introduces some randomness that slightly affects the generated image.

To enable it, if you have a Pascal, Turing or Ampere architecture card (including GTX 1000, RTX 2000, 3000 series), add the `-xformers` parameter to `COMMANDLINE_ARGS` in `webui-user.bat`.

!!! tip
    Please note that some people say that performance is significantly worse with xformers on 700 and 900 series cards.
    I have measured that the 2050 is 50% slower with xformers enabled



#### Xformers


If you have a Pascal, Turing or Ampere architecture card (including GTX 1000, RTX 2000, 3000 series), add the `--xformers` parameter to `COMMANDLINE_ARGS` in `webui-user.bat` **without having to compile and install it again yourself.**

!!! info
    You can download pre-built Xformers at the link on the right! https://rentry.org/25i6yn and remember to check out [GPU Architecture](https://developer.nvidia.com/cuda-gpus) first!

Make sure the Python version is 3.10 or higher (use `Python --version`), then install

Install [VS Build Tools 2022](https://visualstudio.microsoft.com/zh-hans/downloads/?q=build+tools), you only need Desktop development with C++

Install [CUDA 11.3](https://developer.nvidia.com/cuda-11.3.0-download-archive) (later versions are not tested), select `custom`, you only need the following (VS integration is probably unecessary):


* Clone the [xFormers](https://github.com/facebookresearch/xformers) repo, create a `venv` and activate it

```bash
git clone https://github.com/facebookresearch/xformers.git
cd xformers
git submodule update --init --recursive
python -m venv venv
./venv/scripts/activate
```

* To avoid issues with getting the CPU version, [install pyTorch seperately](https://pytorch.org/get-started/locally/):

```
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu113
```

* Then install the rest of the dependencies

```bash
pip install -r requirements.txt
pip install wheel
pip install ninja
```

* As CUDA 11.3 is rather old, you need to force enable it to be built on MS Build Tools 2022.

Do `$env:NVCC_FLAGS = "-allow-unsupported-compiler"`

if on `powershell`, or `set NVCC_FLAGS=-allow-unsupported-compiler` if on `cmd`

You can finally build xFormers, note that the build will take a long time (probably 10-20minutes), it may initially complain of some errors but it should still compile correctly.

```
OPTIONAL tip: To further speed up on multi-core CPU Windows systems, install ninja https://github.com/ninja-build/ninja. Steps to install:

        download ninja-win.zip from https://github.com/ninja-build/ninja/releases and unzip
        place ninja.exe under C:\Windows OR add the full path to the extracted ninja.exe into system PATH
        Run ninja -h in cmd and verify if you see a help message printed
        Run the follow commands to start building. It should automatically use Ninja, no extra config is needed. You should see significantly higher CPU usage (40%+).

    python setup.py build
    python setup.py bdist_wheel

    This has reduced build time on a windows PC with a AMD 5800X CPU from 1.5hr to 10min. Ninja is also supported on Linux and MacOS but I do not have these OS to test thus can not provide step-by-step tutorial.

```


* Run the following

```bash
python setup.py build
python setup.py bdist_wheel
```

In `xformers` directory, navigate to the `dist` folder and copy the `.whl` file to the base directory of `stable-diffusion-webui`

In `stable-diffusion-webui` directory, install the `.whl`, change the name of the file in the command below if the name is different:


```bash
#CMD
./venv/scripts/activate
#Bash
source ./venv/bin/activate
#WindowsBash
source ./venv/Scripts/activate
pip install xformers-0.0.14.dev0-cp310-cp310-win_amd64.whl
```


* Add `-xformers` for 30XX GPU, `--force-enable-xformers` for other GPU





#### Windows compiler error self-check

>Error:`Filename too long` and `fatal error C1083: Cannot open compiler generated file: '': Invalid argument`

It means your path is too long.

>RuntimeError: CUDA error: no kernel image is available for execution on the device

Now that more GPU architectures are automatically supported, try reinstalling and using the --xformers argument.

If you moved Xformers, then you should remove the venv directory inside

[Windows](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases) (You have to compile it yourself outside of the 30XX GPU)

Compile Guide [wiki/Xformers](https://rentry.org/sdg_faq#xformers-increase-your-its-more-cards-supported)， And another guide [Here](https://www.reddit.com/r/StableDiffusion/comments/xz26lq/automatic1111_xformers_cross_attention_with_on/)



### Painting with the CPU

>https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/2597

You can generate as much as possible using the CPU with `-use-cpu all`, although 100 times slower.

### CLIP Interrogate

CLIP can extract tokens from images.

By default, there is only one list - the artists list (from artists.csv).

However you can add more lists by doing the following.
* interrogate creates a directory in the same location as the WebUI
* put a text file into it, with a description on each line


You can see an example of which text file to use at [here](https://github.com/pharmapsychotic/clip-interrogator/tree/main/data). In fact, you could just use the files in this example - except for `artists.txt`, you already have a list of artists in `artists.csv` don't you.

Each file will add a line to the final description. If you put `.top3.` in the file name, e.g. `flavors.top3.txt`, the three most relevant lines in the file will be added to the prompt (any other number will do).

```
For example of what text files to use, see https://github.com/pharmapsychotic/clip-interrogator/tree/main/data.
In fact, you can just take files from there and use them - just skip artists.txt because you already have a list of artists in artists.csv (or use that too, who's going to stop you).
Each file adds one line of text to the final description.
If you add ".top3." to filename, for example, flavors.top3.txt, the three most relevant lines from this file will be added to the prompt (other numbers also work).
```


### **Face restorationThree dimensional face restoration**

Only for real people's faces.


[https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#face-restoration)

### User scripts

If the program is launched with `--allow-code` option, an extra text input field for script code
is available at the bottom of the page, under Scripts -> Custom code. It allows you to input python
code that will do the work with the image.

In code, access parameters from web UI using the `p` variable, and provide outputs for web UI
using the `display(images, seed, info)` function. All globals from the script are also accessible.

A simple script that would just process the image and output it normally:

```
import modules.processing

processed = modules.processing.process_images(p)

print("Seed was: " + str(processed.seed))

display(processed.images, processed.seed, processed.info)
```

### Custom.css

Create a file named `user.css` near `webui.py` and put custom CSS code into it. For example, this makes the gallery taller:

The following example will make the gallery taller.


```
#txt2img_gallery, #img2img_gallery{
    min-height: 768px;
}
```


A useful tip is you can append `/?__theme=dark` to your webui url to enable a built in *dark theme*

e.g. (`http://127.0.0.1:7860/?__theme=dark`)

Alternatively, you can add the `--theme=dark` to the `set COMMANDLINE_ARGS=` in `webui-user.bat`

e.g. `set COMMANDLINE_ARGS=--theme=dark`



### notification.mp3 提示音

If an audio file named `notification.mp3` is present in webui's root folder, it will be played when the generation process completes.


### Custom Scripts

To install custom scripts, place them into the scripts directory and click the Reload custom script button at the bottom in the settings tab. Custom scripts will appear in the lower-left dropdown menu on the txt2img and img2img tabs after being installed.

some [notable custom scripts](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts#stylepile) created by Web UI users

### Developing custom scripts

The Script class definition can be found in modules/scripts.py. To create your own custom script, create a python script that implements the class and drop it into the scripts folder, using the below example or other scripts already in the folder as a guide.

The Script class has four primary methods, described in further detail below with a simple example script that rotates and/or flips generated images.

The Script class has four main methods, described in more detail here by a simple [example script](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Developing-custom-scripts) which script can rotate and/or flip the resulting image.


### Trimming the model [^7]

Place the `.ckpt` file to be trimmed in the `/stable-diffusion-webui` folder, save the [script](https://raw.githubusercontent.com/harubaru/waifu-diffusion/main/scripts/prune. py) locally and delete lines 6 and 8. Then edit the name of the ckpt in the last line of prune.py.

For example, `prune_it('wd-v1-2-full-emma.ckpt')`

Then run this script, the pruning process may take a few minutes.

## RUN

### 4GB graphics card support

Optimizations for GPUs with low VRAM. This should make it possible to generate 512x512 images on videocards with 4GB memory.

`--lowvram` is a reimplementation of an optimization idea by [basujindal](https://github.com/basujindal/stable-diffusion).
Model is separated into modules, and only one module is kept in GPU memory; when another module needs to run, the previous
is removed from GPU memory. The nature of this optimization makes the processing run slower -- about 10 times slower
compared to normal operation on my RTX 3090.

`--medvram` is another optimization that should reduce VRAM usage significantly by not processing conditional and
unconditional denoising in the same batch.

This implementation of optimization does not require any modification to the original Stable Diffusion code.



### Uninterrupted Production

The option **Uninterrupted Production** will appear by right-clicking on the WebUI's generation key.

### Saving

Click the Save button under the output section, and generated images will be saved to a directory specified in settings; generation parameters will be appended to a csv file in the same directory.

### Loading

Gradio's loading graphic has a very negative effect on the processing speed of the neural network.
AUTOMATIC1111 s RTX 3090 makes images about 10% faster when the tab with gradio is not active. By default, the UI
now hides loading progress animation and replaces it with static "Loading..." text, which achieves
the same effect. Use the `--no-progressbar-hiding` commandline option to revert this and show loading animations.


### Png info

Adds information about generation parameters to PNG as a text chunk. You can view this information later using any software that supports viewing PNG chunk info, for example: https://www.nayuki.io/page/png-file-chunk-inspector


### NAI 4chan ver

4chan's version of the post-magic program will be dynamically allocated, and the vram is not enough to make up the ram.

[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

[^3]:[16xx系显卡可以使用半精度生成图片的方式](https://t.me/StableDiffusion_CN/50749)

[^4]:[It's not a virus it's a checkpoint file](https://huggingface.co/Deltaadams/Hentai-Diffusion/discussions/12)

[^5]:[所有超网络的 X/Y](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3836360)

[^6]:[Roaming_info_for_latent_diffusion](http://allophane.com/index.php/2022/10/17/roaming_info_for_latent_diffusion/)

[^7]:[pruning-a-ckpt](https://rentry.co/voldy#-pruning-a-ckpt-)
