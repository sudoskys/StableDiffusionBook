# 工具速查


### 调参简单大纲

竖着看

|人物|场景|数据限定|事件|
|----|------|-----|-----|
|表情| | | |
|头发|广狭选择|绘画类型|缩写词|
|眼睛|光影选择|评价限定|sfw/nsfw|
|衣着|背景主体|联想元素||
|状态|人物事件地|||
|姿势位||||
|镜头位||||

还有一个 `参数`

比如出图尺寸宽了人可能会多

!!! tip
    要匹配好姿势，镜头和人物才不畸形，有时候需要限定量词，多人物时要处理空间关系和 prompt 遮挡优先级。人数->人物样貌->环境样式->人物状态
    
模糊的说是：限定好的数据范围内相似样本越多，越稳定。




### 关于 Tag

[Tag在线协作](https://docs.google.com/spreadsheets/d/1zij5OzCZIaQuhAbiSayhFznjgJ3rwbaNwnUnaUMxyTQ/edit?usp=drivesdk)

所有 Tag 可以上[danbooru](https://gelbooru.com/index.php?page=tags&s=list)


NSFWTag 可以参考[Github Here](https://github.com/scooderic/exhentai-tags-chinese-translation)


### 关于显卡

cuda 是否可用、以及 torch 对应的 cuda 的版本
打开命令窗，输入 python 进入，分行输入
```
import torch
print(torch.__version__)
print(torch.cuda.is_available())
```

**查看torch对应的cuda版本**
```
torch.version.cuda
```
输入 ctrl + z 退出



