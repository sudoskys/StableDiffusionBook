# Hypernetwork 

Hypernetworks[^2] 是一种新颖的概念，用于在不触及任何权重的情况下微调模型。

此项技术被广泛用于画风模仿中，和 Textual Inversion 相比，泛化效果更好。

这个页面主要是为你实际操作提供一份指南。

如果你在寻找一些更加详细深入的 Hypernetwork 说明，请看 [这篇社区贴文](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670).

!!! tip "关于 VAE"
    据说 VAE 会破坏训练结果。

    这里指出：在设置内勾选 **Move VAE and CLIP to RAM when training hypernetwork. Saves VRAM.** 的效果是转移 VAE 到 RAM, 而不是卸载，

    如需卸载，请重命名。

你可以在 `train` 选项卡训练 `Hypernetworks`，训练方式与  `Textual Inversion` 相同。

## 预处理训练集

与训练人物不同 [^1], 训练超网并不需要在意背景是否干净或者主体是否突出，虽然训练素材越多效果越好，但是请注意以下信息

- 画风固定，譬如抛弃画师早期作品
- 去除文字或水印
- 分辨率不要过小

**裁切处理**

目前 WebUI 已经集成了图像预处理功能，可以自动裁切缩放图片。

如果你需要确保画面主体明确，推荐自己手动裁切来保证主体在画面中心。[批量裁切工具](https://www.birme.net)

因为是风格训练，并不用担心训练主体是否非对称等问题，所以推荐打开图像翻转来倍增数据集，提升训练效果。[^1]

**贴标签**

如果你还没有打标签，可以勾选 `Using deepbooru for caption` ，来让 deepbooru 自动生成标签。[^1]

如果你想手动打标签，推荐 [GUI 贴标签工具](https://github.com/arenatemp/sd-tagging-helper)

## 参数

注意，应该使用非常低的学习率，例如 0.000005 或 0.0000005.

如果学习率是 5e-6 ，步数大约 10000 到 20000 。[学习率计算器](https://colab.research.google.com/drive/1qzweYEMIFkG6jPa04tD1MhWWOzgSnDvP?usp=sharing)，来自 [这里](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2670)

分辨率设置为要训练的分辨率，最好使用 1: 1 的纵横比。

如果数据集的图片是长条或者横条，请勾选 `Split cversized images into two`

## 超网层数

关于超网层结构，你可以简单理解成一个微型神经网络，譬如这个默认结构``1,2,1``左右两个 `1` 分别是输入&输出，`2` 是中间层。

超网结构有两种设计方向，分别是更宽或是更深

更宽的超网更加开朗，适合用于认识一些新事物，譬如``1,4,1``这种就属于宽超网。

更深的超网更加社恐，更加适合归纳总结数据，简单讲就是对画风之类的学习效果更加好，譬如``1,1.5,1.5,1.5,1``就是深超网。

另外，请注意，将超网数字加总就是这个超网的数据量，量越大，训练出来的超网文件就越大。

    `1，2，1` 的 pt 文件大概是 83.8MB，`1，2，2，1` 大概是 `167MB`。如果要扩展的话，一定要注意盘空间是否足够，否则很容易爆盘。[^16]

在超网结构设计里需要注意的另一点就是，请让你的设计看起来更像是沙漏而不是水桶，一个``1,2,3,2,1``的超网可能最终效果还不如一个``1,2,2,2,1``的超网。

另外一点就是，和``1,3,1``相比，``1,1.5,1.5,1``虽然它们总量是一样的，但是后者有着更深的网络层级，在画风训练上可能更具优势

如果你期望选用更加激进的方案可以试试``1, 0.75, 0.5, 0.5, 0.5, 0.75, 1``这样的结构，虽然它和``1,3,1``总量是一样的，但是它更加深

层结构目前还是推荐 1，2，1，如果想扩展的话，可以尝试 1，2，2，1.[^16]

**Add layer normalization 选项**

If checked, add layer normalization after every fully connected layer.

它的作用防止超网络过拟合，使训练更加稳定。

LN 对防止过拟合使训练更加稳定是有意义的，但是根据之前的测试，linear 开开启 LN 的情况，可能学不进去东西，出空包弹。使用激活函数后，LN 现在是可使用的。[^16]

Swish 似乎并不需要开启 LN，因为 Swish 的训练速度慢，所以应该使用更高的学习率。

**USE Dropout**

启用 Dropout 可以防止超网络过拟合，目前不支持自定义 dropout 比率，默认为 0.3

在 1，2，1 的层结构上，dropout 可能只会对小数据集有帮助。

## 激活函数 activation functions

激活函数对输入进行非线性变换，使其能够学习和执行更复杂的任务，深层神经网络表达能力更加强大。

支持 relu、leakyrelu 和 linear 。选择 linear（线性） 的效果和**没有激活函数**一样。

Swish 对比 Relu 和 Linear 要更好，Swish 适合更大的网络，而 Elu 可能对 1，2，1 的超网络带来更好的结果。[^16]

```
激活函数是神经网络中非线性的来源。如果去掉激活函数，神经网络的权重和偏差只会进行线性变换（不管神经网络有多少层，线性运算的复合还是线性运算的），最终的效果只相当于单层的线性模型。
```

![hypernet 对比图](https://user-images.githubusercontent.com/75739606/197823893-88a3bece-9100-41d1-8f07-bc6d9b3aa92c.jpg)
> hypernet 对比图，the woman in picture is 24 years old

关于 [激活函数](https://www.geeksforgeeks.org/activation-functions-neural-networks/)，[维基](https://en.wikipedia.org/wiki/Activation_function)

## 额外注意 [^16]

在开启 LN 选用激活函数并开启 Dropout 后，超网络没以前那么容易过拟合，但是可能需要更大的学习率进行学习。

[^1]:[Water miner 的关于超网的 md 笔记](https://t.me/StableDiffusion_CN/444535)

[^2]:[相关的英文指南](https://rentry.org/hypernetwork4dumdums)

[^16]:[Hypernetwork 画风训练经验分享](https://shiina-h.notion.site/shiina-h/Hypernetwork-1fc0b60645284c5e96bb41b583a4e86f)