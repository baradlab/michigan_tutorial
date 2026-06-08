# Setup

**Grab your 3-button mice! Segmenting without one is the wooooooooorst!**

Most of the heavy setup has already been arranged on the workshop machines. This page gets you logged in, points you at the data, and makes sure each of today's tools launches before we dive in.

!!! note "Workshop machine specifics — TODO before the workshop"
    Fill in the exact login / scheduler / module details for this year's machines:

    * How to connect (ssh host, NoMachine/X2Go, web portal, …)
    * How environments are provided (`module load …`, `conda activate …`, or `micromamba run -n …`)
    * The path to the shared dataset
    * Anything that needs licensing/registration this year

## Getting connected

```bash
# TODO: connection command for this year's machines
# e.g. ssh user@workshop-host
```

## Finding the data

I have provided some fun data for everyone to play with. Instead of working with beautiful perfect data, we will be playing with some "typical" data for a relatively new user. The reality is that better data quality will in turn make segmentations better, but you should not be afraid to try to analyze imperfect data!

```bash
cd /scratch/segmentation_dataset   # TODO: confirm this year's path
```

Take a look around in here. Feel free to open the tomogram with `3dmod` and get a sense of what is inside (the deconvolved/denoised version is often easier to read).

```bash
module load imod   # TODO: confirm
3dmod /scratch/segmentation_dataset/<tomogram>.mrc
```

## What we're going to do today

We'll follow a single membrane through the whole pipeline:

1. Use **[MemBrain-seg](membrain.md)** to segment the membranes in the tomogram with a pretrained model.
2. Use **[Mosaic](mosaic.md)** to clean up and organize the membrane segmentation into semantic components and build surface meshes.
3. Use **[Surface Morphometrics](morphometrics.md)** to quantify membrane ultrastructure (curvature, distances, orientation) on those surfaces.
4. Use **[Surforama](surforama.md)** to explore tomogram densities on the membrane surfaces and pick oriented membrane-associated proteins.

## Sanity-check that each tool launches

Raise your hand and get my attention if any of these don't come up cleanly — better to sort it out now than mid-exercise!

```bash
# MemBrain-seg
conda activate membrain-seg      # TODO: confirm env name
membrain --help

# Mosaic
# TODO: confirm activation + launch command

# Surface morphometrics
conda activate morphometrics     # TODO: confirm env name

# Surforama (napari plugin)
# TODO: confirm activation
napari
```
