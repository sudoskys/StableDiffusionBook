▎炼丹调参简单大纲

人物
|-表情
|-头发
|-眼睛
|-衣着
|-状态
|-姿势位
|-镜头位
场景
|-广狭选择
|-光影选择
|-背景主体
|-人物事件地
数据限定
|-绘画类型
|-评价限定
|-联想元素
事件
|-缩写词
|-sfw/nsfw
参数
|-出图尺寸，宽了人可能会多
|-step,越高越具象

要匹配好姿势，镜头和人物才不畸形，有时候需要限定量词，多人物时要处理空间关系和 prompt 遮挡优先级。人数->人物样貌->环境样式->人物状态
模糊的说是：限定好的数据范围内相似样本越多，越稳定。
More on https://rentry.co/voldy



▎关于 Tag
所有 Tag 可以上 danbooru  (https://gelbooru.com/index.php?page=tags&s=list) 查
NSFWTag可以参考 https://github.com/scooderic/exhentai-tags-chinese-translation



▎cuda 是否可用、以及 torch 对应的 cuda 的版本
打开命令窗，输入 python 进入，分行输入
import torch
print(torch.__version__)
print(torch.cuda.is_available())
查看torch对应的cuda版本
torch.version.cuda
输入 ctrl + z 退出
#LOGEXP
#显卡相关



▎速查参考
https://t.me/StableDiffusion_CN/2263
https://t.me/StableDiffusion_CN/4823
https://t.me/kawaii_fox_channel/426
推荐选Tag
t.me/StableDiffusion_CN/31994
t.me/StableDiffusion_CN/37789
推荐选成品模板
https://aibooru.online/
图片推测Tag
@LenKiMo_Bot 需要复制后右击纯文本格式化才能正常复



▎资源表
关于WebUi的FAQ  (https://gist.github.com/crosstyan/f912612f4c26e298feec4a2924c41d99)
https://t.me/StableDiffusion_CN/13436
官方Wiki 
https://rentry.co/voldy
第三方部署教程 (https://blog.xcwosjw.com/archives/3/)
NevTag 传送门 
https://t.me/StableDiffusion_Show/2
Prompts 括号转换
- @M2NM2NBot
图片推测Tag
- @LenKiMo_Bot
调参快捷面板
https://t.me/StableDiffusion_Show/17
原版Colab
https://t.me/StableDiffusion_CN/33087
Webui colab
https://t.me/StableDiffusion_CN/9763
Leak Part1
https://t.me/StableDiffusion_CN/4917
Leak Part2
https://t.me/StableDiffusion_CN/30442
