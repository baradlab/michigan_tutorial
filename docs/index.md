# Segmentation and Modeling for CryoET - putting the Cell in Cellular Structural Biology

Segmentation and geometrical modeling for Cryo-ET are powerful processing steps that can be used to 
generate better visualizations that capture the 3-dimensional nature of tomograms
quantify ultrastructure of cellular features, 
and provide supporting geometry for particle localization and subtomogram averaging.

Because every cellular tomography project involves a different set of features, studying membranes, filaments, large protein complexes, or phase separation, the tools and techniques used for segmentation and modeling can vary widely.
Today, we will cover several different software tools that are used for studying membranes and filaments in cryo-ET. If you are interested in studying these or other aspects of cellular ultrastructure, please reach out to me and I will be happy to help you find the right tools for your specific project, or to collaborate to develop new tools if needed - I am always looking for new projects!

The three major tools we will cover today are:

* [Membrain-Seg](https://github.com/teamtomo/membrain-seg) - a deep learning-based tool for segmenting membranes in cryo-ET data. This tool is nearly bulletproof for segmenting membranes, and does not require retraining. It is developed by [Lorenz Lamm](https://scholar.google.com/citations?user=HscyH3QAAAAJ) as part a collaboration between Ben Engel and Tingying Ping's research groups, and is part of the [teamtomo community software development effort](https://github.com/teamtomo) led loosely by Alister Burt.
* [Dragonfly](https://www.objectresearch.com/dragonfly) - a commercial software package that is widely used in the cryo-ET community for segmentation and modeling. [Matt Swulius](https://pure.psu.edu/en/persons/matthew-t-swulius) has developed a number of workflows for Dragonfly that are used for segmentation and modeling of cellular features. Dragonfly is especially nice for its user-friendly interface for generating training data and training and evaluating models for segmentation. This makes it great for studying your filament of choice, since it is flexible and does not rely on pretrained models.
* [Surface Morphometrics](https://github.com/grotjahnlab/surface_morphometrics) - an open-source toolkit for building high quality triangle mesh models in a fully automated way from segmentations in Cryo-ET, and using those segmentations to quantify local and global membrane ultrastructure. This software was developed by me during my postdoc in [Danielle Grotjahn's](https://grotjahnlab.org) lab at Scripps Research, and my lab is continuing to develop new methodology within the framework. 

We'll also use the following tools for visualization and analysis:

* [IMOD](https://bio3d.colorado.edu/imod/) - a suite of tools for 3D reconstruction and modeling of tomographic data. We will use IMOD for visualization of tomograms and segmentations
* [Meshlab](https://www.meshlab.net/) - a powerful open-source tool for processing and editing 3D meshes. We will use Meshlab for manual generation of triangle meshes.
* [Paraview](https://www.paraview.org/) - a powerful open-source tool for visualization and analysis of large datasets. We will use Paraview for visualization of quantifications on meshes.