# Surface Morphometrics

![Workflow Figure](https://raw.githubusercontent.com/GrotjahnLab/surface_morphometrics/master/Workflow_title.png)
### Quantification of Membrane Surfaces Segmented from Cryo-ET or other volumetric imaging.  
Surface morphometrics is a toolbox I developed during my postdoc to better understand how mitochondrial membranes remodel during stress. It turns out to be useful for a whole lot more than mitochondria! You can use it to assess aspects of membrane geometry in detail both locally and globally within cells, thanks to a robust meshing (modeling) algorithm and a set of simple tools for making measurements on the resulting surfaces.

Today we will scratch the surface with a single tomogram, but one of the most valuable aspects of building models and making quantifications is that it becomes possible to assess statistical significance by comparing quantifications across many tomograms in different cells and conditions. This is a powerful way to move from qualitative phenomenology about cellular remodeling to robust quantitative understanding of the underlying biology. With one tomogram, we're mostly still doing phenomenology.


## Setup:
1. Activate your conda environment and move to the morpho_run folder: 
```bash
conda activate morphometrics
cd /scratch/segmentation_dataset/morpho_run
```
2. Edit the `config.yml` file for our project's needs. I prefer visual studio code for this!
    * Set the `data_folder` to the folder containing your label file (`/scratch/segmentation_dataset/morpho_run/datadir`)
    * Set the `output_folder` to the folder where you want the output files to be saved (`/scratch/segmentation_dataset/morpho_run/workdir`)
    * For `segmentation_values`, use `-127` for the OMM and `-126` for the IMM.
    * Set the `max_triangles` to 50,000 to speed up computation - this will reduce the quality of the final surfaces so don't do this when you are running at home!
    * Set the `num_cores` to 16.
    * Set the `radius_hit` to 10.
    * Set `exclude_borders` to 1.
    * For `intra`, provide `IMM` and `OMM`
    * For `inter`, provide `IMM`: `OMM`


## Example data

There is example data and a config available in the `morpho_run` folder. This is TE3_labels.mrc, which is the same tomogram you will be processing in the rest of the tutorial. You should check some details about it! You can also compare it to the tomogram itself.
```bash
module load imod
header /scratch/segmentation_dataset/morpho_run/datadir/TE3_labels.mrc
header /scratch/segmentation_dataset/TE3_tomo.mrc
3dmod /scratch/segmentation_dataset/morpho_run/datadir/TE3_labels.mrc
3dmod /scratch/segmentation_dataset/TE3_tomo.mrc
```

## Processing your data
### Interactive mesh generation
We are going to do semi-interactive mesh generation in meshlab to teach you how the sausage is made, but there is also a fully configurable pipeline (`python ../surface_morphometrics/segmentation_to_meshes.py config.yml`) that will do the whole thing for you.

1. Prepare the xyz point cloud files to make new meshes: 
```bash
cd /scratch/segmentation_dataset/morpho_run
python ../surface_morphometrics/mrc2xyz.py -l -127 datadir/TE3_labels.mrc workdir/TE3_OMM.xyz
python ../surface_morphometrics/mrc2xyz.py -l -126 datadir/TE3_labels.mrc workdir/TE3_IMM.xyz
```
2. Launch `Meshlab`
```bash
module load meshlab
meshlab
```
3. For each mesh file:
    1. Filters->Normals, Curvatures, and Orientation->Compute Normals for Point Sets
    2. Filters->Remeshing, Simplification, and Reconstruction->Screened Poisson Surface Reconstruction
    3. Filters->Selection->Select Faces by Vertex Quality
    4. Filters->Selection->Delete Selected Faces
    5. Filters->Remeshing, Simplification, and Reconstruction->Quadric Edge Collapse Decimation
    5. File->Export Mesh As...->TE3_OMM.ply

### Pipelined Processing Steps
0. If you wanted to make the meshes automatically: `python ../surface_morphometrics/segmentation_to_meshes.py config.yml`. We aren't doing this today because I think its more fun to do by hand. If you have 30 tomograms each with 3-5 labels, it will no longer be fun.
1. Convert the ply files to vtp files: `python ../surface_morphometrics/ply2vtp.py config.yml TE3_OMM.ply TE3_OMM.surface.vtp`. Do this again for IMM.
2. Run pycurv for each surface (normally, this is best run in parallel on a cluster): 
    `python ../surface_morphometrics/run_pycurv.py config.yml TE3_OMM.surface.vtp`. Do this again for IMM (it will take a long time for IMM!)
    You may see warnings aobut the curvature, this is normal and you do not need to worry.
    We may run into memory constraints - if you do, you can reduce the number of triangles in the surface by setting `max_triangles` to a lower number in the config file. This will reduce the quality of the final surfaces, but will make the computation faster and lower-memory.
3. Measure intra- and inter-surface distances and orientations (also best to run this one in parallel for each original segmentation): `python ../surface_morphometrics/measure_distances_orientations.py config.yml`
4. Don't do this today: Combine the results of the pycurv analysis into aggregate Experiments and generate statistics and plots. This requires some manual coding using the Experiment class and its associated methods in the `morphometrics_stats.py`. Everything is roughly organized around working with the CSVs in pandas dataframes. Running  `morphometrics_stats.py` as a script with the config file and a filename will output a pickle file with an assembled "experiment" object for all the tomos in the data folder. Reusing a pickle file will make your life way easier if you have dozens of tomograms to work with, but it doesn't save too much time with just the example data...

Just in case you have problems running things fast (I don't have a great sense of how long this will take on these machines), I have provided usable output files for the downstream analysis you might want to check out. You can find them in the `morpho_run/workdir` folder.


## Inspecting Results 
### Visualizing the surfaces
1. Load the surfaces in Paraview: 
    * Open Paraview
    * File -> Open -> TE3_OMM.surface.vtp
    * File -> Open -> TE3_IMM.surface.vtp
    * Make each visible
    * Color by `curvedness_vv` to see the curvature of the surface
    * Color by `OMM_dist` (or `IMM_dist`) to see the distance from the IMM to the OMM or visa-versa
2. Adjust color scales to see the features you are interested in. Add and edit the scalebar.
3. Adjust the background, turn on ambient occlusion, and make the surfaces look nice for a screenshot.
4. Take some nice pictures!

### Generating some basic statistics and plots
* `python ../surface_morphometrics/single_file_histogram.py workdir/TE3_IMM.AVV_rh8.csv -n curvedness_vv` will generate an area-weighted histogram for a feature of interest in a single tomogram. I am using a variant of this script to respond to reviews asking for more per-tomogram visualizations!
* `python ../surface_morphometrics/single_file_2d.py workdir/TE3_IMM.AVV_rh8.csv -n1 curvedness_vv -n2 OMM_dist` will generate a 2D histogram for 2 features of interest for a single surface.



## Running individual steps without pipelining
Individual steps are available as click commands in the terminal, and as functions

1. Robust Mesh Generation
    1. `mrc2xyz.py` to prepare point clouds from voxel segmentation
    2. `xyz2ply.py` to perform screened poisson reconstruction and mask the surface
    3. `ply2vtp.py` to convert ply files to vtp files ready for pycurv
2. Surface Morphology Extraction
    1. `curvature.py` to run pycurv in an organized way on pregenerated surfaces
    2. `intradistance_verticality.py` to generate distance metrics and verticality measurements within a surface.
    3. `interdistance_orientation.py` to generate distance metrics and orientation measurements between surfaces.
    4. Outputs: gt graphs for further analysis, vtp files for paraview visualization, and CSV files for         pandas-based plotting and statistics
3. Morphometric Quantification - there is no click function for this, as the questions answered depend on the biological system of interest!
    1. `morphometrics_stats.py` is a set of classes and functions to generate graphs and statistics with pandas.
    2. [Paraview](https://www.paraview.org/) for 3D surface mapping of quantifications.

## Summary of File Types:
* Files with.xyz extension are point clouds converted, in nm or angstrom scale. This is a flat text file with `X Y Z` coordinates in each line.
* Files with .ply extension are the surface meshes (in a binary format), which will be scaled in nm or angstrom scale, and work in many different softwares, including [Meshlab](https://www.meshlab.net/). 
* Files with surface.vtp extension are the same surface meshes in the [VTK](https://vtk.org/) format.
        * The .surface.vtp files are a less cross-compatible format, so you can't use them with as many types of software, but they are able to store all the fun quantifications you'll do!. [Paraview](https://www.paraview.org/) or [pyvista](https://docs.pyvista.org/) can load this format. This is the format pycurv reads to build graphs.
* Files with .gt extension are triangle graph files using the `graph-tool` python toolkit. These graphs enable rapid neighbor-wise operations such as tensor voting, but are not especially useful for manual inspection.
* Files with .csv extension are quantification outputs per-triangle. These are the files you'll use to generate statistics and plots.
* Files with .log extension are log files, mostly from the output of the pycurv run.
* Quantifications (plots and statistical tests) are output in csv, svg, and png formats. 

