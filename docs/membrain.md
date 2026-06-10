# MemBrain-seg

[MemBrain-seg](https://github.com/teamtomo/membrain-seg) is a deep-learning tool for segmenting membranes in cryo-ET data. It is based on a model trained on a wide variety of cellular tomograms, and the pretrained model segments membranes with high accuracy out of the box — which makes it pretty much bulletproof as a first step for the segmentation process. As of **MemBrain v2** it is an end-to-end suite: `MemBrain-seg` (segmentation), `MemBrain-pick` (picking particles along membranes), and `MemBrain-stats` (statistics/morphometrics on those picks).

If you want to do morphometrics, you can move quickly from a MemBrain segmentation to a mesh — though I recommend separating the membrane into individual semantic components first (we'll do that with [Mosaic](mosaic.md)).

!!! note "Verify before the workshop — TODO"
    Confirm this year's env name, the checkpoint path on the workshop machines, and the current `membrain` subcommand names/flags (they evolve between versions).

## Setup

```bash
conda activate membrain-seg
```

## Running MemBrain-seg

```bash
membrain segment \
  --tomogram-path tomograms/YTC041_1_lam4_2_ts_002.mrc \
  --ckpt-path /sw/membrain-seg/models/MemBrain_seg_v10_beta.ckpt \
  --store-probabilities
```

Usually, this is all you have to do. However, sometimes the default threshold is a bit too generous and membranes merge into each other. In this case, that happens a decent bit. We will use the score map to improve the thresholding for a better result. Look at the score map and pick a threshold that separates the membranes cleanly:

```bash
3dmod predictions/YTC041_1_lam4_2_ts_002_scores.mrc
```
Use the pixel view tool to examine the pixel values in the score map to determine a suitable threshold. I quite like 3.5 personally for this tomogram, but others may be quite different.

Once you've decided, apply the threshold directly to the score map using the `membrain thresholds` command:

```bash
membrain thresholds --scoremap-path predictions/YTC041_1_lam4_2_ts_002_scores.mrc --thresholds 3.5
```

This should be pretty permissive - if you want a different threshold, go for it! You may notice there are still a couple small bridging artifacts - these can be removed with a paintbrush in Amira, Dragonfly, or Ais, but today we're going to use geometric clustering in [Mosaic](mosaic.md) to clean them up.

**If you've got a good thresholded segmentation, move on to the [Mosaic](mosaic.md) step now!**


# Other membrain stuff we won't to today
## Separating components

For morphometrics you want each membrane as its own semantic class. I recommend doing this interactively in [Mosaic](mosaic.md), but you can also split connected components automatically:

```bash
membrain components \
  --segmentation-path <path-to-your-segmentation> \
  --connected-component-thres 50
```

This doesn't do a great job when there are small bridging densities like this one has.

## Going further

* **MemBrain-pick** — pick particles along the segmented membranes.
* **MemBrain-stats** — quantitative statistics of particle distributions and membrane morphometrics.

See the [MemBrain documentation](https://github.com/teamtomo/membrain-seg) for current usage of these modules.
