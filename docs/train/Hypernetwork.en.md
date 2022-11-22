# Hypernetwork [^13]

Hypernetworks is a novel concept for fine-tuning models without touching any weights.

!!! tip
    Check **Move VAE and CLIP to RAM when training hypernetwork. Saves VRAM.** The effect is to transfer VAE to RAM, not to unload it.

    If you need to uninstall, please rename it.

You can train `Hypernetworks` in the `train` tab, in the same way as `Textual Inversion`.

Note that you should use a very low learning rate, e.g. 0.000005 or 0.0000005.

If the learning rate is 5e-6, the number of steps is about 10000 to 20000. [Learning Rate Calculator](https://colab.research.google.com/drive/1qzweYEMIFkG6jPa04tD1MhWWOzgSnDvP?usp=sharing), from [here](https://github.com/) AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

The resolution is set to the resolution to be trained, preferably using an aspect ratio of 1: 1.

If the dataset has long or horizontal bars, check `Split cversized images into two`.

If you don't already have a caption, you can check `Using deepbooru for caption` to allow deepbooru to recognise the caption.

## hyper network layer structure

If write "1, 2, 1", hypernetworks are composed of 2 fully connected layers whose intermediate dim is 2x, which is same as up to now.

The more you add the number, like "1, 2, 4, 2, 1", the more the structure of hypernetworks becomes deeper. Deep hypernetworks are suited for training with large datasets.
The layer structure is still recommended for 1,2,1, but if you want to expand it, try 1,2,2,1.[^16]

The pt file for 1,2,1 is about 83.8MB and 1,2,2,1 is about 167MB. if you want to extend it, you have to be careful if you have enough disk space, otherwise it will easily explode. [^16]

## Add layer normalization

If checked, add layer normalization after every fully connected layer.

Preventing overfitting of the hypernetwork makes training more stable.

The LN makes sense for preventing overfitting and making training more stable, but according to previous tests, with the LN on inlinear, it may not learn anything and may come out empty. With the activation function, LN is now available. [^16]

Swish does not seem to need to have LN on, as Swish is slow to train, so a higher learning rate should be used.

## USE Dropout

Enabling Dropout prevents overfitting of the supernetwork. Custom dropout ratios are not currently supported, the default is 0.3

On a 1,2,1 layer structure, dropout may only be useful for small data sets.

## Activation functions activation functions

About [activation functions](https://www.geeksforgeeks.org/activation-functions-neural-networks/), [wiki](https://en.wikipedia.org/wiki/Activation_ function)

The activation function is the source of nonlinearity in a neural network. If the activation function is removed, the weights and biases of the neural network will only be linearly transformed (no matter how many layers there are in the neural network, the composite of linear operations is still a linear operation), and the end result is only equivalent to a single-layer linear model.

The activation function performs a non-linear transformation of the input, allowing it to learn and perform more complex tasks, and the deep neural network is much more expressive.

This feature was proposed by discus0434 in a [merge request](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3199) on 10/21/22.

Support for relu, leakyrelu and linear. Selecting linear has the same effect as no activation function.

Swish is better than Relu and Linear, Swish is suitable for larger networks, while Elu may give better results for 1,2,1 hypernetworks. [^16]

[Related English guides](https://rentry.org/hypernetwork4dumdums)

[Related English discussion](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

![hypernet 对比图](https://user-images.githubusercontent.com/75739606/197823893-88a3bece-9100-41d1-8f07-bc6d9b3aa92c.jpg)
<!--
![hypernet 对比图](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/exp_hypernet.jpg)
-->
> hypernet 对比图，the woman in picture is 24 years old

## Additional notes [^16]

With the LN selected activation function turned on and Dropout enabled, the hypernetwork is not as easily overfitted as before, but may require a larger learning rate for learning.

[Hypernetwork painting style training experience sharing](https://shiina-h.notion.site/shiina-h/Hypernetwork-1fc0b60645284c5e96bb41b583a4e86f)
