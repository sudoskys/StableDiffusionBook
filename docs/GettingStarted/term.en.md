# Help


## How sd work

### Reasoning process

![jalammar s pic](https://jalammar.github.io/images/stable-diffusion/stable-diffusion-components-and-tensors.png)

information creator works entirely in the image information space (or latent space). This feature makes it faster than previous diffusion models that worked in pixel space. Technically, this component is composed of a UNet neural network and a scheduling algorithm.

- Text Encoder

The parsing of the cues is processed by the Text Encoder/CLIP (token embedding), which is a key step in the translation of the cues to the AI.

The Text Encoder is responsible for converting the input cue into an embedding space that is understandable to the U-Net. It is usually a simple transformer-based encoder that maps a sequence of input tokens into a sequence of latent text-embeddings.

Stable diffusion uses ClipText for text encoding. Input text, output 77 marker embedding vectors, each with 768 dimensions.

- information creator

UNet + Scheduler (aka sampling algorithm) progressively processes/disperses information in latent space.

It inputs text embeddings and a starting multidimensional array of noise (a structured list of numbers, also called a tensor) and outputs a processed array of information.

- Image Decoder

The Text Decoder draws a picture based on the information obtained from the information creator. It is run only once at the end of the process to produce the final image.

The autoencoder (VAE) model has two parts, an encoder and a decoder. The encoder is used to convert the image into a latent representation, which is used as input to the U-Net model. The decoder then converts the latent representation back to an image.

During the inference process, a VAE decoder is used to convert the denoised latent image produced by the backward diffusion process back into an image. During the inference process, we only need the VAE decoder.

The Autoencoder Decoder (VAE) draws the decoder of the final image using an array of processed information. Input the processed information array (dimensions: (4,64,64)) and output the resulting image (dimensions: (3, 512, 512), i.e. (red/green/blue, width, height).

- The work of CLIP

![training image](https://pic3.zhimg.com/80/v2-340920caff256e06c29cff7097e23e62_1440w.jpg)
>CLIP training map from https://bbs.huaweicloud.com/blogs/371319

- Process

The autoencoder used in Stable Diffusion has a reduction factor of 8. This means that an image of (4, 512, 512) is (4, 64, 64) in the potential space.

In reasoning about a 512 * 512 image using stable diffusion, the model uses a seed and a text cue as input. The potential seed generates a random potential image of size 64 × 64, while the prompt goes into the Text Encoder to be transformed into a text embedding of size 77 × 768 by CLIP's text encoder.

U-Net iteratively denoises the random Gaussian noise representation while conditioning on the text embedding. u-Net calculates the denoised latent image representation using the sampling algorithm and outputs the noise residuals. After this step has been repeated many times, the potential representation is decoded by the decoder of the Image Decoder's auto encoder and output.

The text embedding is combined with random Gaussian noise into the information creator and a processed array of information (noise residuals) is output.


![stable-diffusion](https://raw.githubusercontent.com/patrickvonplaten/scientific_images/master/stable_diffusion.png)


- Extensions

[illustrated-stable-diffusion](https://jalammar.github.io/illustrated-stable-diffusion/)

[Stable Diffusion](https://huggingface.co/blog/stable_diffusion)

[Introduction to Stable Diffusion](https://pub.towardsai.net/getting-started-with-stable-diffusion-f343639e4931)

[Stable Diffusion From Wikipedia](https://en.wikipedia.org/wiki/Stable_Diffusion)

### Preprocessing for WebUi

[Prompt_parser for WebUi](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/modules/prompt_parser.py) Implemented with native WebUi for fading etc. functions.

The WebUi prompt syntax is converted into a prompt of the corresponding time, which is then passed on to Ai for processing by embedding.

Regarding the weighting implementation: the weighting increase usually takes up one prompt position.

For the implementation of the fade: at a given Step, the WebUi program replaces the corresponding prompt to achieve the fade effect.

And so on.

The whole process is shown in the diagram 

![prompt_draw](https://user-images.githubusercontent.com/75739606/198675128-c2c849d0-d024-468b-80c4-374f13e933e3.png)
>By RcINS

### Math

[diffusion_model](https://www.cs.unc.edu/~ronisen/teaching/fall_2022/pdf_lectures/lecture8_diffusion_model.pdf)

[How diffusion models work: the math from scratch](https://theaisummer.com/diffusion-models/#classifier-free-guidance)



## Term

### CFG 

This term stands for Classifier Free Guidance Scale and is used to measure how well the expected image generated by the model matches your cues. A Cfg Scale value of 0 will generate a random image based on the seed. [1^]

Imagine, for example, that your cue is a torch with a variable-width beam that you shine onto the model's potential space to highlight a specific area - your output image will be drawn from somewhere within that area, depending on the seed.

Toggling the Cfg Scale to zero produces an extremely wide beam that highlights the entire potential space - your output can come from almost anywhere.

Toggling the Cfg Scale to 20 produces a very narrow beam, so much so that in extreme cases it becomes a laser pointer, illuminating a single point in the potential space. 

https://arxiv.org/abs/2207.12598

### Loss functions

About [loss function](https://www.datarobot.com/blog/introduction-to-loss-functions/)

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

[1^]:[Getting Started With Stable Diffusion: A Guide For Creators](https://www.jonstokes.com/p/getting-started-with-stable-diffusion)