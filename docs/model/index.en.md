# Initialization

This section describes the Debug and some optimization solutions for the model parameters to make it work better. The model used for the demonstration in this section is `some model` to expand the tutorial. **Some source tutorials from: [^2]**

It is recommended to frequently pull code from remote code repositories `git pull` to update WebUI web applications.

!!! info "Controversy"
    To avoid involving copyright disputes, this repository does not provide links to NAI's models.
    Try asking your friends for help.

SDWebUi is a framework with many [models] like for example `some model` (https://rentry.org/sdmodels)


## Pickle?

[About pickle](https://docs.python.org/3/library/pickle.html),that Python object serialization

Docs says:
```
The pickle module is not secure. Only unpickle data you trust.

It is possible to construct malicious pickle data which will execute arbitrary code during unpickling. Never unpickle data that could have come from an untrusted source, or that could have been tampered with.
```

Both `.ckpt` and `.pt` files may have malicious pickles in them, AUTOMATIC1111 has done some verification in this regard.

For insecure files, it will prompt 

```
The file may be malicious, so the program is not going to read it. 
You can skip this check with the --disable-safe-unpickle commandline argument.
```


It is not always guaranteed to be 100% secure, and some files may even be false positive, so it is recommended to download the model from a trusted source.

In addition, you can see in [here] (https://github.com/sudoskys/StableDiffusionBook/blob/main/docs/ckpt_safe) is a simple check script.

Related Projects [pickle_inspector](https://github.com/lopho/pickle_inspector).


## Model selection

The dataset and labels used by the model are very important for the impact of the results.

It is important to understand the source of the data before using it.

### Some note

https://huggingface.co/lambdalabs/sd-pokemon-diffusers

https://huggingface.co/Onodofthenorth/SD_PixelArt_SpriteSheet_Generator

### Some model

here is a brief talk about guessing with some models.

- Guesses

For a well-known model, `masterpiece' is likely to be an artificially tagged Tag to suit commercial needs.

The guess is that the full model puts in all the data, unpurged. The basic model, on the other hand, has more accurate data and should be cleaned.

This leads to the instability of the full model.

In "Event 2" we saw many other models, such as the `wallpaper` model, which should have been trained using different datasets.

After some discussion, we believe that some models are classified according to `rating`.


But we don't actually know the details, so the above is just a guess and **not credible**.