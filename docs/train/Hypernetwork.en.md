# Hypernetwork 

Hypernetworks[^2] is a novel concept for fine-tuning models without touching any weights.

This technique is widely used in drawing style mimicry, and generalizes better compared to Textual Inversion.

This page is mainly a guide for you to do it in practice.

If you are looking for some more detailed and in-depth Hypernetwork instructions, please see [this community post](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670).

!!! tip "About VAE"
    VAE is said to corrupt training results.

    The effect of checking **Move VAE and CLIP to RAM when training hypernetwork. Saves VRAM.** is to transfer VAE to RAM, not to uninstall it.

    If you need to uninstall, please rename it.

You can train `Hypernetworks` in the `train` tab, in the same way as `Textual Inversion`.

## Preprocessing training set

Unlike training people [^1], training hypernetworks does not require a clean background or a prominent subject, although the more material you train, the better the result.

- Fixed style, for example, discard the artist's early work
- Remove text or watermarks
- Resolution should not be too small

**Crop processing**

Currently, WebUI has integrated image pre-processing function, which can automatically crop and scale images.

If you need to make sure the subject of the picture is clear, it is recommended to manually crop yourself to ensure the subject is in the center of the picture. [Batch Crop Tool](https://www.birme.net)

Because it is style training, you do not need to worry about whether the training subject is asymmetric or not, so it is recommended to turn on image flipping to multiply the data set and improve the training effect. [^1]

**Labeling**

If you haven't labeled yet, you can check `Using deepbooru for caption` to let deepbooru generate labels automatically. [^1]

If you want to tag manually, we recommend the [GUI tagging tool](https://github.com/arenatemp/sd-tagging-helper)

## Parameters

Note that you should use a very low learning rate, such as 0.000005 or 0.0000005.

If the learning rate is 5e-6, the number of steps is about 10000 to 20000. [Learning Rate Calculator](https://colab.research.google.com/drive/1qzweYEMIFkG6jPa04tD1MhWWOzgSnDvP?usp=sharing), from [here](https://github.com/) AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

The resolution is set to the resolution to be trained, preferably using an aspect ratio of 1: 1.

If the dataset has long or horizontal bars, check `Split cversized images into two`.

## Number of supernet layers

For example, the default structure ``1,2,1`` is a miniature neural network with ``1` as the input & output and ``2` as the middle layer.

There are two design directions for the supernet structure, wider or deeper

Wider hypernets are more cheerful and suitable for understanding new things, such as ``1,4,1`` which is a wide hypernet.

The deeper supergrid is more socially fearful and more suitable for summarizing data, which simply means that it is more effective for learning about drawing style and so on, for example, ``1,1.5,1.5,1.5,1`` is the deep supergrid.

Also, please note that the total number of supernets is the amount of data in this supernet, and the larger the amount, the larger the trained supernet file will be.

    The pt file for `1,2,1` is about 83.8MB, and `1,2,2,1` is about `167MB`. If you want to expand it, you must pay attention to whether there is enough disk space, otherwise it is easy to blow up the disk. [^16]

Another thing to keep in mind when designing a supernet structure is to make your design look more like an hourglass than a bucket, a ``1,2,3,2,1`` supernet may end up less effective than a ``1,2,2,2,1`` supernet.

Another point is that, compared to ``1,3,1``, ``1,1.5,1.5,1`` has a deeper network layer than ``1,3,1``, which may be more advantageous in terms of training in drawing style

If you expect to choose a more aggressive solution, you can try a structure like ``1, 0.75, 0.5, 0.5, 0.5, 0.75, 1``, although it is the same as the total amount of ``1,3,1``, but it is deeper.

The layer structure is still recommended 1, 2, 1, but if you want to extend it, you can try 1, 2, 2, 1.[^16]

**Add layer normalization option**

If checked, add layer normalization after every fully connected layer.

It is useful to prevent overfitting of the hypernetwork and to make the training more stable.

LN is useful to prevent overfitting and make training more stable, but according to previous tests, with linear on LN, it may not learn anything and may come out as a blank packet. With the activation function, LN is now available. [^16]

Swish does not seem to need to have LN on, because Swish is slow to train, so it should use a higher learning rate.

**USE Dropout**

Enable Dropout to prevent overfitting of the supernetwork, custom dropout ratios are not currently supported, the default is 0.3

On a 1,2,1 layer structure, dropout may only be useful for small data sets.

## Activation functions activation functions

Activation functions perform nonlinear transformations on the input, enabling them to learn and perform more complex tasks, and making deep neural networks more expressive.

Supports relu, leakyrelu, and linear. Choosing linear has the same effect as **no activation function**.

Swish is better than Relu and Linear, Swish is suitable for larger networks, while Elu may give better results for 1,2,1 hypernetworks. [^16]

```
The activation function is the source of nonlinearity in neural networks. If the activation function is removed, the weights and biases of the neural network will only be linearly transformed (no matter how many layers the neural network has, the composite of linear operations is still a linear operation) and the final result is only equivalent to a single-layer linear model.
```

![hypernet comparison chart](https://user-images.githubusercontent.com/75739606/197823893-88a3bece-9100-41d1-8f07-bc6d9b3aa92c.jpg)
> hypernet comparison chart, the woman in picture is 24 years old

About [activation function](https://www.geeksforgeeks.org/activation-functions-neural-networks/), [wiki](https://en.wikipedia.org/wiki/Activation_ function)

## Additional notes [^16]

With LN selected activation function turned on and Dropout turned on, the hypernet is not as easy to overfit as before, but may require a larger learning rate for learning.

[^1]:[Water miner's md notes on hypernets](https://t.me/StableDiffusion_CN/444535)

[^2]:[Related English guide](https://rentry.org/hypernetwork4dumdums)

[^16]:[Hypernetwork painting style training experience sharing](https://shiina-h.notion.site/shiina-h/Hypernetwork-1fc0b60645284c5e96bb41b583a4e86f)