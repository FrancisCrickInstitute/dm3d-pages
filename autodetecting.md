# Auto detecting.

This plugin has some limited autodetection and tracking routines. When autodetecting, a threshold is used and meshes are initialized based on the threshold. 
This docuemnt shows a couple workflows for detecting specific types of images.

## Detecting bright nuclei 

This simply done using a threshold.

Find a slice in the furrow pane where the objects we want to detect and seperate. Adjust the contrast value until the nuclei are separated.

Then using the javascript console run:

    controls.guessMeshes( t_value );

That will detect meshes in all of the image using the provided threshold value.

What will often happen is that across an image different thresholds are 
required. To address this issue we've used neural networks. 
[Active Unet Segmentation](https://github.com/FrancisCrickInstitute/ActiveUnetSegmentation) 
to train a neural network to detect a distance transform of the image. 

Using the neural network to make a prediction, the prediction can be used to automatically segment the image. 

- open the prediction, and select the third channel.
- Check the good level for threshold. 
- run guess meshes. 
- set the image energy to perpendicular intensity with a force of around -0.05. 
- deform the mesh/refine.

## Tracking

The automatic tracking routine provided is very naive, but it can be useful 
for finding errors in the detection, or finding cells that are ambiguous in one
frame but discernible in another.

To start a completely new tracking

    controls.autoTrackFromScratch()

To only track cells that are not part of any track.
    
    controls.trackAvailableTracks()
