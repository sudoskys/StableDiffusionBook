 
# Getting Started Painting


>This page is awaiting refinement
>Todo
>修复混乱的逻辑
>
>Upscaler 2 的预期用途 https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/3749
>


This section will introduce the parameters and related SDWebUi resources. If you can draw, the results will be more stable and considerable.

The tag reference should use [Danbooru wiki Tag](https://danbooru.donmai.us/wiki_pages/tag_groups) and [MidJourney-Styles-and-Keywords-Reference](https:// github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)

For brushstrokes/painting tools/styling/painting techniques tag group see [Danbooru](https://danbooru.donmai.us/wiki_pages/tag_group%3Aimage_composition)

If you are used to reading Japanese tag references and techniques, you can search in [Japanese Wiki](https://seesaawiki.jp/nai_ch/d/%be%ec%bd%ea%a1%a6%c7%d8%b7%ca).

This section covers: imitating style, specific shots, adding effects, fine-tuning and fine-tuning, PS grafting out of the picture, specific poses through 3D, redrawing, iteration


>Note: WebUi's settings page requires pressing `Apply setting` to save the settings.


## Basic flow

![WorkFlow](https://user-images.githubusercontent.com/75739606/197821744-99b18fbc-1ae4-4e37-b7e7-9a47adaa6ae5.svg)
<!--
![WorkFlow](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/draw_workflow.svg)
-->
This diagram demonstrates the flow of circular iteration
There are two types of iterations, circular iterations and linear iterations. Linear iterations are suitable for diversity testing, while **loop iterations** are a better choice for optimization.

`Changing hints back and forth + seeds` is not a good choice.

The basic direction of the current research is

- prompt + PS/Inpaint (fine-tuning/grafting)

- prompt + 3D reference

