**Grab your 3-button mice! Segmenting without one is the wooooooooorst!**

Your setup should be quite quick, as everything should already be arranged. However, we need to activate dragonfly. To do so, run the following commands (the last one is just to launch dragonfly and make sure it comes up successfully)

```bash
conda activate dragonfly
bash ~.bin/dragonfly-rsync.bash
dragonfly
```

Make sure you are able to open the Deep Learning Toolbox within dragonfly. You don't need it now but sometimes it complains about "extra instances". Raise your hand and get my attention and I will try to help if you run into this!

## Finding the data
I have provided some fun data for everyone to play with. Instead of working with beautiful perfect data, we will be playing with some "typical" data for a relatively new user. I collected it during the first year of my postdoc and I am quite fond of it so please don't hate too hard. The reality is that better data quality will in turn make segmentations better, but you should not be afraid to try to analyze imperfect data!

The data can be found at:
```bash
cd /scratch/segmentation_dataset
```

Take a look around in here. Feel free to open stuff with 3dmod and get a sense of what is in the tomogram (maybe easier to see in the deconv version). We are going to:

1. Use membrain-seg to segment the membranes in the tomogram.
2. Use dragonfly to do some quick contrast enhancement and train it to segment the 3 classes of filaments.
3. Use surface_morphometrics to generate some meshes and do some basic quantifications on the data.

