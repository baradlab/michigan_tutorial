# Surforama

[Surforama](https://github.com/cellcanvas/surforama) is a [napari](https://napari.org/) plugin for **interactively exploring volumetric data by leveraging 3D surfaces**. Given a membrane surface mesh, it projects the local tomogram density onto the surface so you can see what's sitting on (or in) the membrane, annotate and pick particles directly on the surface, and export oriented particles as RELION-formatted STAR files for subtomogram averaging.

It pairs naturally with the surfaces we build in [Mosaic](mosaic.md) / [Surface Morphometrics](morphometrics.md): once you have a clean membrane mesh, Surforama turns it into a tool for finding and orienting membrane-associated proteins. Developed by Kevin Yamauchi, Kyle Harrington, and collaborators ([teamtomo](https://github.com/teamtomo) / [cellcanvas](https://github.com/cellcanvas)).

* **Repo:** <https://github.com/cellcanvas/surforama>
* **Paper:** [Surforama: interactive exploration of volumetric data by leveraging 3D surfaces (bioRxiv 2024)](https://www.biorxiv.org/content/10.1101/2024.05.30.596601v2)

!!! note "Confirm before the workshop — TODO"
    Confirm this year's env/install and which surface + tomogram we load, then fill in the concrete click-path below.

## When to use it

* You have a membrane surface mesh and want to **see densities on it** in context.
* You want to **pick particles on a membrane surface** with sensible orientations (normal to the membrane).
* You want oriented particles exported to **RELION STAR** files to feed subtomogram averaging.

## Setup

```bash
# TODO: confirm env + install on the workshop machines
conda activate surforama     # placeholder
pip install surforama        # installs the napari plugin
napari
```

## Workflow

!!! note "TODO: fill in concrete steps from the workshop machines"

1. Launch `napari` and open the Surforama plugin (Plugins menu).
2. Load the tomogram and a membrane surface mesh (e.g. the `.vtp`/mesh from Mosaic or Surface Morphometrics).
3. Project the tomogram density onto the surface and adjust the sampling depth/contrast to see membrane-associated features.
4. Pick particles on the surface; orientations are taken from the surface normals.
5. Export the oriented particles as a RELION STAR file for subtomogram averaging.

## Outputs

* Surface-projected views of the tomogram density for inspection/figures.
* Picked particles with positions **and orientations** on the membrane.
* RELION-formatted STAR files ready for subtomogram averaging.

## Tips

* The quality of your picks depends on the quality of the surface — clean meshing in [Mosaic](mosaic.md) pays off here too.
* Surface normals give you orientation priors "for free," which is a big head start for averaging membrane proteins.
