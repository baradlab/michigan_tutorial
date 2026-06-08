# Mosaic

[Mosaic](https://github.com/KosinskiLab/mosaic) is a unified, GUI-driven tool from the [Kosinski lab](https://www.embl.org/groups/kosinski/) (EMBL Hamburg) for **analyzing and modeling biomembranes from 3D structural data**. It pulls the whole "segmentation → model" path into one interface: import a tomogram, clean and segment membranes, build surface meshes, localize proteins, measure geometry, and export simulation-ready systems. It's the spiritual successor to the lab's earlier [ColabSeg](https://github.com/KosinskiLab/colabseg).

In today's workflow, Mosaic is where we take the raw membrane voxels from [MemBrain-seg](membrain.md), clean them up, and split them into semantic components. We build the surface meshes themselves in [Surface Morphometrics](morphometrics.md) — Mosaic *can* mesh too, but today we're using it just for the segmentation cleanup and separation step.

* **Repo:** <https://github.com/KosinskiLab/mosaic>
* **Docs:** <https://kosinskilab.github.io/mosaic/>

!!! note "Confirm before the workshop — TODO"
    The description and links above are correct, but please confirm the workshop-machine specifics and the exact step names before publishing:

    * **Install / launch** — env name and launch command on this year's machines.
    * **Exact menu/operation names** for the steps under "Workflow" below (Mosaic evolves quickly).

## When to use it

* You have a membrane segmentation (e.g. from [MemBrain-seg](membrain.md)) that needs cleanup or splitting into semantic classes. **(This is what we use it for today.)**
* You also want meshing, protein localization, geometry analysis, or export to physical simulation in one place down the line — Mosaic does all of these, even though today we hand meshing off to Surface Morphometrics.

## Setup

```bash
# TODO: confirm env + launch command on the workshop machines
conda activate mosaic        # placeholder
mosaic                       # placeholder entrypoint
```

See the [installation docs](https://kosinskilab.github.io/mosaic/) for the canonical instructions.

## Workflow

!!! note "TODO: fill in concrete steps from the workshop machines"

1. Import the tomogram and the MemBrain-seg membrane segmentation.
2. Clean up the segmentation — remove stray points/voxels, fill small gaps, trim at borders.
3. Separate the segmentation into semantic components (e.g. OMM, IMM, ER, vesicle).
4. Export the cleaned, per-class segmentation for [Surface Morphometrics](morphometrics.md) to mesh.
5. (Optional, beyond today) Mosaic can also build meshes, localize proteins, measure geometry, and export a simulation-ready system.

## Outputs

* Cleaned, per-class membrane segmentations — ready to be meshed in [Surface Morphometrics](morphometrics.md).

## Tips

* Do the semantic separation here once, carefully — it pays off through the entire morphometrics pipeline.
* Keep class names consistent with what you'll put in the morphometrics `config.yml` (e.g. `OMM`, `IMM`).
* Mosaic and Surface Morphometrics overlap on meshing and geometry analysis — today we use Mosaic only for segmentation cleanup + semantic separation, and Surface Morphometrics for the meshing and the curvature/distance quantification. It's worth knowing Mosaic can do much of this on its own.
