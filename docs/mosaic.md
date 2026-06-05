# Mosaic

Mosaic is an interactive toolkit (out of the Engel lab) for **editing membrane segmentations and turning them into clean, organized surface models**. After MemBrain gives us raw membrane voxels, Mosaic is where we split them into semantic components (OMM, IMM, ER, vesicles, …), clean up artifacts, and produce tidy point clouds / meshes that downstream morphometrics can consume.

!!! warning "Confirm before the workshop — TODO"
    I scaffolded this page from the workshop outline. Please confirm the canonical details before publishing:

    * **Install / launch** — package name, conda env, and whether we run it as a napari plugin or standalone.
    * **Repository / citation link** — update the link in the [Overview](index.md) and below.
    * **Exact menu/operation names** for the steps in "Workflow" below.

## When to use it

* You have a membrane segmentation (e.g. from [MemBrain](membrain.md)) that needs cleanup or splitting into semantic classes.
* You want to go from voxel segmentation to a clean surface/mesh model interactively, rather than via the fully automated pipeline.

## Setup

```bash
# TODO: confirm env + launch command
conda activate mosaic        # placeholder
napari                       # if Mosaic is a napari plugin
```

## Workflow

!!! note "TODO: fill in concrete steps from the workshop machines"

1. Load the tomogram and the MemBrain membrane segmentation.
2. Separate the segmentation into semantic components (e.g. OMM, IMM, ER, vesicle).
3. Clean up stray voxels / fill small holes / trim segmentation at borders.
4. Export each component for modeling — point cloud and/or mesh.

## Outputs

* Cleaned, per-class membrane segmentations.
* Surface/mesh models ready for [Surface Morphometrics](morphometrics.md).

## Tips

* Do the semantic separation here once, carefully — it pays off through the entire morphometrics pipeline.
* Keep class names consistent with what you'll put in the morphometrics `config.yml` (e.g. `OMM`, `IMM`).
