# Segmentation and Modeling for CryoET - putting the Cell in Cellular Structural Biology

!!! info "2026 edition"
    This is the **2026 edition** of the tutorial. Looking for the original June 2024 workshop (MemBrain-Seg, Dragonfly, Surface Morphometrics)? Use the version selector in the header, or go straight to the [2024 archive](https://baradlab.com/michigan_tutorial/2024/).

Segmentation and geometrical modeling for Cryo-ET are powerful processing steps that can be used to
generate better visualizations that capture the 3-dimensional nature of tomograms,
quantify ultrastructure of cellular features,
and provide supporting geometry for particle localization and subtomogram averaging.

Because every cellular tomography project involves a different set of features — studying membranes, filaments, large protein complexes, or phase separation — the tools and techniques used for segmentation and modeling can vary widely.
Today, we will cover several different software tools that are used for studying membranes and filaments in cryo-ET. If you are interested in studying these or other aspects of cellular ultrastructure, please reach out to me and I will be happy to help you find the right tools for your specific project, or to collaborate to develop new tools if needed - I am always looking for new projects!

## What's new since 2024

The segmentation and modeling landscape has moved fast. In 2024 we leaned on MemBrain-Seg for membranes and Dragonfly for everything else. In 2026 we focus on a tighter, mostly-open membrane workflow: segment with MemBrain-seg, clean up and mesh in Mosaic, quantify ultrastructure with Surface Morphometrics, and explore/pick membrane-associated proteins on the resulting surfaces with Surforama. We've retired the Dragonfly section.

## The workflow we will cover today

We'll follow a single membrane through the whole pipeline, in order:

* [**MemBrain-seg**](https://github.com/teamtomo/membrain-seg) — a deep learning-based tool for segmenting membranes in cryo-ET data. It is nearly bulletproof for segmenting membranes with a pretrained model and does not require retraining. MemBrain-seg is part of the broader MemBrain v2 suite (which also adds picking and statistics). It is developed by [Lorenz Lamm](https://scholar.google.com/citations?user=HscyH3QAAAAJ) and collaborators, and is part of the [teamtomo community software development effort](https://github.com/teamtomo).
* [**Mosaic**](https://github.com/KosinskiLab/mosaic) — a unified GUI tool from the [Kosinski lab](https://www.embl.org/groups/kosinski/) (EMBL Hamburg) for analyzing and modeling biomembranes from 3D structural data. It takes you from a tomogram and segmentation through cleanup, semantic separation, and surface meshing — and can go further to protein localization, geometry analysis, and simulation-ready export. We'll use it to clean up and organize the MemBrain-seg output into components and build surface meshes. ([docs](https://kosinskilab.github.io/mosaic/))
* [**Surface Morphometrics**](https://github.com/grotjahnlab/surface_morphometrics) — an open-source toolkit for building high-quality triangle-mesh models from membrane segmentations and using them to quantify local and global membrane ultrastructure (curvature, inter-/intra-membrane distances, orientation). This software was developed by me during my postdoc in [Danielle Grotjahn's](https://grotjahnlab.org) lab at Scripps Research, and my lab continues to develop new methodology within the framework — including newer helper tools like [`qvox`](https://github.com/teamtomo/qvox) for voxel-array operations and [`ETSegTools`](https://github.com/bbarad/ETSegTools) for manipulating multilabel segmentations.
* [**Surforama**](https://github.com/cellcanvas/surforama) — a [napari](https://napari.org/) plugin for interactively exploring volumetric data on 3D surfaces. It projects tomogram density onto a membrane mesh so you can see and pick membrane-associated proteins, and exports oriented particles as RELION STAR files for subtomogram averaging. Developed by Kevin Yamauchi, Kyle Harrington, and collaborators ([teamtomo](https://github.com/teamtomo) / [cellcanvas](https://github.com/cellcanvas)).

## Visualization and analysis utilities

We'll also use the following tools for visualization and analysis:

* [IMOD](https://bio3d.colorado.edu/imod/) - a suite of tools for 3D reconstruction and modeling of tomographic data. We will use IMOD (`3dmod`) for visualization of tomograms and segmentations.
* [napari](https://napari.org/) - a fast, interactive n-dimensional image viewer for Python. Surforama ships as a napari plugin.
* [Meshlab](https://www.meshlab.net/) - a powerful open-source tool for processing and editing 3D meshes. We will use Meshlab for manual generation of triangle meshes.
* [Paraview](https://www.paraview.org/) - a powerful open-source tool for visualization and analysis of large datasets. We will use Paraview for visualization of quantifications on meshes.
