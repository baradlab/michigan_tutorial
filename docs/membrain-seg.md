# Membrain-Seg

Membrain-Seg is a relatively new tool for segmenting membranes in cryo-ET data. It is based on a deep learning model that has been trained on a variety of cellular tomograms. The model is able to segment membranes with high accuracy with a pretrained model, which makes it pretty much bulletproof as a first step for starting the segmentation process. If you want to do morphometrics, you can pretty quickly move from the Membrain-Seg output to a mesh using the `segmentation_to_meshes.py` script in the `surface_morphometrics` package (though I recommend you extract the individual components into their own classes first using dragonfly or amira).

## Setup
```bash
conda activate membrain-seg
```

## Running Membrain-Seg
```bash
membrain segment --tomogram-path TE3_tomo.mrc --ckpt-path /sw/membrain-seg/models/MemBrain_seg_v10_alpha_MONAI1.3.0.ckpt --store-probabilities
3dmod <path_to_predictions>
```

Usually, this is all you have to do. However, sometimes the default threshold is a bit too generous, and membranes merge into each other. This is bad. If this happens, you can try to adjust the threshold by running the following command:

```bash
3dmod <path to scoremap>
membrain thresholds --scoremap-path <path_to_scoremap>--thresholds X
```
Determine X by looking at the pixel levels in the score map! You want to set the threshold so that the membranes are well separated.

## Separating components
I recommend using Amira or Dragonfly to separate components into semantic classes manually, but you can also use the following command to do it automatically:

```bash
membrain components --segmentation-path <path-to-your-segmentation> --connected-component-thres 50
```

And thats it! You should now have a set of membrane segmentations that are ready to be turned into meshes for morphometrics! For today's exercises, we will use these segmentations to help with dragonfly, but we will use some precalculated segmentations for morphometrics. If you are feeling brave, feel free to freelance during the morphometrics section and use this seg! It might be quite a bit better than the one we will be using.