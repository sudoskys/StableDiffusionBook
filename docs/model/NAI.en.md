# NAI Leak model

Incident report about NAI Leak.

The models folder structure for using the NAI Leak model should be as follows.


```
./models
├── Codeformer
├── ESRGAN
├── GFPGAN
├── hypernetworks
│   ├── aini.pt
│   ├── anime_2.pt
│   ├── anime_3.pt
│   ├── anime.pt
│   ├── furry_2.pt
│   ├── furry_3.pt
│   ├── furry_kemono.pt
│   ├── furry_protogen.pt
│   ├── furry.pt
│   ├── furry_scalie.pt
│   ├── furry_transformation.pt
│   └── pony.pt
├── LDSR
├── Stable-diffusion
│   ├── final-pruned.ckpt -> novelai s model.ckpt (pruned)
│   ├── final-pruned.vae.pt -> novelai s animevae.pt
│   ├── final-pruned.yaml -> model.ckpt is the same folder as config.yaml
│   └── 其他模型文件.ckpt
└── SwinIR
```

`hypernetworks` and `Stable-diffusion` are the new folders that need to be created. Other files are renamed according to the rules.

??? info "Content"
    - stableckpt/ - Stable Diffusion checkpoints
    - animefull-latest - The model NovelAI uses in production
    - workspace/ - Code used to train/run/finetune models
    - sd-private.tar.zst - Stuff to train Stable Diffusion
    - github/ - Code taken from GitHub. CREDENTIALS SCRUBBED
    - novelai/ - From NovelAI org
    - *.tar.zst Archived git repos, public AND PRIVATE
    - aboutus.gpg - Our public GPG key
    - sha256sum - SHA256 sums of every file
    - sha256sum.sig Detached signature for the sums, signed by our GPG key

## Part 1

|file|corresponding Leak path|description|
|----|---------|--------|
|`final-pruned.ckpt`|`stableckpt/animefull-final-pruned/model.ckpt` (pruned)|model master file|
|`final-pruned.vae.pt`|`stableckpt/animevae.pt`|for stabilization styles|
|`final-pruned.yaml`|`stableckpt/animefull-final-pruned/config.yaml`|with documenting extra parameters, memory consumption, and ineffective|
|`stableckpt/vector_adjust/v2.pt`|styled|feels inferior to `hypernet`|
|personal don't need to download|`workspace`|full set of front and back ends, 40GB only to start|

Note that the name of `final-pruned .yaml` should correspond to `final-pruned .ckpt`

`hypernetworks` contains files in `stableckpt/modules/modules`, which are style model that can be called as character-specific `embedding models`, and used with models to generate specific styles.

embedding s file format is `*.pt`. This embedding model needs to be enabled in the settings tab of WebUi(**Only** for hypernetworks folder. BUT for Textual Inversion,Webui automatically loads the models in the embeddings folder, which is one of the differences between them)

Most people cannot run `workspace`.


## Part 2

`prodmodels` are GPT models (for language processing), but the program actually uses CLIP.

`random_stableckpt` is a number of models, some of which are copies of the Part 1 folder files


![Part1](https://user-images.githubusercontent.com/75739606/197821809-7eed7776-9508-4c71-9b07-5f02e13290b2.jpg)
<!--
![Part1](https://raw.githubusercontent.com/sudoskys/StableDiffusionBook/main/resource/models.jpg)
-->

About the style model, start the cli with a prompt to load it OK, at the same time you can go to the settings to choose the model you like `hypernetwork`



## Full and baseline models

`animesfw-latest` = NAI baseline model

`animefull-final-pruned` = `full-latest` = NAI full model (with adult content)




## Use latest (7G) or pruned (4G) model

The 4GB model is pruned from the 7GB model, removing the last weight and leaving the EMA weights.

Individual users only need to use the pruned 4GB model. Using latest would overuse RAM and VRAM.

And since NAI also uses EMA weights online, it does not make sense to choose the latest 7GB model.

[Conclusion from the discussion in this post](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3882551)




**About EMA**

EMA moving average does not help in generating images.

But it does prevent overfitting in subsequent training.

Not sure if it works in dreambooth training.



## Style [^5]


![furry](https://user-images.githubusercontent.com/474879/194965036-4c9f70ca-a32f-4f68-a9a1-17a827e8e61e.jpg)
>furry

![test-result](https://user-images.githubusercontent.com/115398132/194805364-95e523aa-4bec-4a88-9fe1-b3d39d2f2992.jpg)

```
masterpiece, best quality, masterpiece, 1girl, solo, outdoors, flowers, dancing
Negative prompt: nsfw, lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts,signature, watermark, username, blurry, artist name
Steps: 28, Sampler: Euler, CFG scale: 12, Seed: [SEE COLUMN], Size: 512x512, Model hash: 925997e9, Hypernet: [SEE ROW]
```

>"aini" has a very strong style you might not care for that you have to really prompt to get out of (I like it shrug ) and going into this I would have said it has the highest consistency & quality, but from these seeds and settings, I was surprised by "pony"
I think "anime_3" is the highest quality of that set but they're all somewhat inconsistent. I generally wouldn't recommend them.

It can be seen that `furry`'s hypernetwork is more aggressive in adding animal features, so the more conservative changes here may be related to the sampler, steps and CFG. [^5]

[^5]:[所有超网络的 X/Y](https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2017#discussioncomment-3836360)
