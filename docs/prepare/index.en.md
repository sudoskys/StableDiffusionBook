# PREPARATION

The purpose of this document is to summarize the results of the Chinese community's discussion and to create a self-help guide for people who are just starting with Stable Diffusion painting, beginning with WebUi and learning about Ai painting.

## How to ask a polite question in the community?

[How to ask questions the smart way?](http://www.catb.org/~esr/faqs/smart-questions.html)

Suppose you identify any errors or important content that needs to be added - In that case, you can send **Issue** or submit **Pr** to this repository to help complete the document and help more people.

Feel free to share any questions you may have. Before asking any questions, please ensure no relevant answers are in the documentation. If you need to ask a community member, please include all logs. However, please remember that no one - including community members - is obligated to answer any questions for you without having any stake in the process.

If you see a section you do not recognize, it is possible that your version of WebUi needs to be higher or support this feature.

## Can I join the party?

Before we start - unfortunately, because of the need to use `CUDA` acceleration, only **Nvidia graphics cards** support it well. (AMD can be used but is significantly slower than Nvidia graphics, ~ of course, without a graphics card, you can also use the CPU to spend hundreds of times the time to generate ~)

**Linux + AMD graphics card** Please read [AMD Installation Guide](https://rentry.org/ayymd-stable-diffustion-v1_4-guide) and [AMD Installation WebUi Guide](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Install-and-Run-on-AMD-GPUs)

[For discussions related to supporting AMD Gpu solutions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/1046)

!!! danger "About graphics card warranty"
    The warranty policy of graphics card manufacturers for deep learning cards is equivalent to mining cards.
    The card will risk breaking if you play too much (e.g. three days in a row of graphics).

[Stable diffusion performance test reports for various graphics cards](https://docs.google.com/spreadsheets/d/1Zlv4UFiciSgmJZncCujuXKHwc4BcxbjbSBg71-SdeNk/edit#gid=0)

## The choice of models

Generating images requires a model and the WebUi. You need to download a model first to use it. The tutorial test uses a third-party model from China, not a live model.

Sharpen the knife before cutting the wood. Common models are often uploaded to these two model stations:

- [civitai](https://civitai.com/)

- [huggingface](https://huggingface.co/)

**Anime style**

It is worth mentioning that as of March 2023, the rise of Lora makes the traditional DreamBooth and other Merge model scenery no longer. Lora and Hypernet with hot-plugging capability make the painting style much more flexible. Nevertheless, a good base model is still an important thing. The following are some of the best models worthy of reference.

- [Anything V3.0](https://huggingface.co/andite/anything-v4.0/tree/main). This is a merged model, probably a hodgepodge, but the results are pretty good.

- [OrangeMixs](https://huggingface.co/WarriorMama777/OrangeMixs)。This is also a merged model, which merged through [Merge block weighted](https://github.com/bbc-mc/sdweb-merge-block-weighted-gui) with excellent results. The authors provide several versions, among which the more famous Abyssal Orange ([AOM2](https://huggingface.co/WarriorMama777/OrangeMixs/tree/main/Models/AbyssOrangeMix2)/[AOM3](https://huggingface.co/WarriorMama777/OrangeMixs/tree/main/Models/AbyssOrangeMix3)) is the product of a merge of realistic and anime styles.

**Realistic style**

- [Stable Diffusion 2.0](https://stability.ai/blog/stable-diffusion-v2-release)

- [Stable Diffusion 1.5](https://huggingface.co/runwayml/stable-diffusion-v1-5)

- [Stable Diffusion 1.4](https://huggingface.co/CompVis/stable-diffusion-v1-4)

- [chilloutmix](https://civitai.com/models/6424/chilloutmix)

### Dataset Sources

The Stable Diffusion Model series is based on [LAION](https://laion.ai/).

NAI series models are based on [Danbooru](danbooru.donmai.us/)  and Stable Diffusion Model.

Anything series model is based on Merge.

The OrangeMixs series model is based on [Merge block weighted](https://github.com/bbc-mc/sdweb-merge-block-weighted-gui).
