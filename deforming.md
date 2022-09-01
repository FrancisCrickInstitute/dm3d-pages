#Deforming Examples
## Parameters

### Overview

| parameter | value | description |
| --------- | ----: | ----------- |
| gamma     | 1000  | Slows down deformation rate. |
| alpha     | 1  | The stiffness of the mesh, how much it wants to shrink. |
| pressure     | 0  | A normal force outwards that causes mesh to grow (or shrink if negative). |
| steric neighbors     | 0  | Prevents meshes from overlapping. |
| image weight     | 0.001  | Depends on image first parameter to adjust. |
| divisions     | 2  | number of times triangles are divided for a new mesh. |
| beta     | 0.1  | Bending stiffness. Attempts to reduce curvature. |

The default parameters create a smoothly deforming mesh when there are no
external energies. Then it is just a matter of selecting a good image
energy to deform the mesh to the image.

I find the image weight by guess and check because it changes frequently. 
When the image weight is too low, the mesh will deform freely, and pretty much shrink.
It will smooth out curvature and get smaller.

If the image weight is much to large, then the mesh will deform wildly.

## Energies

  **perpendicular intensity energy:** This will attract nodes to regions of high intensity.
  **perpendicular gradient energy:** This will attract nodes to regions of high gradient.
  **no energy:** The mesh will deform without any influence from the image.
  **curvature smoothing energy:**  

## Trouble Shooting

- The mesh deforms to the correct location in many spots, but gets "stuck". 
-- This can be caused by noise in the image. Filtering the image in imagej can help. Such as with a gaussian blur. 
-- Image 
