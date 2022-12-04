# Extension

## Extensions

**WebUi now comes with its own extension manager interface.**

Extensions are a more convenient form of user scripts.

Extensions all exist in their own subdirectory inside the `extensions` directory. You can use git to install an extension like this:

```
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients extensions/aesthetic-gradients
```

This installs an extension from `https://github.com/AUTOMATIC1111/stable-diffusion-webui-aesthetic-gradients` into the `extensions/aesthetic-gradients` directory.

Alternatively you can just copy-paste a directory into `extensions`.

For developing extensions, see [Developing extensions](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Developing-extensions).

**See a list of all extensions at [here](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts#stylepile)**

### txt2mask 

Automatically create masks for Stable Diffusion inpainting using natural language. 
https://github.com/ThereforeGames/txt2mask

### DreamBooth extension

Dreambooth Extension for Stable-Diffusion-WebUI

https://github.com/d8ahazard/sd_dreambooth_extension

### tokenizer

An extension for stable-diffusion-webui that adds a tab that lets you preview how CLIP model would tokenize your text. 

https://github.com/AUTOMATIC1111/stable-diffusion-webui-tokenizer

### training-picker

Extension for stable-diffusion-webui that allows the user to mux through the keyframes of a video, and automatically pick and export training examples from individual keyframes. 

https://github.com/Maurdekye/training-picker

## Custom Scripts

The content of this topic may not be instantly updated here, [source address](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts#prompt-interpolation)

### multi-subject-render

Generate multiple complex subjects all at once.

https://github.com/Extraltodeus/multi-subject-render

### sd-dynamic-prompts

A custom script for AUTOMATIC1111/stable-diffusion-webui to implement a tiny template language for random prompt generation 

https://github.com/adieyal/sd-dynamic-prompts

### Depth Maps for Stable Diffusion

This script is an addon for AUTOMATIC1111's Stable Diffusion Web UI that creates depthmaps from the generated images. 

https://github.com/thygate/stable-diffusion-webui-depthmap-script

### sd-lexikrea

Script for Automatic1111 Stable Diffusion to pull prompts from krea and lexica. 

https://github.com/Vetchems/sd-lexikrea

### Auto-completion

Here's a project that enables SD to do a NAI-like patching effect.

https://github.com/DominikDoom/a1111-sd-webui-tagcomplete

### Advanced prompt matrix

https://github.com/GRMrGecko/stable-diffusion-webui-automatic/blob/advanced_matrix/scripts/advanced_prompt_matrix.py

```
<cyber|cyborg|> cat <photo|image|artistic photo|oil painting> in a <car|boat|cyber city>
```

### XYZ Drawing Script

Generate an .html file to interactively navigate through the image set. Use the scroll wheel or arrow keys to move through the Z dimensions.

https://github.com/xrpgame/xyz_plot_script

### Embedding to PNG

https://github.com/dfaker/embedding-to-png-script

### Random Steps and CFG

https://github.com/lilly1987/AI-WEBUI-scripts-Random
