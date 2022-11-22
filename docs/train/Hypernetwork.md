# Hypernetwork [^13]

Hypernetworks 是一种新颖的概念，用于在不触及任何权重的情况下微调模型。

!!! tip
    在设置内勾选 **Move VAE and CLIP to RAM when training hypernetwork. Saves VRAM.** 的效果是转移 VAE 到 RAM, 而不是卸载，

    如需卸载，请重命名。

你可以在 `train` 选项卡训练 `Hypernetworks`，训练方式与  `Textual Inversion` 相同。

注意，应该使用非常低的学习率，例如 0.000005 或 0.0000005.

如果学习率是 5e-6 ，步数大约 10000 到 20000 。[学习率计算器](https://colab.research.google.com/drive/1qzweYEMIFkG6jPa04tD1MhWWOzgSnDvP?usp=sharing)，来自 [这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

分辨率设置为要训练的分辨率，最好使用 1: 1 的纵横比。

如果数据集的图片是长条或者横条，请勾选 `Split cversized images into two`

如果你还没有打标签，可以勾选 `Using deepbooru for caption` ，来让 deepbooru 识别标签。

## hyper network layer structure

If write "1, 2, 1", hypernetworks are composed of 2 fully connected layers whose intermediate dim is 2x, which is same as up to now.

The more you add the number, like "1, 2, 4, 2, 1", the more the structure of hypernetworks becomes deeper. Deep hypernetworks are suited for training with large datasets.

层结构目前还是推荐 1，2，1，如果想扩展的话，可以尝试 1，2，2，1.[^16]

1，2，1 的 pt 文件大概是 83.8MB，1，2，2，1 大概是 167MB。如果要扩展的话，一定要注意盘空间是否足够，否则很容易爆盘。[^16]

## Add layer normalization

If checked, add layer normalization after every fully connected layer.

防止超网络过拟合，使训练更加稳定。

LN 对防止过拟合使训练更加稳定是有意义的，但是根据之前的测试，linear 开开启 LN 的情况，可能学不进去东西，出空包弹。使用激活函数后，LN 现在是可使用的。[^16]

Swish 似乎并不需要开启 LN，因为 Swish 的训练速度慢，所以应该使用更高的学习率。

## USE Dropout

启用 Dropout 可以防止超网络过拟合，目前不支持自定义 dropout 比率，默认为 0.3

在 1，2，1 的层结构上，dropout 可能只会对小数据集有帮助。

## 激活函数 activation functions

关于 [激活函数](https://www.geeksforgeeks.org/activation-functions-neural-networks/)，[维基](https://en.wikipedia.org/wiki/Activation_function)

激活函数是神经网络中非线性的来源。如果去掉激活函数，神经网络的权重和偏差只会进行线性变换（不管神经网络有多少层，线性运算的复合还是线性运算的），最终的效果只相当于单层的线性模型。

激活函数对输入进行非线性变换，使其能够学习和执行更复杂的任务，深层神经网络表达能力更加强大。

在 10/21/22 的一个 [合并请求](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/3199) 中，这项功能被 discus0434 提出。

支持 relu、leakyrelu 和 linear 。选择 linear（线性） 的效果和没有激活函数一样。

Swish 对比 Relu 和 Linear 要更好，Swish 适合更大的网络，而 Elu 可能对 1，2，1 的超网络带来更好的结果。[^16]

[相关的英文指南](https://rentry.org/hypernetwork4dumdums)

[相关的英文讨论](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

![hypernet 对比图](https://user-images.githubusercontent.com/75739606/197823893-88a3bece-9100-41d1-8f07-bc6d9b3aa92c.jpg)
<!--
![hypernet 对比图](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/exp_hypernet.jpg)
-->
> hypernet 对比图，the woman in picture is 24 years old

## 额外注意 [^16]

在开启 LN 选用激活函数并开启 Dropout 后，超网络没以前那么容易过拟合，但是可能需要更大的学习率进行学习。

[Hypernetwork 画风训练经验分享](https://shiina-h.notion.site/shiina-h/Hypernetwork-1fc0b60645284c5e96bb41b583a4e86f)
