
## 安装 Stable-diffusion-webui 开源框架

此教程参考了 crosstyan[^2]

仓库地址 https://github.com/AUTOMATIC1111/stable-diffusion-webui

#### 安装

[Dependencies](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Dependencies)

**然后看这个**
https://github.com/AUTOMATIC1111/stable-diffusion-webui#installation-and-running

**安装的问题看**
https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki


#### 安装自助

仓库的一键安装会创建 虚拟环境，然后启动 [launch.py](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/launch.py)


安装依赖，如果你想使用conda,可以自己运行 launch 安装依赖！！

**网络问题**

git 报错
HTTP_PROXY 和 HTTPS_PROXY 环境变量，或者使用clash 的tun模式. 或者把 git clone 的仓库源换成 huggingface..

**时间**

设镜像或者挂代理，依赖项 >2GB,请做好准备，而且对于Windows,**依赖默认安装在C盘**！

#### 更新框架

Git拉取请使用 `git pull` 更新。

如果是整合或者其他，请向上游（谁分发的）索取。

------

## 安装 Novel Ai 的原版后端

此教程来自 sanae[^1]

### 硬件需求

一台拥有一张至少有11G 显存的NVIDIA GPU的linux系统的x86设备。

#### 软件需求

- NVIDIA驱动(CUDA 11.6 Toolkit)

- Docker 19+

- nvidia-container-toolkit

#### 安装docker


```
curl -fsSL https://get.docker.com | bash
```

**国内镜像**

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

#### 安装nvidia-container-toolkit

**Ubuntu, Debian**

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit

sudo systemctl restart docker
```

**CentOS/RHEL**
```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.repo | sudo tee /etc/yum.repos.d/nvidia-docker.repo

sudo yum install -y nvidia-container-toolkit

sudo systemctl restart docker
```

#### 驱动

**安装显卡驱动**

使用 `nvidia-smi` 查看显卡状态。

使用 `docker run --help | grep -i gpus` 确认nvidia-container-toolkit安装成功。



#### 配置并运行

**模型**

- 下载模型文件
 
 [SharePoint](https://sanae1-my.sharepoint.cn/:u:/g/personal/kaze_sanae1_partner_onmschina_cn/Eezd5aJPk9xHmb5iVcXb768Bm5AUEHp2DM9biozM-54y9w?e=T6DaPd)

- 解压相关文件
 
`tar -zxvf novelai.tar.gz`

**docker 镜像**

- [下载相关文件](https://sanae1-my.sharepoint.cn/:u:/g/personal/kaze_sanae1_partner_onmschina_cn/ESe-AjlXEhNDvnwOyZtBjKkBnLa69b8qTYi2dGzJmKq5IA?e=EQol8q)


- 导入 Docker 镜像

`docker load -i novelaidocker.tar`

- 运行

```
docker run --gpus all -d -p 80:80 -v 解压的NovelAI位置:/root -e DTYPE="float32" -e AMP="1" -e MODEL="stable-diffusion" -e DEV="True" -e MODEL_PATH="/root/stableckpt/animesfw-latest" -e MODULE_PATH="/root/stableckpt/modules" -e TRANSFORMERS_CACHE="/root/transformer_cache" -e SENTRY_URL="" -e ENABLE_EMA="1" -e VAE_PATH="/root/stableckpt/animevae.pt" -e BASEDFORMER="1" -e PENULTIMATE="1" novelai:latest gunicorn main:app --workers 1 --worker-class uvicorn.workers.UvicornWorker --bind 0.0.0.0:80
```

如果希望包含 nsfw 内容，则把`-e MODEL_PATH="/root/stableckpt/animesfw-latest"` 改成 `-e MODEL_PATH="/root/stableckpt/animefull-latest"`

解压的 Novelai 位置替换为你实际解压 NovelAI 模型相关文件出来的 novelai 文件夹的位置，如 /root/novelai


- 容器状态

查询出容器ID `docker ps`

查看容器LOG `docker logs [Container ID]`

出现 “Application startup complete.” 即代表程序已经就绪

**请求API**


参考代码，具体参照leak的前端后端项目，以及其中的`sd-private\hydra-node-http\main.py`

`prompt` 中 `masterpiece, best quality `, 开头对应原版 `web Add Quality Tags`  选项，不建议删除，后面直接跟自己 `prompt` 即可

uc 部分对应 *web Undesired Content*，建议保留默认

sampler 是采样方法，可选 `plms/ddim/k_euler/k_euler_ancestral/k_heun/k_dpm_2/k_dpm_2_ancestral/k_lms`

seed 是种子，自己随机一个整数数字，不然一直会出一样的结果

n_samples 代表要生成几张图片


```python
import requests
import json
import base64
import random

endpoint = "http://10.10.12.67/generate"
data = {"prompt": "masterpiece, best quality, brown red hair,blue eyes,twin tails,holding cat", "seed": random.randint(0, 2**32)
,"n_samples":1,"sampler":"ddim","width":512,"height":768,"scale":11,"steps":28,"uc":"lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry"}


req = requests.post(endpoint, json=data).json()
output = req["output"]

for x in output:
  img = base64.b64decode(x)

  with open("output-" + str(output.index(x)) + ".png", "wb") as f:
    f.write(img)
```

[^1]:[NovelAI原版部署教程](https://telegra.ph/NovelAI%E5%8E%9F%E7%89%88%E9%83%A8%E7%BD%B2%E6%95%99%E7%A8%8B-10-07)

[^2]:[关于 AUTOMATIC1111 /stable-diffusion-webui 的 FAQ:](https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)


