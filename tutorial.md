# DM3D tutorial

This guide starts with a two channel sample image and demonstrates how
to segment and track the meshes. It assumes that you have installed
the dm3d into fiji, if not check 

## Open a file
The example data is a zip file containing three images available 
here [tutorial-data.zip](https://zenodo.org/record/7544194/files/tutorial-data.zip?download=1)

Start fiji and open the provided image file: "tile_3-sample.tif". Once 
the image has loaded you can start the DM3D plugin from the plugin
menu. After which the DM3D interface will start and a dialog will come 
asking you to select a  channel. Select channel 1.

## Segmenting the membrane. 

After selecting the channel, the image is loaded. Nothing changes in
the 3D canvas, but the name of the image should be displayed.

### Initialize meshes.

The goal is to add spheres until they represent the target cell enough
to initialize a mesh that will deform to the original image data to
produce an acceptable segmentation.

To initialize a mesh click the button "initialize mesh..." this will
open a new tab with three orthoganal views of the 3D image.

The white lines in the image indicate the position of the other two
views. The slider to the right adjust the position of the view along 
the third axis. 

Using the top left initializer, adjust the slider until the image has a 
cell of interest in view preferably where the crossection is largets.

Click near the center of the cross section and a sphere will start to 
be added. Move the cursor to the edge of the cell and click again. That 
will create a sphere which will be used to initialize a mesh.

Once the sphere has been created it can be modified with the two colored
circles. Dragging the yellow circle will move the sphere and dragging the
blue circle will change the radius.

The figure shows the collection of spheres in the 3 views. For this 
image and this cell, this is an adequate guess. Click add mesh and
the spheres will change into a new mesh.

### Deform mesh.

Go back to the main tab to prepare the interface for deforming the 
initialized mesh to the image.

Select the "Max Intensity" image energy. This will attract meshes
to bright regions. The parameters should be set as follows

- gamma: 500
- alpha: 1.0
- pressure: 0
- steric neighbors: 0
- image weight: 0.01
- beta: 0.1

Click deform. The shape will quickly settle to a shape that should
capture the membrane well. Click stop! The deformation does not stop
automatically.

After deforming it is good to remesh to improve the triangle distribution.
Below "connection remesh" button there are two values. Set the min 
lenth to 0.01 and the max length to 0.02.

Then click the "connection remesh" button. That will redo the mesh so that the triangles 
are more uniform. Then click deform and when the mesh stops deforming
click stop again.

## Segmenting a nucleus

### Initialize a mesh.

Change the channel to the second channel, the dna channel.
Click on the initialize mesh button. A second initialization tab will
open. 

Again add spheres to the create a representation of the desired shape,
the dna shapes should be a bit easier to capture with spheres.

### Deform the mesh

Selected the "Max Gradient" energy, and **adjust the image weight**.

- gamma: 500
- alpha: 1.0
- pressure: 0
- steric neighbors: 0
- image weight: 0.0001
- beta: 0.1

Then click deform mesh. This should deform to the nuclei. Again use
the "remesh connections" button, and "deform" again to refine the shape
of the mesh until it is not improving any more.

## Using the distance transform.

### Automatically detecting nuclei.

First open the image provided in the tutorial dataset, 
"pred-dna-350nm-tile_3-sample.tif".and select channel 3. That is 
the distance transform. Go to the file menu and select "restart meshes"

To detect nuclei, go to the "tools" menu and start the javascript console.
Type in the command.

    controls.guessMeshes( 3 );

Then click the button "eval".

The value "3" is a threshold value. A small number will predict larger
meshes, but sometimes they can be merged. A large number will predict
smaller meshes. On the control panel, clicking on the histogram 
can help to show how well the threshold value works.

### Deforming meshes to the distance transform.

To deform to the distance tranform select a "Max Intensity" and 
set the image weight to a negative number. 

- gamma: 500
- alpha: 1.0
- pressure: 0
- steric neighbors: 0
- image weight: -0.1
- beta: 0.1

Then deform all meshes by holding down Control and clicking deform. After
the meshes have deformed and stopped changing significantly, click on 
"stop!".

That will refine the shapes, to further improve the meshes. Hold down
control and click "connection remesh" and click deform again. 

### Processing all frames

To process all 6 frames past the following javascript script.

    for( i = 0; i<6; i++){
        controls.toFrame(i);
        controls.guessMeshes(3);
        controls.deformAllMeshes(100);
        controls.reMeshConnectionsAllMeshes(0.01, 0.02);
        controls.deformAllMeshes(100);
    }
    
Click eval. That will go through all 10 frames, guess the meshes, and 
deform them.

**This will process the first frame but it shouldn't create any new meshes.**


## Tracking meshes

### Autotrack available meshes.

In the "tools" menu select "Manage Tracks". That will start a new tab
to manage the tracks. Tracks can be linked automatically. In the 
javascript console run.

    controls.toFrame(0);
    controls.autotrackAvailableTracks();

This can be run from any frame and it tries to find meshes in the next frame.

To do all of the frames.

    for(i = 0; i<5; i++){
        controls.toFrame(i);
        controls.autoTrackAvailableTracks();
    }

This successfully tracks all of the meshes in this example.

## Creating membrane meshes from DNA meshes.

Once the nuclear meshes are tracked. Open the image. 
"pred-membrane-350nm-tile_3-sample.tif" and select channel 3 again.

This involves the same method for the dna but we're starting from 
the dna meshes and deforming them to the distance transform.

- gamma: 500
- alpha: 1.0
- pressure: 0
- steric neighbors: 0
- image weight: -0.1
- beta: 0.1

Then hold control and click "deform". After meshes have slowed down, 
click "stop!" and "connection remesh" and start deforming again.

Since this is iterative, I script it. Then go through and find problems.

    for( i = 0; i<6; i++){
        controls.toFrame(i);
        controls.deformAllMeshes(100);
        controls.reMeshConnectionsAllMeshes(0.01, 0.02);
        controls.deformAllMeshes(100);
        controls.reMeshConnectionsAllMeshes(0.01, 0.02);
        controls.deformAllMeshes(100);
        controls.reMeshConnectionsAllMeshes(0.01, 0.02);
        controls.deformAllMeshes(100);
    }

### Notes

These parameters were found by guessing and checking. Once the mesh
is initialized I click deform. Then depending on how the mesh deforms
I adjust some parameters and try to get it to deform better.

If the mesh deforms very poorly I use undo.

## Supplement

### Adding DM3D to fiji.

- Start Fiji and got to the help menu.
- From the help menu select update. Fiji will check for updates
- Click the button that says "manage update sites"
- The name doesn't matter, double click on the name to change it to "DM3D"
- Double click on the URL field and change it to: https://sites.imagej.net/Odinsbane/
- Update Fiji

### Poorly deforming meshes.

When meshes deform poorly.

- adjust parameters.
- re-initialize the mesh to be a better guess.
- filter the original image.
- Manually edit mesh.

### Troubleshooting

- Q: I cannot add spheres.
- A: Is the UI blocked on some task, like a deforming mesh or changing
a parameter?
