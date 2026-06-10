# Setup


Most of the heavy setup has already been arranged on the workshop machines (thanks Chris!). This page points you at the data and sets up the folder structures we'll be using for our work.

We'll be working 

## Getting set up

Open a new terminal and get ready for lots of different conda environments - each tool we are using will have its own environment.

```bash
source conda_init.sh
conda activate membrain-seg # Membrain is going to be used first in our workflow
module load imod # It is extremely valuable to have 3dmod ready to look at intermediate results. Always look at your data!
```

## Setting up your data structure

We have pre-downloaded 3 tomograms and their corresponding segmentations for you to work with from EMPIAR-12534 (a dataset of cyclohexamide-treated yeast cells with stalled co-translating ribosomes). I am going to walk through a single tomogram for the segmentation steps, but you can do the other two as well if you pick up the clustering quickly. Every step except mesh refinement is fast enough to comfortably accomodate all three tomograms.

The data and segmentations are available in the `/data/EMPIAR-12534` directory.

```bash
mkdir -p /work/participant/segmentation_dataset
cd /work/participant/segmentation_dataset
mkdir tomograms
mkdir segmentations
mkdir morphometrics
ln -s /work/data/EMPIAR-12534/data/Reconstructed_tomograms/YTC041_1/YTC041_1_lam4_2_ts_002.mrc_9.98Apx.mrc /work/participant/segmentation_dataset/tomograms/YTC041_1_lam4_2_ts_002.mrc
```

The tomogram is now available in the `tomograms` directory. If you want, you can link the other two tomograms at the same time - every processing step will take 3 times as long, but it may be interesting to see stats for multiple tomograms.

I recommend you open the tomogram with `3dmod` and get a sense of what is inside (the deconvolved/denoised version is often easier to read).

```bash
3dmod /work/participant/segmentation_dataset/tomograms/YTC041_1_lam4_2_ts_002.mrc
```

There is a lot of interesting structure inside the tomogram, including membranes and other cellular components. There are a couple of microtubules, a bit of nucleus, a mitochondrion, the plasma membrane, and some ribosomes. We'll ignore the microtubules today, but it may be fun at the end (if you want to make a cool figure) to run this tomogram through easymode and see if you can grab some of the structures of interest. 

## What we're going to do today

We'll follow a single tomogram through the whole pipeline:

1. Use **[MemBrain-seg](membrain.md)** to segment the membranes in the tomogram with a pretrained model.
2. Use **[Mosaic](mosaic.md)** to clean up the membrane segmentation and split it into semantic components.
3. Use **[Surface Morphometrics](morphometrics.md)** to build surface meshes from the cleaned segmentation and quantify membrane ultrastructure (curvature, distances, orientation, thickness).
4. Use **[Surforama](surforama.md)** to explore tomogram densities on the membrane surfaces and pick oriented membrane-associated proteins.
5. (Bonus) If there's time, use ArtiaX and Chimera to visualize some ribosome-mitochondria interactions.

## General tips
If something breaks, go ahead and raise your hand and ask for help. Especially for steps that involve automated parsing, sometimes typos a file name or other minor things can be very hard to diagnose on your own!

Don't be afraid to experiment with settings - most of these tools have more than one way to achieve the same result, and different parameters may work better for your data.


# When you're done, you can move to [MemBrain-seg](membrain.md)!