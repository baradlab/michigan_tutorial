# Mosaic

!!! info "mosaic mouse"
    If you have a 3-button mouse, now is the time to use it! Mosaic is a very mouse-driven tool.

[Mosaic](https://github.com/KosinskiLab/mosaic) is a unified, GUI-driven tool from the [Kosinski lab](https://www.embl.org/groups/kosinski/) (EMBL Hamburg) for **analyzing and modeling biomembranes from 3D structural data**. It pulls the whole "segmentation → model" path into one interface: import a tomogram, clean and segment membranes, build surface meshes, localize proteins, measure geometry, and export simulation-ready systems. It's the spiritual successor to the lab's earlier [ColabSeg](https://github.com/KosinskiLab/colabseg). It is a really great exploratory tool, especially for interactive membrane analysis and modeling. We're going to use a small subset of its capabilities today, but its well worth playing around on your own with it!

In today's workflow, Mosaic is where we take the raw membrane voxels from [MemBrain-seg](membrain.md), clean them up, and split them into semantic components. We build the surface meshes themselves in [Surface Morphometrics](morphometrics.md) — Mosaic *can* mesh too, but today we're using it just for the segmentation cleanup and separation step. We use morphometrics for later steps because it provides more robust quantifications and scales more simply to large numbers of tomograms, but we are actively going back and forth with Mosaic in my lab to play with segmentation and particle picking utilities.

* **Repo:** <https://github.com/KosinskiLab/mosaic>
* **Docs:** <https://kosinskilab.github.io/mosaic/>

## Setup

```bash
cd /work/participant/segmentation_dataset/predictions
conda activate mosaic        
mosaic                       
```

## Workflow
There are currently, by my count, 6 distinct membranes in this data: The outer membrane (OMM), inner membrane (IMM), endoplasmic reticulum (ER), vesicles, plasma membrane (PM), and the nuclear membrane (NM). We often split the nuclear membrane into INM and ONM for convenience, as they have distinct properties and functions even though they are part of the same bilayer. Microtubules have also been segmented as membrane - they are often picked up by membrane networks.

For the sake of this tutorial, I am going to remove the microtubules and vesicles (since they are small, high curvature, and not very complete in this tomogram). If you want to keep vesicles in the dataset, you should feel free to keep them - I just know they are a bit less interesting in this dataset because they are small and incomplete. 

If you want to analyze all three volumes, you may want to remove additional membranes beyond these - I'd recommend keeping the IMM, OMM, INM, and ONM, since these have some interesting metrics. At the very least, keep the mitochondria! This is especially true because one of the other tomograms has a bit of vacuole, which isn't even in this segmentation. 

1. Import the thresholded membrane segmentation from MemBrain-seg. Remember to check "Segmentation Volume" during the opening dialogue!
2. Perform connected component clustering to break up components. **This will make a lot of components, and the software will be a little slow to respond. We'll start removing components quickly and it will get snappy again!**
3. Using the "select" dialogue, select all components smaller than log 3.5 and remove them. You can adjust this threshold as needed, but its a quick way to get rid of junk. You can also do this ahead of time in membrain in the future once you know the number you like.
4. Using the "Pick Objects" tool (Shift-E), select and remove the microtubules any any other unwanted components.
5. Step through the remaining clusters and merge the ER and the separated pieces of IMM, and label the ER, INM, ONM, and PM. 
6. You'll see that the OMM is still connected to a large piece of IMM! To solve this, we'll use Leiden Clustering. Select the OMM cluster (for me, it was cluster 7),  then run "Clustering -> Leiden" with the default (-7.3) threshold.
7. Wait for both jobs to complete (this may take a couple minutes; one goes fast), then finish merging and relabeling the IMM and OMM.
8. Reorder the components to ensure the correct semantic labels are applied. Today, I will be using the order IMM, OMM, ER, PM, INM, ONM. If you did more or fewer components, adjust the order accordingly - this ordering is needed for the next step!
7. Select all labels, then right click and export all as a single volume in the segmentations folder named "YTC041_1_lam4_2_ts_002_labels.mrc". Filename is somewhat important here - surface morphometrics expects the beginning of both the tomogram and the segmentation to match. There are workarounds we can talk about if you don't think you can follow this convention; let me know.

## Outputs

* Cleaned, per-class membrane segmentations — ready to be meshed in [Surface Morphometrics](morphometrics.md).

## Tips

* Do the semantic separation here once, carefully — it pays off through the entire morphometrics pipeline.
* Keep class names consistent with what you'll put in the morphometrics `config.yml` (e.g. `OMM`, `IMM`).
* For a given dataset, always order your components in a consistent manner. This ensures that the semantic labels are applied correctly and makes the data easier to work with. I made [qvox](https://github.com/teamtomo/qvox) a while back to deal with issues of inconsistent ordering and other segmentation issues, and can provide scripts for reordering if needed.
* Mosaic and Surface Morphometrics overlap on meshing and geometry analysis — today we use Mosaic only for segmentation cleanup + semantic separation, and Surface Morphometrics for the meshing and the curvature/distance quantification. It's worth knowing Mosaic has its own opinionated approach to these tasks, that may be useful for some of your applications!
