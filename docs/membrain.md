# MemBrain-seg

[MemBrain-seg](https://github.com/teamtomo/membrain-seg) is a deep-learning tool for segmenting membranes in cryo-ET data. It is based on a model trained on a wide variety of cellular tomograms, and the pretrained model segments membranes with high accuracy out of the box — which makes it pretty much bulletproof as a first step for the segmentation process. As of **MemBrain v2** it is an end-to-end suite: `MemBrain-seg` (segmentation), `MemBrain-pick` (picking particles along membranes), and `MemBrain-stats` (statistics/morphometrics on those picks).

If you want to do morphometrics, you can move quickly from a MemBrain segmentation to a mesh — though I recommend separating the membrane into individual semantic components first (we'll do that with [Mosaic](mosaic.md)).

!!! note "Verify before the workshop — TODO"
    Confirm this year's env name, the checkpoint path on the workshop machines, and the current `membrain` subcommand names/flags (they evolve between versions).

## Setup

```bash
conda activate membrain-seg   # TODO: confirm env name
```

## Running MemBrain-seg

```bash
membrain segment \
  --tomogram-path TE3_tomo.mrc \
  --ckpt-path /sw/membrain-seg/models/<current_model>.ckpt \
  --store-probabilities
3dmod <path_to_predictions>
```

Usually, this is all you have to do. However, sometimes the default threshold is a bit too generous and membranes merge into each other. If that happens, look at the score map and pick a threshold that separates the membranes cleanly:

```bash
3dmod <path_to_scoremap>
membrain thresholds --scoremap-path <path_to_scoremap> --thresholds X
```

Determine `X` by looking at the pixel levels in the score map — you want membranes to be well separated.

## Separating components

For morphometrics you want each membrane as its own semantic class. I recommend doing this interactively in [Mosaic](mosaic.md), but you can also split connected components automatically:

```bash
membrain components \
  --segmentation-path <path-to-your-segmentation> \
  --connected-component-thres 50
```

And that's it! You should now have a set of membrane segmentations ready to be turned into meshes for morphometrics. For today's exercises we'll hand this off to Mosaic for cleanup, and use some precalculated segmentations for the morphometrics section — but if you're feeling brave, feel free to carry your own segmentation all the way through.

## Going further

* **MemBrain-pick** — pick particles along the segmented membranes.
* **MemBrain-stats** — quantitative statistics of particle distributions and membrane morphometrics.

See the [MemBrain documentation](https://github.com/teamtomo/membrain-seg) for current usage of these modules.
