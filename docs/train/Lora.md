# Lora

[Akegarasu 的视频指南](https://www.bilibili.com/video/BV1fs4y1x7p2)

[Github](https://github.com/Akegarasu/lora-scripts)

Lora 模型可以在 t2i 和 i2i 模式下使用，并且 Lora 模型可以和任何主模型一起使用。

## 使用 Lora 模型

在扩展标签页安装 [Lora 模型加载插件](https://github.com/kohya-ss/sd-webui-additional-networks)，然后将你的 Lora 模型放到 stable-diffusion-webui/models/lora 路径下。比如 [幻星集塔罗牌 LoRA](https://civitai.com/models/11177).

在 SD（Sketch Design）的文生图或图生图界面内，点击生成按钮下的粉色图标，即“显示额外网络”选项卡。在弹出的面板中选择“Lora”选项卡。

然后点击想要应用的 Lora 模型，它将被添加到提示语中，其格式为 `<lora: 数字>`，数字代表 Lora 模型的权重，默认为 1。

接下来点击“生成”按钮开始生成图片。

当生成完成后，将鼠标移动到 Lora 卡片的左下角或标题上方，会出现“替换预览”的红色文字。点击此处即可将刚生成的图片设置为此 Lora 模型的预览图。

## 训练 Lora 模型

这里我们以流行的 Kohya’s GUI 作为参考展开讲解：

### 准备
首先，为了训练自己的 Lora 模型，我们需要准备以下工具和素材：

1. 一台拥有足够显存（至少 8GB）的电脑；
2. 至少 100 张你想要的风格图片放在同一个文件夹中；
3. 一个基础模型，如 stable-diffusion-v1-5 或 ChilloutMix；

接下来，按照以下步骤进行操作：

1. 安装 Python 3.10，并将其添加到 PATH 环境变量中。
2. 安装 [Git](https://git-scm.com/download/win) 和 [Visual Studio 2015、2017、2019 和 2022 redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe)。
3. 运行 PowerShell 以管理员身份运行，并执行 Set-ExecutionPolicy Unrestricted 命令。
4. 关闭 PowerShell。
5. 打开一个普通用户的 PowerShell 终端，运行以下命令：

### 安装训练程序

首先下载 kohya_ss 

```shell
git clone https://github.com/bmaltais/kohya_ss.git
cd kohya_ss
```

```shell
python -m venv venv
.\venv\Scripts\activate

pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116
pip install --use-pep517 --upgrade -r requirements.txt
pip install -U -I --no-deps https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases/download/f/xformers-0.0.14.dev0-cp310-cp310-win_amd64.whl

cp .\bitsandbytes_windows\*.dll .\venv\Lib\site-packages\bitsandbytes\
cp .\bitsandbytes_windows\cextension.py .\venv\Lib\site-packages\bitsandbytes\cextension.py
cp .\bitsandbytes_windows\main.py .\venv\Lib\site-packages\bitsandbytes\cuda_setup\main.py

accelerate config
```

#### （可选）安装 CUDNN 8.6

可提高 NVIDIA 30X0/40X0 的训练速度。如果您需要安装它，请下载 [cudnn_windows](https://b1.thefileditch.ch/mwxKTEtelILoIbMbruuM.zip) 文件并解压缩到该存储库的根目录下。然后运行以下命令：

```shell
.\venv\Scripts\activate
python .\tools\cudann_1.8_install.py
```

### 启动 GUI

这里有两种方法启动 GUI，一种是通过 bat 脚本，一种是通过 Python 程序。
```shell
gui.bat --listen 127.0.0.1 --server_port 7860 --inbrowser --share
```

```shell
.\venv\Scripts\activate
python.exe .\kohya_gui.py
```

### 数据集准备




### 训练

在 GUI 中，你可以指定要使用的基础模型和风格图片文件夹，配置相关参数，如 source model（基础模型），batch size（并行数量），network dim（网络大小），learning rate（学习率）等。等待训练完成，您可以在输出文件夹中找到生成的 Lora 模型文件（以。safetensors 为后缀）。

在扩展标签页安装 [Lora 模型加载插件](https://github.com/kohya-ss/sd-webui-additional-networks)，然后将 Lora 模型文件放到 `stable-diffusion-webui/models/lora` 路径下，并重新启动 SD WebUi。然后就可以按照之前的步骤，在 SD WebUi 中使用您的 Lora 模型生成图片。

## 无 GUI 训练

https://github.com/kohya-ss/sd-scripts

## AutoDL

AutoDL 在线训练镜像：https://www.bilibili.com/read/cv21450198

LoRA 在线云端训练教程 AutoDL  https://www.bilibili.com/read/cv21450198

## 其他

[关于 Lora 的更多说明](https://huggingface.co/datasets/HuggingFace-CN-community/translation/blob/main/lora_cn.md)