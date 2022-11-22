## About the graphics card

!!! tip
    Pay attention to the graphics card temperature, there are reports that the graphics card is too hot and fries.

**TIPS**, make sure you have configured and are using this environment. (you can use torch)

First use `nvidia-smi` to determine if the graphics driver is available.

Then determine the configuration of CUDA: open a terminal, start `python`, and type the following command in the branch

```python
import torch
print(torch.__version__)
print(torch.cuda.is_available())
```

**Check the CUDA version**

```python
import torch
torch.version.cuda
```

Type ctrl + z to exit

If any errors occur, ask someone or use a search engine to resolve them.

## WebUi Multi-GPU support

The simplest model is to implement a method for parallel processing of multiple data by starting multiple instances with the `--device-id` parameter. Each GPU loads a model and then assigns them work.

Considering the multitude of features offered by this project, I (the author) think it might take a while to use all the features in parallel.

[Response from WebUi developers](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/156#issuecomment-1241218733)

>Using memory from between two GPUs is not simple. I only have one so I can't research/develop this.

# WebUi 16xx GPU use half-precision to generate images [^3]

Solution from [this issue](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/28#issuecomment-1241448049)

1. activate the venv used by the WebUI, to run in the correct virtual environment

2. uninstall the torch and torchvision you are using now:

```bash
pip uninstall torch torchvision
uninstall torch and torchvision: ```bash pip

3. reinstall ``CUDA 11.6`` compiled by ``torch`` and ``torchvision``.

```bash
pip install torch torchvision --extra-index-url https://download.pytorch.org/whl/cu116
```

4. download `Cudnn_8.5` for backup.

Use the download tool to download, direct download will redirect you to Nvidia s developer registration screen

Windows: https://developer.nvidia.com/compute/cudnn/secure/8.5.0/local_installers/11.7/cudnn-windows-x86_64-8.5.0.96_cuda11- archive.zip

Other versions: https://developer.nvidia.com/rdp/cudnn-archive

5. Copy all the files from the bin and lib folders in the Cudnn 8.5 archive to `venv\Lib\site-packages\torch\lib`, overwriting all the files.

6. Then the 16xx GPU can happily use half-precision image generation! It can generate 1024x640 images with 6G load Full model, which greatly reduces VRAM usage.

However, you still can't use `DDIM Sampling`, but you can use `Euler a`.
