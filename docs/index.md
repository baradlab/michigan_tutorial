# Segmentation and Modeling for CryoET - putting the Cell in Cellular Structural Biology

!!! info "2026 edition"
    This is the **2026 edition** of the tutorial. Looking for the original June 2024 workshop (MemBrain-Seg, Dragonfly, Surface Morphometrics)? Use the version selector in the header, or go straight to the [2024 archive](https://baradlab.com/michigan_tutorial/2024/).

Segmentation and geometrical modeling for Cryo-ET are powerful processing steps that can be used to
generate better visualizations that capture the 3-dimensional nature of tomograms,
quantify ultrastructure of cellular features,
and provide supporting geometry for particle localization and subtomogram averaging.
You learned a lot about segmentation during your easymode session on Monday, so instead of focusing on general segmentation techniques, I thought today we'd really dive deep into how to make ultra high quality membrane segmentations and use them for quantitative cell biology.


Today, we will cover several different software tools that are used for studying membranes in cryo-ET. Similar tools have been developed in recent years for filament organization (including in progress tools from the Barad lab) and protein colocalization. I'll focus on the membrane-specific tools for this tutorial, but I'll include a list of other great quantitative tools for your reference at the end of this document.

## What's new since 2024

The segmentation and modeling landscape has moved fast. In 2024 we leaned on MemBrain-Seg for membranes and Dragonfly for everything else. Membrain-seg remains in my opinion the most consistent tool for generating high-quality membrane segmentations with minimal postprocessing, and is still the go-to tool for this task. However, recent efforts from several groups have improved the simplicity and accuracy of general segmentation, making it less common to need to do entirely novel segmentation training for any but the most specialized samples. Because of this, and because you got a small experience of [Ais earlier in the week](), In 2026 we focus on a tighter, mostly-open membrane workflow: segment with MemBrain-seg, clean up and mesh in Mosaic, quantify ultrastructure with Surface Morphometrics, and explore/pick membrane-associated proteins on the resulting surfaces with Surforama. We've retired the Dragonfly section.

## The workflow we will cover today

We'll follow a single membrane through the whole pipeline, in order:

* [**MemBrain-seg**](https://github.com/teamtomo/membrain-seg) — a deep learning-based tool for segmenting membranes in cryo-ET data. It is nearly bulletproof for segmenting membranes with a pretrained model and does not require retraining. MemBrain-seg is part of the broader MemBrain v2 suite (which also adds picking and statistics). It is developed by [Lorenz Lamm](https://scholar.google.com/citations?user=HscyH3QAAAAJ) and collaborators, and is part of the [teamtomo community software development effort](https://github.com/teamtomo).
* [**Mosaic**](https://github.com/KosinskiLab/mosaic) — a unified GUI tool from the [Kosinski lab](https://www.embl.org/groups/kosinski/) (EMBL Hamburg) for analyzing and modeling biomembranes from 3D structural data. It can take you from a tomogram and segmentation all the way through meshing, protein localization, geometry analysis, and simulation-ready export. In today's workflow we'll use it specifically to **clean up the MemBrain-seg output and split it into semantic components** — we'll build the meshes themselves in Surface Morphometrics. ([docs](https://kosinskilab.github.io/mosaic/))
* [**Surface Morphometrics**](https://github.com/grotjahnlab/surface_morphometrics) — an open-source toolkit for building high-quality triangle-mesh models from membrane segmentations and using them to quantify local and global membrane ultrastructure (curvature, inter-/intra-membrane distances, orientation). This software was developed by me during my postdoc in [Danielle Grotjahn's](https://grotjahnlab.org) lab at Scripps Research, and my lab continues to develop new methodology within the framework. Today we'll be working with the 2.0 beta, which includes a new API, mesh refinement, membrane thickness measurement, protein-membrane localization, and other new features.
* [**Surforama**](https://github.com/cellcanvas/surforama) — a [napari](https://napari.org/) plugin for interactively exploring volumetric data on 3D surfaces. It projects tomogram density onto a membrane mesh so you can see and pick membrane-associated proteins, and exports oriented particles as RELION STAR files for subtomogram averaging. Developed by Kevin Yamauchi, Kyle Harrington, and collaborators ([teamtomo](https://github.com/teamtomo) / [cellcanvas](https://github.com/cellcanvas)). Today we'll be using it with our own midzone surfaces to identify membrane-associated ribosomes.

## Visualization and analysis utilities

We'll also use the following tools for visualization and analysis:

* [IMOD](https://bio3d.colorado.edu/imod/) - a suite of tools for 3D reconstruction and modeling of tomographic data. We will use IMOD (`3dmod`) for visualization of tomograms and segmentations.
* [napari](https://napari.org/) - a fast, interactive n-dimensional image viewer for Python. Surforama ships as a napari plugin.
* [Paraview](https://www.paraview.org/) - a powerful open-source tool for visualization and analysis of large datasets. We will use Paraview for visualization of quantifications on meshes.

# Other useful tools for membranes
* [Meshlab](https://www.meshlab.net/) - a powerful open-source tool for processing and editing 3D meshes. We use Meshlab (and pymeshlab) for generation of triangle meshes, and use the graphical tool for piloting new workflows for meshing.
* [pyvista](https://docs.pyvista.org/) - a Python library for 3D visualization and mesh analysis. Can be used for visualizing and analyzing 3D meshes and volumetric data. Sometimes nicer than paraview.


# Other quantitative cell biology tool for cryo-ET
* [tomospatstat](https://sites.google.com/site/3demimageprocessing/tomospatstat) - a tool for spatial protein localization statistics in cryo-ET data.
* [Membrain-Stats](https://github.com/CellArchLab/membrain-stats) - a tool for statistical analysis of protein localization and distribution on membrane geodesics.
* [Filament Toolbox](https://research-explorer.ista.ac.at/record/14502) - a tool for analyzing and visualizing filamentous structures in cryo-ET data.
* [CryoCAT](https://cryocat.readthedocs.io/latest/index.html) - Software from Beata Turonova with lots of cool utilities including protein clustering analysis and membrane thickness measurement.