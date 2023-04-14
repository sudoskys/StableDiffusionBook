# 姿态骨骼控制

ControlNet 可以指定生成图像的姿势、深度信息、轮廓等信息，从而提高生成图像的质量。要使用 ControlNet，需要安装 Stable Diffusion 插件，并在插件中输入您想要生成的图像的描述和条件。

打开 WebUi 的扩展选项卡，选择第三个从链接安装的标签页，输入 `https://jihulab.com/hunter0725/sd-webui-controlnet` 拉取插件文件（请确定文件路径不包含中文）。

## 流程介绍

![pose](https://github.com/sudoskys/StableDiffusionBook/blob/main/resource/controlnet_pose.png?raw=true)

操作中有两个流程：**从样例图片生成 深度图/骨架/轮廓 控制网**，和 **以控制网为基准的再生成。**

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

### openpose keypoints

关于 openpose 预制图上面的标注点，下面这幅图可以很好解释：

![exp-535](https://user-images.githubusercontent.com/75739606/232018385-843c584e-a3a1-4aaa-adfa-9741a59ef907.jpg)

## 使用 ControlNet

![Use](https://github.com/sudoskys/StableDiffusionBook/blob/main/resource/controlnet_usage.png?raw=true)

打开 T2I/I2I 面板底部的选项卡，然后勾选“启用”（如果显存不足，需要额外勾选“LowVram”）。接下来，选择相应的预处理器和控制网模型，并上传样例图（推荐上传真人图片或 3D 模型），以生成控制网。如果你已经有了骨骼图或其他控制网，则预处理器请选择“None”，直接上传控制网即可。

完成以上步骤后，在启用条件下就可以直接生成所需内容。

骨骼图可以自己制作！也可以使用 [avatarposemaker](https://avatarposemaker.deezein.com/) 来摆弄。

## 使用 ControlNet 操作线稿

### 上色任务

我们主要利用 canny 边缘检测上色完成这项任务。

下面是 Blilbili@大江户战士 的视频教材

<iframe src="//player.bilibili.com/player.html?aid=568208936&bvid=BV1pv4y18793&cid=1046275718&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>

### 提取线稿

要使用 Controlnet 提取线稿，确保安装 Additional Networks 插件加载线稿风格 Lora 模型。在进行线稿提取前，请确保图像的底模风格没有特别固定。

[(线稿/線画/マンガ風/漫画风) Style 线稿模型](https://civitai.com/models/16014)

如果效果不佳，可以尝试使用低分辨率输入图像或高清修复工具进行后期处理。

下面是 Blilbili@大江户战士 的视频教材

<iframe src="//player.bilibili.com/player.html?aid=568016537&bvid=BV1Nv4y1a7ts&cid=1041074740&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="100%" height="600"> </iframe>

## 使用 ControlNet 混合图像

ControlNet是一种经过优化的模型，它类似于inpainting、depth2depth、pix2pix等模型，但是有一个额外的条件输入。在训练模型时，会使用不同的条件控制来区分不同的模型，例如canny、normal、hed等等。因此，当你提供自己的草图和提示时，经过优化的ControlNet模型能够为你展示与之相应的sd1.5等效图像或者你定制的模型。这使得使用自定义模型变得更加简单。[^1]

ControlNet执行img2img非常出色，它并不仅仅是简单地叠加像素。它还会从文本提示中学习概念，并以一种合理的方式混合两个图像条件，从而生成高质量的图像。

![shot](https://user-images.githubusercontent.com/75739606/232014352-6d768824-ffbf-411f-ae63-06b58314f2c3.png)

## 与 Blander 结合使用

[Blender+SD设计辅助插件Seg管理器.zip](https://github.com/sudoskys/StableDiffusionBook/files/11231765/SegManager.zip)
>By S1AI群(280531552) @叶月葵

<video controls>
      <source id="mp4" src="https://user-images.githubusercontent.com/75739606/232020476-c0c1d5c4-78c0-4a13-8926-ba3fbc8d16a9.mp4" type="video/mp4">
</videos>


[^1]:[ControlNet在Img2Img中的惊人效果](https://www.reddit.com/r/StableDiffusion/comments/1152ius/mindblowing_controlnet_trick_mixed_composition/)

