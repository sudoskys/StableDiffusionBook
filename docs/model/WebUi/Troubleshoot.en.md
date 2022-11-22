
# Self-help when a bug occurs in generating images

## Generate black/green image

[Generate black/green image](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-NVidia-GPUs)

If u use GTX 16xx GPU, you need to add `-precision full --no-half` to the startup parameter, so if the VRAM is low, add `-medvram` as well.

If it's a different card and VAE is loaded, add `--no-half-vae` parameter[^2].

## RuntimeError Sizes of tensors must match

(img2img) If `RuntimeError: Sizes of tensors must match` appears, adjust the resolution of the input image

## Rainbow mess

If the AI outputs a messy rainbow-colored image, the generated resolution is set too low

## High resolution ghost image / low memory to generate large resolution image

This simply means regenerate or super-score using low resolution. See Hook and Kiss

You can see the exact procedure in the next section.

## RuntimeError: Unable to find a valid cuDNN algorithm to run convolution

Insufficient VRAM

## CUDA out of memory

Cause: Insufficient VRAM.

Please add `-lowvram` and `-medvram` parameters at startup to try to solve this problem.
