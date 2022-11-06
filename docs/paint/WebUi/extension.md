# 扩展


## 绘画插件

安装完毕重启程序。**目前WebUi已经自带扩展管理器界面。**

### 随机艺术家插件

[项目地址](https://github.com/yfszzx/stable-diffusion-webui-inspiration)

```bash
cd extensions
git clone https://github.com/yfszzx/stable-diffusion-webui-inspiration
```


### 美学权重插件

[项目地址](https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients)

```bash
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients extensions/aesthetic-gradients
```


### 历史记录画廊

[项目地址](https://github.com/yfszzx/stable-diffusion-webui-images-browser)

```bash
cd extensions
git clone https://github.com/yfszzx/stable-diffusion-webui-images-browser
```


### Wildcards 通配符

[项目地址](https://github.com/AUTOMATIC1111/stable-diffusion-webui-wildcards)

允许使用类似 `__name__` 提示的语法，以从名为 `name.txt` 的文件中获取随机一行。


### Deforum

[项目地址](https://github.com/deforum-art/deforum-for-automatic1111-webui)

Deforum 的官方API，一个用于 2D 和 3D 动画的扩展脚本，supporting keyframable sequences, dynamic math parameters (even inside the prompts), dynamic masking, depth estimation and warping.

### Stable-Diffusion-Video2Video

将 视频 投入 Img2Img,输出带有关键帧的视频。

https://github.com/Leonm99/Stable-Diffusion-Video2Video

### Img2img Video

https://github.com/memes-forever/Stable-diffusion-webui-video

### Artists To Study

把 https://artiststostudy.pages.dev 添加到 WebUi

https://github.com/camenduru/stable-diffusion-webui-artists-to-study

### 美学权重评分器

计算生成图像的美学分数

https://github.com/tsngo/stable-diffusion-webui-aesthetic-image-scorer

### Tokenizer

stable-diffusion-webui的一个扩展，增加了一个标签，让你预览CLIP模型将如何标记你的文本。

https://github.com/AUTOMATIC1111/stable-diffusion-webui-tokenizer


## 自定义脚本

此主题的内容可能不会即时更新到此处，[源地址](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts#prompt-interpolation)

### Depth Maps for Stable Diffusion

这个脚本是AUTOMATIC1111的Stable Diffusion Web UI的一个插件，可以从生成的图像中创建深度图。

https://github.com/thygate/stable-diffusion-webui-depthmap-script

### sd-lexikrea

Automatic1111 Stable Diffusion 的脚本，用于从 krea 和 lexica 中提取提示。 

https://github.com/Vetchems/sd-lexikrea


### 自动补全

这里有一个项目，可以使SD做到类似 NAI 的补全效果。

https://github.com/DominikDoom/a1111-sd-webui-tagcomplete

### Advanced prompt matrix

https://github.com/GRMrGecko/stable-diffusion-webui-automatic/blob/advanced_matrix/scripts/advanced_prompt_matrix.py

安装后可以使用以下语法
```
<cyber|cyborg|> cat <photo|image|artistic photo|oil painting> in a <car|boat|cyber city>
```

### XYZ 绘图脚本

生成一个 .html 文件以交互浏览图像集。 使用滚轮或箭头键在 Z 维度中移动。

https://github.com/xrpgame/xyz_plot_script

### Embedding to PNG

将一个已经存在的 embeddings 转换为图片。

https://github.com/dfaker/embedding-to-png-script

### Random Steps and CFG

https://github.com/lilly1987/AI-WEBUI-scripts-Random

