# Term




### Loss functions

About [loss function](https://fangkaipeng.com/?p=2056#header-id-16)

### Latent space

A representation of compressed data where similar data points are spatially closer together.

### ENSD

ENSD in the settings page is the eta noise seed increment.

It changes your seed to perform some eta/sigma operations.

NAI uses 31337


### CLIP

```
CLIP is a very advanced neural network that transforms your prompt text into a numerical representation. Neural networks work very well with this numerical representation and that's why devs of SD chose CLIP as one of 3 models involved in stable diffusion's method of producing images. As CLIP is a neural network, it means that it has a lot of layers. Your prompt is digitized in a simple way, and then fed through layers. You get numerical representation of the prompt after the 1st layer, you feed that into the second layer, you feed the result of that into third, etc, until you get to the last layer, and that's the output of CLIP that is used in stable diffusion. This is the slider value of 1. But you can stop early, and use the output of the next to last layer - that's slider value of 2. The earlier you stop, the less layers of neural network have worked on the prompt.

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features#ignore-last-layers-of-clip-model
```

To get the AI to draw, you first need to get the program to "understand" your instructions, such as

"a big cherry tree above a lake with flying petals in the sky."

For a textual description of a relatively complex scene, the AI needs to be able to "understand" and match the corresponding picture, and most projects rely on a model called CLIP.

CLIP searches the potential space of the generated model to find a potential image that matches a given text description.

It is very modern and efficient.


### CUDA


With CUDA technology, the GPU can be used for general purpose processing (not just graphics); this approach is called GPGPU, and unlike CPUs, GPUs concurrently execute a large number of threads at a slower rate rather than a single thread at a faster rate. The GeForce 8800 GTX, for example, has 128 internal processors in its core. Using CUDA technology, it is possible to use those internal processors as thread processors to solve data-intensive calculations.


### LDM

Latent Diffusion Model

![？](https://user-images.githubusercontent.com/75739606/198675128-c2c849d0-d024-468b-80c4-374f13e933e3.png)
>Diagram of the lower part

[潜在扩散模型](https://zhuanlan.zhihu.com/p/573984443)

