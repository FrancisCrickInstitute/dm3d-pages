# Binary Mesh Format

The binary mesh format is a very simple format the plugin uses
for loading and saving meshes. The library, 
[binarymeshformat](https://pypi.org/project/binarymeshformat/) is 
available at pypi, and can be installed through pip.

    pip install binarymeshformat

It's recommended you use a virtual environment, or other environment
management.

## Opening mesh files with python.

```python

import binarymeshformat as bmf

tracks = bmf.loadTracks("sample.bmf");

```

## Rendering meshes in VEDO

If you install [VEDO]( https://github.com/marcomusy/vedo ) Then this
will show a window with 1 frame worth of meshes. When the window is close
the next frame will appear.

```python

#!/usr/bin/env python

import binarymeshformat as bmf
import vedo.mesh
import vedo

import sys

def getVedoMesh( bmfmesh ):
	points = [];
	triangles = []
	p = bmfmesh.positions
	t = bmfmesh.triangles
	
	for i in range( len(bmfmesh.positions)//3):
		point = [ p[3*i], p[3*i+1], p[3*i+2] ]
		points.append(point)
	for i in range( len(bmfmesh.triangles) // 3 ):
		triangle = [ t[i*3] , t[i*3+1], t[i*3 + 2] ]
		triangles.append( triangle );
	
	return vedo.mesh.Mesh( [ points, triangles ] )
		
def showMeshes( bmf_meshes ):
	x = []
	index = 0
	for bmesh in bmf_meshes:
		vmesh = getVedoMesh( bmesh )
		vmesh.color(index).lineColor('black')
		x.append(vmesh)
		index += 1
	vedo.show(x).close();

	
if __name__ == "__main__":
	tracks = bmf.loadMeshTracks(sys.argv[1])
	print("loaded %s tracks"%len(tracks) )
	
	frames = set()
	
	for track in tracks:
		for k in track.meshes:
			frames.add(k)
	
	for frame in frames:
		to_see = [ track.meshes[frame] for track in tracks if frame in track.meshes.keys() ]
		showMeshes(to_see)


```
