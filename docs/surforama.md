# Surforama

[Surforama](https://github.com/cellcanvas/surforama) is a [napari](https://napari.org/) plugin for **interactively exploring volumetric data by leveraging 3D surfaces**. Given a membrane surface mesh, it projects the local tomogram density onto the surface so you can see what's sitting on (or in) the membrane, annotate and pick particles directly on the surface, and export oriented particles as RELION-formatted STAR files for subtomogram averaging.

It pairs naturally with the surfaces we build in [Surface Morphometrics](morphometrics.md): once you have a clean membrane mesh, Surforama turns it into a tool for finding and orienting membrane-associated proteins. Developed by Kevin Yamauchi, Kyle Harrington, and collaborators ([teamtomo](https://github.com/teamtomo) / [cellcanvas](https://github.com/cellcanvas)).

* **Repo:** <https://github.com/cellcanvas/surforama>
* **Paper:** [Surforama: interactive exploration of volumetric data by leveraging 3D surfaces (bioRxiv 2024)](https://www.biorxiv.org/content/10.1101/2024.05.30.596601v2)


## When to use it

* You have a membrane surface mesh and want to **see densities on it** in context.
* You want to **pick particles on a membrane surface** with sensible orientations (normal to the membrane).
* You want oriented particles exported to **RELION STAR** files to feed subtomogram averaging.

## Setup
Unfortunately, I introduced a scaling bug in my code this week, so you won't be able to make an obj scaled correctly for Surforama. Whoops! Instead, go ahead and grab [surforama.obj](static/surforama.obj)

For surforama, its also helpful to have some deconvolution (or denoising!) done on your data:
```bash
module load imod
mtffilter tomograms/YTC041_1_lam4_2_ts_002.mrc -dec 0.5 -def 5 YTC041_1_lam4_2_ts_002_dec.mrc
```

```bash
conda activate surforama    
surforama --image-path YTC041_1_lam4_2_ts_002_dec.mrc --mesh-path surforama.obj
```

Try changing the parameters of the offset and the thickness to project - you should be able to find some dark spots at an offset around 5-8 that correspond to membrane-associated ribosomes! 

If you want, you can try setting up a star file and picking those particles - these get their initial orientations directly from the surface normals, so you get a lot of the geometric advantages Mart and Will talked about for contextual particle picking.

## Outputs

* Surface-projected views of the tomogram density for inspection/figures.
* Picked particles with positions **and orientations** on the membrane.
* RELION-formatted STAR files ready for subtomogram averaging.

## Tips

* The quality of your picks depends on the quality of the surface — clean meshing in [Mosaic](mosaic.md) pays off here too.
* Surface normals give you orientation priors "for free," which is a big head start for averaging membrane proteins.
