 
# 架炉生火

这节介绍 Debug 和一些 关于 WebUI 网页应用模型参数 的优化方案，让它更好用。本节演示所用模型为 某个模型 展开教程。**部分源教程来自：[^2]**

推荐经常从远端代码库拉取代码 `git pull` 更新 WebUI 网页应用。

!!! info "版权"
    为避免涉及版权纠纷，本仓库不提供 NAI 的模型链接。
    可以尝试关注中文社区 t.me@StableDiffusion_CN_WIKI 或 打开整合包自行查看 `model` 目录。

SDWebUi是一个框架，除了 NAI 模型外还有许多[其他模型](https://rentry.org/sdmodels)


[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)


## Pickle 安全提示

`.ckpt` 和 `.pt` 文件中都可能有恶意的 pickle，

WebUi *内置了安全检查*，如果你使用 Automatic1111 的WebUi, 在这方面做了一些验证。对于不安全的文件，会提示 
```
The file may be malicious, so the program is not going to read it. 
You can skip this check with --disable-safe-unpickle commandline argument.
```

另外，你可以在 [这里](https://github.com/sudoskys/StableDiffusionBook/blob/main/docs/ckpt_safe) 看到一个简单的检查脚本。

```python
    raise pickle.UnpicklingError("global '%s/%s' is forbidden" %
                                 (module, name))
```
如果你运行脚本得到了类似 "global something.something is forbidden" 这意味着检查点试图做坏事，可能有危险。

不一定保证百分百安全，甚至也会误报一些文件，所以建议从可靠信源下载模型。

相关项目 [pickle_inspector](https://github.com/lopho/pickle_inspector) 。

[Pickle的介绍](https://docs.python.org/3/library/pickle.html)，Python object serialization

```
pickle模块不安全。只解pickle您信任的数据。
可以构造恶意pickle数据，在解pickle期间执行任意代码。永远不要解pickle可能来自不可信来源或可能被篡改的数据。
```

## 模型选调

模型使用的数据集和标签对于效果影响非常重要，在使用之前要先了解数据来源。

TODO:

https://huggingface.co/lambdalabs/sd-pokemon-diffusers

https://huggingface.co/Onodofthenorth/SD_PixelArt_SpriteSheet_Generator


### 某个模型

这里以某模型简单谈论一下猜测。

- 猜测

对于某个著名模型，`masterpiece` 有可能是为了适应商业需求人工标记的 Tag.

猜测 full 模型投入了所有的数据，未经清洗。而 sfw 模型则数据更加精确，应该受过清洗。

这导致了 full 模型的不稳定。

在 “事件2” 中，我们看到了很多其他的模型，比如 `wallpaper` 模型，它们应该使用了不同的数据集进行训练。

经过讨论，我们认为 有些模型是根据 `rating` 进行分类的。


但是事实上我们并不知道细节，所以以上只是猜测并且**不可信**的。
