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

The segmentation landscape has moved fast. In 2024 we leaned on MemBrain-Seg for membranes and Dragonfly for everything else. In 2026, a family of general pretrained networks means you can get high-quality multi-class segmentations with little or no manual training, and the modeling/morphometrics side has been streamlined with new tooling. We've retired the Dragonfly section in favor of these newer, mostly-open approaches.

## Segmentation tools we will cover today

* [**MemBrain**](https://github.com/teamtomo/membrain-seg) — a deep learning-based tool for segmenting membranes in cryo-ET data. It is nearly bulletproof for segmenting membranes with a pretrained model and does not require retraining. MemBrain v2 adds picking (MemBrain-pick) and statistics (MemBrain-stats) on top of segmentation. It is developed by [Lorenz Lamm](https://scholar.google.com/citations?user=HscyH3QAAAAJ) as part of a collaboration between the Engel and Tingying Peng research groups, and is part of the [teamtomo community software development effort](https://github.com/teamtomo).
* [**Mosaic**](#) — an interactive toolkit out of the Engel lab for editing membrane segmentations and turning them into clean, physically regularized mesh/surface models that are ready for downstream analysis and simulation. We'll use it to clean up and organize membrane segmentations into semantic components before modeling.
* [**EasyMode**](https://www.biorxiv.org/content/10.64898/2026.05.19.726344v1) — a library of general pretrained segmentation networks for cellular cryo-ET, trained on thousands of tilt series across a wide variety of sources. EasyMode renders tomogram content computationally accessible without any per-dataset training, which makes it a powerful first pass for finding the many different elements of cellular architecture and feeding flexible subtomogram-averaging workflows.
* [**TARDIS**](https://github.com/SMLC-NYSBC/napari-tardis_em) — *Transformer-bAsed Rapid Dimensionless Instance Segmentation*. TARDIS provides pretrained models that **instance**-segment filaments and membranes across modalities (cryo-ET, cryo-EM, plastic-section ET) in a single framework without retraining, and is especially strong on microtubules and other filaments. Developed by [Robert Kiewisz](https://github.com/RRobert92) and colleagues at NYSBC.

## Surface Morphometrics

* [**Surface Morphometrics**](https://github.com/grotjahnlab/surface_morphometrics) — an open-source toolkit for building high-quality triangle-mesh models in a fully automated way from segmentations in cryo-ET, and using those models to quantify local and global membrane ultrastructure. This software was developed by me during my postdoc in [Danielle Grotjahn's](https://grotjahnlab.org) lab at Scripps Research, and my lab continues to develop new methodology within the framework — including newer helper tools like [`qvox`](https://github.com/teamtomo/qvox) for voxel-array operations and [`ETSegTools`](https://github.com/bbarad/ETSegTools) for manipulating multilabel segmentations.

## Visualization and analysis utilities

We'll also use the following tools for visualization and analysis:

* [IMOD](https://bio3d.colorado.edu/imod/) - a suite of tools for 3D reconstruction and modeling of tomographic data. We will use IMOD (`3dmod`) for visualization of tomograms and segmentations.
* [napari](https://napari.org/) - a fast, interactive n-dimensional image viewer for Python. Several of today's tools (Mosaic, TARDIS) ship as napari plugins.
* [Meshlab](https://www.meshlab.net/) - a powerful open-source tool for processing and editing 3D meshes. We will use Meshlab for manual generation of triangle meshes.
* [Paraview](https://www.paraview.org/) - a powerful open-source tool for visualization and analysis of large datasets. We will use Paraview for visualization of quantifications on meshes.
