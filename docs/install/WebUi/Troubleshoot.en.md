
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

In most cases, this means that the VRAM is overflowing, but it could also be a GPU hardware problem.

It is said that if you are using deepdanbooru, you will get this message, so try to restart or install the cpu version of deepdanbooru.

**See the next chapter for image generation problems**.