Dragonfly is a free-for-nonprofit software package that has a nice set of tools for generating training data, training a variety of different neural networks types, and applying them flexibly to different kinds of data. Jess Heebner in Matt Swulius's lab developed some excellent protocols for using matlab to segment tomography data, and showed that in high-contrast examples it is possible to differentiate actin from cofilactin, for example. They also established a simplified protocol for extracting filaments in amira from segmentations, massively reducing the human time required for that kind of analysis. Their jove article detailing this protocol can be found here: https://www.jove.com/t/64435/deep-learning-based-segmentation-of-cryo-electron-tomograms. What we are going to do today is a slight update from their original protocols, but ultimately you can't go wrong following their jove article. The only exception would be that I would advise not using the segmentation wizard, and instead using the deep learning toolbox. That is what we will do today!

## Launch Dragonfly
Hopefully this is already done if you licensed everything and it went well!

```bash
module load dragonfly
Dragonfly
```

We will pause here for everyone to get set up with Dragonfly. If you have any issues, please let one of us know! Sometimes Dragonfly stalls when you launch it - if this happens, open a new tab and try launching dragonfly again. That should unstall the first one and you can kill the second job once it does (with Ctrl-C). Yes, I also hear how crazy that sounds as a strategy.

## Setting up Dragonfly

Hopefully you all got Dragonfly licensed and opened earlier today. If you haven't, please do so now. If you have any issues, please let me know. 

Within dragonfly, we want to make one settings change to make life easier:
1. Go to File -> Preferences
3. Change "Default Unit" to "nanometers" - most people do segmentations in nm scale, because that is what the volume EM community uses. The features we care about are usually 5+ nm in scale so this is a good default. All the software we are using will work fine in Angstrom scale, however.

The software is a bit liable to crashing - to account for this, I'd recommend setting up autosave in File -> Preferences -> Autosave. I'd recommend saving every 5 minutes, and keeping 3 backups. This will save you a lot of headache if the software crashes.

I anticipate this will take a bit of time to get everyone set up for everyone (licensing in particular) so if you get through quickly, feel free to grab a coffee!

## Loading the Denoising Model

The first thing we are going to do is load a denoising model. This is a neural network that has been trained to remove noise from tomography data. This is not at all a critical step in the process, but it can make the hand segmentation process a little easier. We made this denoising model using Matt Swulius's [CryoTomoSim](https://doi.org/10.1101/2023.04.28.538636) software to generate noise free and noisy simulated data with lots of membranes, filaments, and other bits and bobs. It is not quite as accurate as cryoCARE or isonet, but it is fast and easy.

To load the denoising model, go to the "Deep Learning" tab, and click "Load Model". Navigate to `/scratch/segmentation_dataset` and select the whole `tutorial` folder. This will load the denoising model, but you won't actually run it through the denoising tool (you can, but we won't today).

## Loading the Data and the Membrain Segmentation

Next, we are going to load the data. File -> Open -> navigate to `/scratch/segmentation_dataset` and select the `TE3_tomo.mrc` file. This is a tomogram showing a mitochondrion with some filaments in it. You should have gotten membrane segmentations of this data from the previous tutorial. If you haven't, you can find a usable segmentation in `/scratch/segmentation_dataset/morpho_run/datadir`. Load that file with the same File -> Open process.

## Calibrating the Data

Calibration puts all the data on a similar intensity scale. 

1. Check on the mode and standard deviation of the intensities in the data. On the data panel on the right side of the screen, with the tomogram selected, click on the histogram button under tools.
2. Note down the mode and standard deviation of the data. Calculate the mode - standard deviation.
3. Right click on the dataset and select Calibrate Intensity Scale. Set the foreground label to the mode and the background label to the mode - standard deviation. This will put the data on a consistent intensity scale.

## Preprocessing the Image

1. Workflows -> Image Filtering
2. Select "Histogram Equalization". Deselect the output.
3. Click add and then select "Gaussian". Deselect the output.
4. Click add and then select "Unsharp". Leave the output.
5. Click "Apply" to apply the filter.

This is where you can try out the denoiser in addition to the other "standard" processing steps. Instead of or in addition to doing the above steps, you can filter with AI:
1. Main -> Filter with AI
2. Select the denoising model you loaded earlier.
3. Click "Filter" to apply the filter.

## Preparing to train the Neural Network

1. Convert the membrane segmentation to a multi-ROI by right-clicking on the segmentation and selecting "Extract ROIs". 
2. Delete the background ROI by clicking and trashing.
3. Select the other ROI and right-click to "Create Multi-ROI from ROI". This will create a multi-ROI with the membrane pre-segmented.
4. Add the following classes to the multi-ROI: "Actin", "Septin", "Microtubule", "Ribosome", "Background"
5. Right click on the objects panel and select "Create a box from" and select current box. Choose to make the box 400 by 400 by 32 pixels.
6. Move the box to a place where you can see some of each class of object. The 4-panel view can help with this.
7. Switch to the segment panel and select the ROI painter -> round brush, and select the OTSU brush. This will allow you to paint in the different classes of objects quickly and efficiently (if your calibration is good!)
8. Start painting! Ctrl-left click to paint in the different classes of objects, and shift-left click to erase.
9. Once you are done, 
9. Once you are done, right click on the box and select "Add to ROI", "New ROI", name it "Mask".

## Training a new neural network

1. AI -> Deep Learning Tool
2. New Model
3. "U-Net", "Semantic Segmentation", 6 classes (or 5 if you skipped ribosomes), 2.5D, 3 or 5 layers. More layers helps a lot but takes longer to train. 
4. Set up deep learning with that tool.
5. Select the input data as input, the multi-ROI as output, and the mask as mask.
6. Adjust the augmentation to have 2x augmentation. More is better, but we want to go fast not good today.
7. Select visual feedback and select the input data.
8. Adjust the training parameters:
    * Lower the epoch number. 25 is a good number that will go quick but not be low-quality.
    * Increase batch size until the GPU memory is ~60% full.
9. Click "Train"

**Coffee break!!!**

## Evaluating the neural net and retraining
Once the neural network is trained, you can apply it to the data and see how well it does. If it doesn't do well, you can continue to retrain it using the OTSU brush. At OHSU, we generally do 1 round of training with a pretrained network, then spend a bit of time repainting, then do a second round of training. This generally allows us to get a good to great segmentation with under an hour of human time.
1. Segment Panel -> Segment with AI
2. Select the neural network you just trained.
3. Click "Segment" (or "Preview" if you only want to see one slice)
Optional: If the segmentation is bad, you can retrain the network with the new data.
4. Use the otsu brush to correct any mistakes. You can also use the "Fill" tool to move large areas to background if needed.
5. Repeat the training process once the correction is done. You might want to use a new box/mask to cover a relatively small number of larger slices - maybe 1k by 1k by 32 so you just clean up part of the data then train.


## Extracting Trained Data
Once you have an inferred segmentation, you can extract the data as tifs then convert them to mrc. This is a bit of a pain, and ORS has a beta version of direct MRC export, so expect this to be much better in the future!
1. Right click on the segmentation and select "Convert to Greyscale"
2. Right click on the greyscale and select "Export" and select "TIF". Choose somewhere to save the data.
3. Convert the stack of TIFs to MRC using IMOD.
```bash
module load imod
cd <wherever you saved the tif files>
tif2mrc *.tif TE3_dragonfly_labels.mrc
3dmod TE3_dragonfly_labels.mrc
```
