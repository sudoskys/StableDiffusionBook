
# Start

First you must have a 4GB model and the menu at the top of the interface is used to select the model.

If you run it and report an error, please read the previous configuration tips.

We don't provide NAI models, you can go to the About/Bottom Icons page to find the `Chinese Community` channel, or maybe the Alibaba treasure chest.


## Run

**After loading in the model**

Win edit and run `webui.bat`

Linux users run `launch.py` or `webui.py`, the former will install the environment automatically.

```
REQS_FILE="requirements.txt" python launch.py
```

```
COMMANDLINE_ARGS="--medvram --always-batch-cond-uncond" REQS_FILE="requirements.txt" python launch.py
```

## Generate the first image using the prompt

Here a certain model is used, the example only applies to ``some model`''

For the token, read the back, an example is given here for you to complete the test.

The one at the top is a selection model.

- Write the words about the desired result in the first input box

You can use emoticons and emoji, and use () to enhance the weights, see later for the rules

```
((masterpiece)), best quality, illustration, 1 girl, beautiful,beautiful detailed sky, catgirl,beautiful detailed water, cinematic lighting, dramatic angle, Grey T-Shirt, (few clothes),(yuri),(medium breasts),white hair,cat ears,masturbation,bare shoulders ,(gold eyes),wet clothes
```

- A second input box to write undesired results

That is, specify what tags need to be filtered


```
lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry, bad feet
```


**First adjustment of parameters**

Selected resolution parameter, the larger the generation the longer it takes.


The generated result ends similarly as follows
```
Steps: 28, Sampler: Euler a, CFG scale: 7, Seed: 2706937631, Size: 1024x512, Model hash: 925997e9
```
With the same parameters and Seed (-1 is random), the same image can be produced without loading the style model, which can be used for fine tuning!

Together with the GPU load information
```
Time taken: 33.97s
Time taken: 33.97s
Torch active/reserved: 1975/2144 MiB, Sys VRAM: 7890/8134 MiB (93.61%)
```

**Quick Setup**

Don't want to go back and forth setting Clip parameters?

Add the `sd_hypernetwork` and `CLIP_stop_at_last_layers` fields to the `Quicksettings list` settings on the settings page, click the button at the top of the page to save, restart webui, and you'll see a quick switch option at the top of the Ui.


## Multilanguage/Localization

The language file is provided as a single `.json` file. Simply place this file in the `localizations` directory and select it in the settings.



**Create a localization file of your own**

Go to Settings and click the button at the bottom of `Download localization template` to download a localization template that you can edit.



## Shared links

- Use the `--share` option to run online!

You'll get a `xxx.app.gradio` shared link, plus you can set authentication for said `gradio` shared instance using the parameter: `--gradio-auth username:password`

!!! danger "RCE"

    When using the `--share` parameter, your instance is accessible on the public Internet.

    But recently someone reported a possibly dangerous [code vulnerability](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/2571)

    So, you can set authentication for `-gradio` shared instances using the parameter: `--gradio-auth username:password` (multiple comma-separated usernames and passwords can be provided)

    Examples
    `--share --gradio-auth admin:admin,user1:user_password`

    Using this example will create two users, one with the account password admin and the other with the account user1 and the password user_password

    If the attacker has access to the ui, they may be able to run the python script remotely.

    10/30 Community Report: Someone is scanning the publicly available WebUi

    11/1 Community Report: Shared links may lead to a risk that **attackers can access all files on the system. **


You can lock settings from being edited by others by adding the `--free-ze-settings` startup param.

- Port forwarding

The `-listen` parameter causes the server to listen for network connections.

Use `--listen` to make the server listen to network connections. This will allow computers on the local network to access the UI, and if you configure port forwarding, also computers on the internet.


- Listening port

Use `-port xxxx` to make the server listen on a specific port, xxxx being the wanted port. Remember that all ports below 1024 need root/admin rights, for this reason it is advised to use a port above 1024. Defaults to port 7860 if available.


- Themes

Add `--theme` parameter when startup to switch themes....



## Custom Runs

To further familiarize yourself with this program, you will find that you can modify the Bat run script to add parameters.

The specific list of parameters is shown below.

Error generating an image? Please read the `Paint guide` section.

First, you can run `python webui.py --help` to see all command arguments, or read them in the [source file](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/ shared.py) to read them.

Tip:
The recommended way to customize how the program runs is to edit webui-user.bat (Windows) and webui-user.sh (Linux)



*Customize the runtime environment

`set PYTHON=b:/soft/Python310/Python.exe`

`set PYTHON` allows setting custom Python paths

`set VENV_DIR=C:\run\var\run`
set VENV_DIR allows you to select the directory of your virtual environment. The default is `venv`. The special value `-` runs scripts without creating a virtual environment.

`set COMMANDLINE_ARGS=--ckpt a.ckpt`
set COMMANDLINE_ARGS sets the command line parameter webui.py to run
Example using `a.ckpt` instead of `model.ckpt`.


*Specify which GPU to run on*

Selecting the default GPU to use `--device-id 0` in place of `CUDA_VISIBLE_DEVICES=0` for older versions (before 2022/10) allows you to select a second GPU to allow two instances to run at the same time, enabling simple device selection in a more concise manner.

To view the GPU model and CUDA version, you can use the `nvidia-smi` command for information




**Optimize command parameters**

see [Official Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Optimizations)

| commandline argument            | explanation                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|---------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `--xformers`                    | Use[xformers](https://github.com/facebookresearch/xformers)library. Great improvement to memory consumption and speed. Windows version installs binaries mainained by[C43H66N12O12S2](https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases). Will only be enabled on small subset of configuration because that's what we have binaries for.[Documentation](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Xformers) |
| `--force-enable-xformers`       | Enables xformers above regardless of whether the program thinks you can run it or not. Do not report bugs you get running this.                                                                                                                                                                                                                                                                                                                 |
| `--opt-split-attention`         | Cross attention layer optimization significantly reducing memory use for almost no cost (some report improved preformance with it).  Black magic.On by default for`torch.cuda`, which includes both NVidia and AMD cards.                                                                                                                                                                                                                       |
| `--disable-opt-split-attention` | Disables the optimization above.                                                                                                                                                                                                                                                                                                                                                                                                                |
| `--opt-split-attention-v1`      | Uses an older version of the optimization above that is not as memory hungry (it will use less VRAM, but will be more limiting in the maximum size of pictures you can make).                                                                                                                                                                                                                                                                   |
| `--medvram`                     | Makes the Stable Diffusion model consume less VRAM by splitting it into three parts - cond (for transforming text into numerical representation), first_stage (for converting a picture into latent space and back), and unet (for actual denoising of latent space) and making it so that only one is in VRAM at all times, sending others to CPU RAM. Lowers performance, but only by a bit - except if live previews are enabled.            |
| `--lowvram`                     | An even more thorough optimization of the above, splitting unet into many modules, and only one module is kept in VRAM. Devastating for performance.                                                                                                                                                                                                                                                                                            |
| `*do-not-batch-cond-uncond`     | Prevents batching of positive and negative prompts during sampling, which essentially lets you run at 0.5 batch size, saving a lot of memory. Decreases performance. Not a command line option, but an optimization implicitly enabled by using`--medvram`or`--lowvram`.                                                                                                                                                                        |
| `--always-batch-cond-uncond`    | Disables the optimization above. Only makes sense together with`--medvram`or`--lowvram`                                                                                                                                                                                                                                                                                                                                                         |
| `--opt-channelslast`            | Changes torch memory type for stable diffusion to channels last. Effects not closely studied.                                                                                                                                                                                                                                                                                                                                                   |


[Discussion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3889) It has been suggested that by disabling hardware-accelerated GPU scheduling on Windows settings, WebUi performance has improved by about 10-50%



## API documentation

Run the program with the `--api` argument and visit `{your main url}/docs` in your browser to see the [Api](https://github.com/AUTOMATIC1111/stable-diffusion-webui/tree/master/modules/) documentation for WebUi. api) documentation. [^4]

Here is a synchronization class implementation.

```python
import time
import json
import requests
import io
import base64
from PIL import Image, PngImagePlugin

class WebUiApi(object):
    def __init__(url):
        self.url=url

    def txt2img(payload,outpath:str=None,infotie:bool=True):
        payload_json = json.dumps(payload)
        response = requests.post(url=f'{self.url}/sdapi/v1/txt2img', data=payload_json).json()
        # response 响应包含 images、parameters 和 info,image 可能会含有多个图像。
        for i in response['images']:
            # 解码 base64
            image = Image.open(io.BytesIO(base64.b64decode(i)))
            # 元信息输出
            pnginfo = PngImagePlugin.PngInfo()
            if infotie:
               pnginfo.add_text("parameters", str(response['info']))
            # Save, as the pic is not automatically saved.
            if not outpath:
               print("Random file name")
               outpath=f"{str(time.time())}.png"
            image.save(outpath, pnginfo=pnginfo)

payload = {
    "prompt": "1girl",
    "steps": 20
}
# Other parameters will use default values
WebUiApi(url="http://127.0.0.1:7860").txt2img(payload=payload,outpath="1145.png",infotie=True)

# 实际使用的时候不应该保存到本地再发送，而是直接发送，避免存储图片作品造成 版权 问题。
```

跨平台多后端项目 [novelai-bot](https://github.com/koishijs/novelai-bot)

Discord 机器人项目 [aiyabot](https://github.com/Kilvoctu/aiyabot/blob/main/core/stablecog.py)




# Error handling Troubleshooting

File paths, which do not allow spaces, make sure your program's folder path does not have spaces in it. This program has been tested to run on Python 3.10.6; errors may occur with lower versions of Python.

To reinstall, remove the directories: `venv`, `repositories`.



## Install TOO SLOW！

**Dependencies >2GB**, please wait. Tips for Windows users: **Dependencies are installed on C drive by default**




## Low memory graphics cards/CUDA out of memory

Make sure you have the latest CUDA toolkit and GPU drivers that are ready to run.

Programs running on video cards with a small amount of VRAM (<=4GB) may experience out of memory errors.



**牺牲速度来支持低显存显卡**

When running on video cards with a low amount of VRAM (<=4GB), out of memory errors may arise. Various optimizations may be enabled through command line arguments, sacrificing some/a lot of speed in favor of using less VRAM:

- If you have 4GB VRAM and want to make 512x512 (or maybe up to 640x640) images, use --medvram.

- If you have 4GB VRAM and want to make 512x512 images, but you get an out of memory error with --medvram, use --medvram --opt-split-attention instead.

- If you have 4GB VRAM and want to make 512x512 images, and you still get an out of memory error, use --lowvram --always-batch-cond-uncond --opt-split-attention instead.

- If you have 4GB VRAM and want to make images larger than you can with --medvram, use --lowvram --opt-split-attention.

- If you have more VRAM and want to make larger images than you can usually make (for example 1024x1024 instead of 512x512), use --medvram --opt-split-attention. You can use --lowvram also but the effect will likely be barely noticeable.

- Otherwise, do not use any of those.



## Where my Python?

If the desired version of Python is not in PATH, modify the line set PYTHON=python in webui-user.bat with the full path to the python executable.

## ERROR:asyncio:Accept failed on a socket


First check for port conflicts, and any software conflicts

This error may be due to compatibility of `asyncio` library of `Python38` with `Windows`.

Try running `netsh winsock reset` as a CMD administrator, and if that doesn't work, switch ports.

If that still doesn't work, switch to Python `3.10.6`, which is the version of WebUi's development environment.




## Venv

If you are using conda you can run the launch install dependency yourself without the one-click script.

This is because the one-click installer script given by the repository creates the virtual environment and then launches [launch.py](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/launch.py).

At runtime, the one-click installer creates a python virtual environment, so if you install a module before installing it, then any installed modules will not cause conflicts.

If you need to prevent the creation of a virtual environment and use system python, edit `webui.bat` and replace `setVENV_DIR=venv` with `set VENV_DIR=`


## api-ms-win-core-path-l1-1-0.dll is missing[^3]

Running on Windows 7 is likely to report the error: `api-ms-win-core-path-l1-1-0.dll is missing` because many programs require system files from newer versions of Windows.

If this error does not occur, there is no need to perform the following actions.

These files have been ported to be compatible with W7 and are available at [here](https://github.com/nalexandru/api-ms-win-core-path-HACK/releases/download/0.3.1/api-ms-win-core-path- blender-0.3.1.zip) for download. Its [Github page](https://github.com/nalexandru/api-ms-win-core-path-HACK/)

Unzip and copy `x86.dll` to `C:\Windows\SysWOW64`, copy `x64.dll` to `C:\Windows\System32` and reboot.

## an illegal memory access was encountered ....CUDA kernel errors...

[RelatedIssue](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/1766)

In most cases, this means that the video memory is overflowing, but it could also be a GPU hardware problem.

It is said that if you are using deepdanbooru, you will get this message, so try to restart or install the cpu version of deepdanbooru.

**See the next chapter for image generation problems**.


[^3]:[Wiki2](https://rentry.co/voldy)

[^4]:[Basic Documentation and Examples for using API](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3734)
