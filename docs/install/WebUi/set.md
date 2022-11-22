# 安装 Stable-Diffusion-webui 开源框架

此教程参考了 crosstyan[^2] 的 [FAQ](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#prompt-editingmixing)

WebUi 的官方代码仓库地址为 https://github.com/AUTOMATIC1111/stable-diffusion-webui

[官方英文安装教程](https://github.com/AUTOMATIC1111/stable-diffusion-webui#installation-and-running)

[官方给定的依赖列表](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Dependencies)

## 硬件要求

WebUi 最低能用 N 卡显存为 2GB

WebUi 最低可用 N 卡显存为 4GB

在开始前，打开 CMD， 输入 `nvidia-smi` 看看机器的英伟达显卡是否正常运作。

![效果。png](https://s1.ax1x.com/2022/10/10/xtdNNR.png)
>这里是 9 月份效果（现在快了不少）

## 安装

!!! tip
    推荐有能力的同学阅读一下 [官方 Wiki](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki) 的内容。

    注意！Ai 需要的依赖大小很大，需要做好心理准备。总计依赖大小约几 G，在中国环境可能很慢！

    而且对于 Windows, **依赖默认安装在 C 盘**，请确保空间充足。

    不推荐使用 整合包，会导致升级困难。

WSL 不可以迁移，如果 C 盘不够，寄！

### **Windows 安装**

在 Windows 上的安装流程如下。

**安装 Python**

安装 [Python 3.10.6](https://www.python.org/downloads/windows/)，安装时选中“Add Python to PATH”

>为什么整合包不需要安装 Python？他们的脚本中指定了同目录下的环境，阅读之后的文档就可以知道

!!! tip "如果你在安装依赖时遇到了网络问题，需要按照以下步骤切换至国内镜像"

    打开 CMD 命令行（Win+R 运行 cmd），输入`Python` 回车，如果能进入 Python 就 ctrl+z （或通过输入 `exit()` ) 退出。
    然后输入以下命令设置镜像。
    ```bash
    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
    ```

**安装 Git**

在 [GitScm](https://git-scm.com/download/win) 下载 Git 安装包并安装，然后在开始菜单中找到 `git bash`

或打开想要放置的目录，右键打开快捷菜单中寻找 `Git Base Here`

亦或在想要运行的目录下打开 CMD

**拉取仓库**

在命令框中运行 `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git` 拉取仓库，这里如果如果失败请设置代理。

使用 Git 下载的源代码，可以直接使用 `git pull` 更新代码。推荐经常更新，并关注社区讨论。

如果是整合或者其他，请向上游（谁分发的）索取。

!!! tip
    如果你不会设置代理，可以使用以下命令拉取镜像

    `git clone https://hub.fastgit.org/AUTOMATIC1111/stable-diffusion-webui.git `

**尝试安装**

将 4G 或 7G 或 其他模型 `model.ckpt` 放在 `models` 目录中，`WebUi` 目前支持在界面顶部选择模型。

（可选）将`GFPGANv1.4.pth`放在基本目录中，旁边是 `webui.py`

先确认你的显卡是可用的（看上文），然后以普通非管理员用户身份从 Windows 资源管理器 运行 `webui-user.bat` 。脚本会自动下载依赖（**一般需要 22 分钟，默认安装在 C 盘 Python 下，下载的资源为 1.98GB，解压后大小 2GB 以上**）并输出 启动网页界面 的地址。

在此区间你可以打开资源管理器，关注网络速度。在没有完成下载之前，脚本不会继续输出任何提示。（等了两小时？可以试试挂代理）

大约 30 分钟后安装完毕，程序会输出一个类似 `http://127.0.0.1:7860/` 的地址，点开即可（注意是 http, 且不指定端口的话可能会变动）。

!!! tip
    运行需要模型！

    如果你是瞄准二次元来的，请想办法搞到某个模型咯，至于模型选哪个，请看下一章节。如果你是整合包，也可以移植整合包其中的`models`文件夹。（甚至你可以直接去下载拆分整合包。...？但是直接使用整合包迟早要走自己安装的流程，因为不能更新）

    如果对其他模型感兴趣，（没错就是艺术，大概是梵高那类的）这里提供一个模型网站 [Stable Diffusion Models](https://rentry.org/sdmodels#)，可以通过下载 `2.Stable Diffusion v1.4 [4af45990] [7460a6fa] [06c50424]` 这一个模型来运行 `Stable Diffusion` 的原始模型

### Windows 从整合包迁移

使用上面的命令拉取代码但先不运行，然后将 Bat 文件和里面指定的文件夹复制到全新环境，把 `models` 和 `outputs` 以及 `embeddings` 复制到新文件夹，自行处理冲突。

!!! tip "命令"

    此处的命令指的是安装完 Git 之后的内容

    即为 `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git`

    或在无代理的情况下 `git clone https://hub.fastgit.org/AUTOMATIC1111/stable-diffusion-webui.git`

然后在新代码目录下，尝试运行原来的 Bat，如果报错，就根据提示排查是不是 NotFound 什么依赖，或者根据下面的命令说明修改 Bat 文件。

如果你不想迁移你的整合包，你也可以参考 [带图片的教程](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99#%E6%87%92%E4%BA%BA%E5%8C%85%E6%94%B9-git-%E4%BB%93%E5%BA%93%E8%BF%9B%E8%A1%8C%E6%9B%B4%E6%96%B0)

### Linux 安装

先安装依赖
```bash
# Debian-based:
sudo apt install wget git python3 python3-venv
# Red Hat-based:
sudo dnf install wget git python3
# Arch-based:
sudo pacman -S wget git python3
```

一键脚本

```bash
bash <(wget -qO- https://raw.githubusercontent.com/AUTOMATIC1111/stable-diffusion-webui/master/webui.sh)
```

完毕。

!!! info
    如果可以，尽量使用 `miniconda` （anaconda 特别巨大。...），创建一个 Python 3.10.6 的虚拟环境。

    ```bash
    conda create -n aidraw python=3.10.6

    conda activate aidraw

    COMMANDLINE_ARGS="--medvram" REQS_FILE="requirements.txt" python launch.py
    ```
[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)
