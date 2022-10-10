# AiDraw

首先，不是N卡的可以关了(因为用到了 Cuda)，除非想用 Cpu 跑。AMD卡 请读 [这里](https://rentry.org/ayymd-stable-diffustion-v1_4-guide)

N卡显存 > 2GB
内存大于 4GB


请点击目录查看下一节。

## 新闻

仓库：https://github.com/AUTOMATIC1111/stable-diffusion-webui

!!! tip
    目前stable-diffusion-webui迭代非常快，每天都会有大量更新，因此建议每天都拉取最新代码。

以下是新闻和测试内容。

-------

10/10 更新：
测试发现,新版本 Webui 优化显存占用(20xx—>10xx)，关闭浏览器和TG的硬件加速后，即使是4GB的  RTX2050 也可以启动 --medvram 模式！而且很快.