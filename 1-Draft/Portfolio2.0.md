# Inspo
## BOIDS
https://github.com/mrdoob/three.js/blob/master/examples/webgl_gpgpu_birds.html
## Frutiger Aero underwater 3D
https://www.bluemarinefoundation.com/the-sea-we-breathe/journeys/
https://youtu.be/0D-J_Lbxeeg?t=3596 (perlin noise bg and sun rays)

## 26/01/27
fih geometry
![[Pasted image 20260208193143.png]]
red is Z
blue is Y
tail
(0, tailHeight,-tailOffset)
(0, -tailHeight,-tailOffset)
(0,0,0)

bod
(0,0,0)
(0, bodyHeight, , bodyLegth*(1-headRatio))
(0,, -bodyHeight,  bodyLegth*(1-headRatio))

hed
(0, , bodyHeight, bodyLegth*(1-headRatio))
(0, -bodyHeight, , bodyLegth*(1-headRatio))
(0, 0,  bodyLegth)