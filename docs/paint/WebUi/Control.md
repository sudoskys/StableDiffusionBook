# 姿态骨骼控制

ControlNet 可以指定生成图像的姿势、深度信息、轮廓等信息，从而提高生成图像的质量。要使用 ControlNet，需要安装 Stable Diffusion 插件，并在插件中输入您想要生成的图像的描述和条件。

打开 WebUi 的扩展选项卡，选择第三个从链接安装的标签页，输入 `https://jihulab.com/hunter0725/sd-webui-controlnet` 拉取插件文件（请确定文件路径不包含中文）。

## 流程介绍

![pose](https://github.com/sudoskys/StableDiffusionBook/blob/main/resource/controlnet_pose.png?raw=true)

操作中有两个流程：从样例图片生成 深度图/骨架/轮廓 控制网，和 以控制网为基准的再生成。

下面我们来配置这个插件

首先你需要从 https://huggingface.co/lllyasviel/ControlNet/tree/main/models 下载深度预测模型和骨架预测模型，用于从样例图片中提取控制网。对应模型放入 `extensions\sd-webui-controlnet\annotator` 的相应文件夹中以支持从样例生成对应产物的功能。

然后在 https://huggingface.co/webui/ControlNet-modules-safetensors/tree/main 中下载控制网模型，将 ControlNet 模型（`.pt`，`.pth`，`.ckpt`，`.safetensors`）放在 `models/ControlNet` 文件夹中，重启 webui。

## 关于控制图的注解

openpose 是指人体姿势的标注，depth 是指图像的深度信息，scribble 是指图像的轮廓或草稿，mlsd 是指图像的多边形分割。您可以使用这些条件来控制生成图像的形状、位置、角度等细节。您可以在插件中选择不同的条件类型，并用鼠标在画布上绘制或导入相应的数据。canny 指边缘检测，达成上色的效果。

|控制网| 样例图 | 控制网 | 生成物 |
|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|
|*| (no preprocessor) |  <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/bal-source.png?raw=true"> | <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/bal-gen.png?raw=true"> |
|*| (no preprocessor) |  <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/dog_rel.jpg?raw=true"> | <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/dog_rel.png?raw=true"> |
|canny 边缘检测上色|<img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/mahiro_input.png?raw=true">  |  <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/mahiro_canny.png?raw=true"> | <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/mahiro-out.png?raw=true"> |
|hed 预处理器边缘检测/创意|<img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/evt_source.jpg?raw=true">  |  <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/evt_hed.png?raw=true"> | <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/evt_gen.png?raw=true"> |
|openpose 预处理器控制动作|<img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/an-source.jpg?raw=true">  |  <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/an-pose.png?raw=true"> | <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/an-gen.png?raw=true"> |
|depth 深度图控制画面结构|<img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/sk-b-src.png?raw=true">  |  <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/sk-b-dep.png?raw=true"> | <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/sk-b-out.png?raw=true"> |
||<img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/nm-src.png?raw=true">  |  <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/nm-gen.png?raw=true"> | <img width="256" alt="" src="https://github.com/Mikubill/sd-webui-controlnet/blob/main/samples/nm-out.png?raw=true"> |

除此之外还有 MLSD 线段图还有 Scribble 手绘图。

## 使用 ControlNet

![Use](https://github.com/sudoskys/StableDiffusionBook/blob/main/resource/controlnet_usage.png?raw=true)

在 T2I/I2I 面板底部打开选项卡，然后勾选启用（如果显存不够需要额外勾选 LowVram）。

然后选择对应的预处理器和控制网模型，然后上传样例图（推荐上传真人图片或 3D 模型）生成控制网。如果你已经有了骨骼图或者其他控制网，预处理器请选择 None 直接上传控制网。

然后在启用条件下就可以直接生成。

骨骼图可以自己制作！也可以使用 [avatarposemaker](https://avatarposemaker.deezein.com/)

<iframe src="//player.bilibili.com/player.html?aid=568208936&bvid=BV1pv4y18793&cid=1046275718&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>
