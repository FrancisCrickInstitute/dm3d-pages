# Javascript console examples

Using the javascript exposes many of the actions that are difficult to include in 
a graphical interface. Each one can increase the complexity of the interface significantly.
There can also be parameters that need to be passed. Or actions that can be done
repetively.

A handy feature of the javascript console is that it has auto complete. Start by typing a command, especially by
using the 'controls' object, and press tab and a selection of options will be shown.

## The API

The main entry point is the [SegmentationController](javadoc/deformablemesh/SegmentationController.html) object that handles 
most of the data. All of the public methods are accessible.


## Changing the color scheme (>=0.36)

```javascript
mf = controls.getMeshFrame3D();
mf.setBackgroundColor(Color.BLACK);
controls.setVolumeColor(Color.GREEN);
```
set it back.

```javascript
mf.setBackgroundColor(Color.WHITE);
controls.setVolumeColor(Color.BLUE);
```

## Surface Plots

There are two surface plots available. Intensity or Curvature.

```javascript

sp = controls.intensitySurfacePlot();
sp.processAndShow();

```

We can set a clipping range, and change the colors used. We also can access the MeshFrame and  set the background color.

```javascript

  cp = controls.curvatureSurfacePlot();
  cp.process();
  cp.setMax(cp.getMax()*0.8);
  cp.setHighColor(Color.YELLOW);
  cp.setLow(Color.BLUE);
  cp.show();
```

The surface plots have quite a few controls.

## Iterating through all of the frames and deforming each existing mesh 100 steps.

```javascript
n = controls.getNFrames();
tracks = controls.getAllTracks();
for(var i = 0; i<n; i++){
    controls.toFrame(i)
    ts = tracks.size();
    for( var j = 0; j<ts; j++){
        track = tracks.get(j);
        if(track.containsKey(i)){
            mesh = track.getMesh(i);
            echo(i + ", " + j + ", " + mesh);
            controls.selectMesh(mesh);
            controls.deformMesh(100);
        }
    }
}
```

Creates a movie by taking a snapshot and rotating the viewing platform each frame.

```javascript

ImageStack = Java.type("ij.ImageStack");
ColorProcessor = Java.type("ij.process.ColorProcessor");
ImageStack = Java.type("ij.ImageStack");

mf3d = controls.getMeshFrame3D();
stack = 0;
for(var i = 0; i<360; i++){
	c = Math.cos(i*3.14/180)*Math.cos(0.1);
	s = Math.sin(i*3.14/180)*Math.cos(0.1);
	z = Math.sin(0.1)
	controls.nextFrame();
	mf3d.lookTowards([c, s, z], [0, 0, 1]);
	img = mf3d.snapShot();
	proc = new ColorProcessor(img);
	if(stack==0){
		stack = new ImageStack(proc.getWidth(), proc.getHeight());
	}
	stack.addSlice(proc);
}
plus = new ImagePlus();
plus.setStack(stack);
plus.show();
```

Create a movie of the current mesh deforming. Frames determines how many frames will be in the resulting movie.
and steps is how many steps of deformation between frames. *Warning, this will effectively erase your undo history.*

```javascript

ImageStack = Java.type("ij.ImageStack");
ColorProcessor = Java.type("ij.process.ColorProcessor");
ImageStack = Java.type("ij.ImageStack");
frames = 100;
steps = 3;

mf3d = controls.getMeshFrame3D();
stack = 0;
for(var i = 0; i<frames; i++){
	controls.deformMesh(steps);
	img = mf3d.snapShot();
	proc = new ColorProcessor(img);
	if(stack==0){
		stack = new ImageStack(proc.getWidth(), proc.getHeight());
	}
	stack.addSlice(proc);
}
plus = new ImagePlus();
plus.setStack(stack);
plus.show();
```

