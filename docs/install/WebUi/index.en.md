# Installing the Stable-Diffusion-webui open source framework

This tutorial is referenced from [FAQ](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#prompt-editingmixing) by crosstyan[^2]

The official code repository for WebUi is at https://github.com/AUTOMATIC1111/stable-diffusion-webui

[Official English installation tutorial](https://github.com/AUTOMATIC1111/stable-diffusion-webui#installation-and-running)

[Official list of given dependencies](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Dependencies)


## Hardware Requirements

WebUi minimum usable N-card graphics memory is 2GB

WebUi has a minimum of 4GB of N-card memory available


Before starting, open CMD and type `nvidia-smi` to see if your machine's Nvidia graphics card is functioning properly.

! [效果.png](https://s1.ax1x.com/2022/10/10/xtdNNR.png)
>Here is the September effect (it's a lot faster now)


## Installation

!!! tip
    We recommend anyone who can afford it to read the [Official Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki).

    Note! Ai requires a large dependency size, so be prepared for that. The total dependency size is about several gigabytes, which can be very slow in the Chinese network environment!

    And for Windows, **dependencies are installed on C drive** by default, please make sure you have enough space.

    It is not recommended to use the integration package, it will cause difficulties to upgrade.

>Tips:WSL can not be migrated, if the C drive is not enough, unfortunately....



### **Windows installation**

The installation process on Windows is as follows.

**Install Python**

Install [Python 3.10.6](https://www.python.org/downloads/windows/), check "Add Python to PATH" when installing


**Install Git**

Download the Git installer from [GitScm](https://git-scm.com/download/win) and install it, then find `git bash` in the start menu

or open the directory you want to put it in, right click on it and look for `Git Base Here` in the shortcut menu.

or start CMD in the directory where you want to run it


**pulling repository**


Run `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git` in cmd to pull the repository.

And you can use `git pull` to update the code directly. We recommend updating frequently and following the community discussions.


**Try to install**

Place the 4G or 7G or other model `model.ckpt` in the `models` directory, `WebUi` currently supports model selection at the top of the interface.

(Optional) Put `GFPGANv1.4.pth` in the base directory, next to `webui.py`

Make sure your graphics card is available first (see above), then run `webui-user.bat` as a normal non-admin user from Windows Explorer. The script will automatically download the dependencies (**usually takes 22 minutes, installed by default under Python on the C drive, 1.98GB of downloaded resources, 2GB+ in size after decompression**) and output the address to launch the web interface.

During this interval you can open Explorer and keep an eye on the network speed. The script will not continue to output any prompts until the download is complete. (Waiting for two hours? Try hanging a proxy)

After about 30 minutes of installation, the program will output an address like `http://127.0.0.1:7860/`, just click on it (note that it is http, and the port may change if you don't specify it).




!!! tip
    Models are required to run!

    If you wish to generate secondary girls, please find a way to get some models. As for how to choose a model, please see the next section. If you can also clone other people's `models` folders to yours.

    If interested in other models, (oil painting, watercolor), we recommend [Stable Diffusion Models](https://rentry.org/sdmodels#), which can be downloaded by `2.Stable Diffusion v1.4 [4af45990] [7460a6fa] [ 06c50424]` which is a model to run the original model of `Stable Diffusion`.


### Linux installation

Install the dependencies first

```bash
# Debian-based:
sudo apt install wget git python3 python3-venv
# Red Hat-based:
sudo dnf install wget git python3
# Arch-based:
sudo pacman -S wget git python3
```

One-click script

```bash
bash <(wget -qO- https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh)
```

Done.


!!! info
    If you want, try to use `miniconda` (anaconda special huge ....) to create a virtual environment for Python 3.10.6.


    ```bash
    conda create -n aidraw python=3.10.6

    conda activate aidraw

    COMMANDLINE_ARGS="--medvram" REQS_FILE="requirements.txt" python launch.py
    ```
[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)

