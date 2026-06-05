# EasyMode

[EasyMode](https://www.biorxiv.org/content/10.64898/2026.05.19.726344v1) is a library of **general pretrained segmentation networks for cellular cryo-ET**, trained on thousands of tilt series spanning a large and diverse variety of sources. The key idea: it renders tomogram content computationally accessible **without any per-dataset training**. Instead of painting training data and training a network for every new sample (as we did with Dragonfly in 2024), you run a general model and get a multi-class segmentation of cellular architecture as a first pass — great for finding many different elements at once and feeding flexible subtomogram-averaging workflows.

!!! warning "Confirm before the workshop — TODO"
    Confirm the install/launch and the available model classes on this year's machines, and update the repository link below + in the [Overview](index.md).

## When to use it

* You want a broad, multi-class segmentation of cellular content (membranes, ribosomes, filaments, large complexes, …) with no manual training.
* You're scoping a new dataset and want to see "what's in here" quickly.
* You want segmentations to drive subtomogram-averaging / particle localization.

## Setup

```bash
# TODO: confirm env + entrypoint
conda activate easymode      # placeholder
```

## Running EasyMode

!!! note "TODO: fill in concrete commands from the workshop machines"

```bash
# TODO: model selection + inference command
# easymode segment --tomogram <tomo>.mrc --model <general_model> --output <out>
3dmod <tomogram>.mrc <out_segmentation>.mrc
```

## Outputs

* A multi-class voxel segmentation of the tomogram.
* Per-class masks you can carry into modeling/morphometrics or use for particle extraction.

## Tips

* EasyMode is a fast first pass — combine it with the membrane-specialist ([MemBrain](membrain.md)) and filament-specialist ([TARDIS](tardis.md)) tools where you need the highest-quality segmentation of a specific feature.
* Pretrained generality means no training data to paint, but always eyeball the result against the raw tomogram before trusting it downstream.
