#Tracking Meshes

The plugin provides some functionality for tracking meshes.

## Example cases

### Single track appears.

When one new track appears it is either, a division event or an 
interrupted track. 

If it is a division event and one new track appeared then there is a
track that continues, but should be split. To split the track I do it
one of two ways. 

1. The mesh track manager. I hold down shift and click on 
the mesh where the track should be split. That will select all of the 
meshes after the selected frame. Then I click the button 'to new track'.

2. The alternative way to split a track is to go to the frame before the 
mesh appears and run. 

    controls.splitMeshTrack();

### Track disappears

This can happen because a cell was not detected, or two cells were fused.
If it is a missed detection, I select the disappearing track, go to the
frame after it has disappeared and create a new track with the initializer.

When two cells are fused. I might delete the double mesh and create two
new meshes. Or I use the furrow and 


## Coloring meshes.

The colors can be made transparent. So it is possible to turn a mesh off.

    t = controls.getSelectedMeshTrack();
    t.setColor( new Color( 255, 255, 255, 0 ) );
 
To turn off all of the meshes.

    tracks = controls.getAllMeshTracks();
    tracks.forEach( function( t ) {
        t.setColor( new Color( 255, 200, 200, 128 ) );
    });

## Continuing a mesh track.

### Mesh initialization tab

When a new mesh is created it will be linked to the currently selected
track, unless that track has a mesh in the current frame.

### Initialize by tracking

Pressing 't' from the 3D display will cause a mesh to be copied to a 
new frame. The new mesh will be linked with the track it was selected
from.

This is primariy for manual segmenting cells with small displacements and
shape changes. The intent is to be able to initialize a mesh from the
previous frame.

## Mesh Track Manager

The track manager is a way to find errors, and modify tracks. 

### Splitting a track using 'to new track'

Hold down shift, and click on the mesh that you want to move to a new track.
That will select all of the track from the click on mesh to the end of the
track. Then click the button "to new track" and the mesh will be split.

## Javascript tools

### Splitting a track

    controls.splitMeshTrack();
    
That will split the currently selected mesh track by moving all of the
meshes after the current frame to a new mesh track.

Color tracks based on some criteria. 
