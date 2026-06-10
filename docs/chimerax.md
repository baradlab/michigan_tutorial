# ChimeraX (and ArtiaX) Visualization

Using [ChimeraX](https://www.cgl.ucsf.edu/chimerax/) with the [ArtiaX Plugin](https://github.com/FrangakisLab/ArtiaX) and our quantified surfaces, we will make a (hopefully attractive) visualization of our data.


## Setup
1. Start ChimeraX with `module load chimerax && chimerax`
2. Install ArtiaX: Tools -> More tools to get to the toolshed, then search and install ArtiaX. Restart ChimeraX to activate ArtiaX! 
3. Download the star file: [clean_ribosomes.star](static/clean_ribosomes.star). ArtiaX has a (new as of a couple weeks ago) open bug with starfiles with strings in it, so I am just providing a clean starfile for you. I made it with the starfile python repository 

## Making the right obj/mtl combo:
Here, we'll export an obj file with a specific color map using morphometrics:

```bash
morphometrics export_obj --feature IMM_dist config.yml morphometrics/YTC041_1_lam4_2_ts_002_labels_OMM.AVV_rh9.vtp
```

This will make an obj file with an associated mtl (material) file, which records color for the obj in a way that lots of different softwares read.

