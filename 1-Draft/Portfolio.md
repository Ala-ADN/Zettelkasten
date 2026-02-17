## Inspo

![[Pasted image 20251214194331.png]]1bit retro, cellular automata ...

![[Pasted image 20251214194533.png]]
https://forums.tigsource.com/index.php?action=profile;u=3073;sa=showPosts
https://dev.to/bytebodger/dithering-images-with-reactjavascript-och


notebook obradinn interactive experience
https://drive.google.com/file/d/1SHe_-f2DIIYDoxaQkmJMU8dA-4yAlvng/view
https://youtu.be/geyC6FfMFf8
## Fonts
https://fonts.google.com/specimen/IM+Fell+DW+Pica
https://fonts.google.com/specimen/Caveat

## 25/12/25
- [x] Migrate from vertex animation to armature
- [x] Make 2 bones instead of thee

## 26/1/2
- [x] make pages modular and data-driven
- [x] animate page turn
### iMPORTING GLB
DO NOT SET Y+ UP IN THE BLENDER EXPORT (the camera position was wrong (it mistaked the y and z axis))
![[Pasted image 20260102202639.png]]
now I played with scale and fov to find something I'm happy with (the page does't look like it's 30 meters tall from the camera presepctive)
the page model doesn't match the 2D page, I can find the fov value by brute force or
use the formula
$$
\text{vFOV} = 2 \arctan\left( \frac{h}{2d} \right)
$$Where:
- **vFOV** is the vertical field of view
- **h** is the height of the target
- **d** is the distance from the observer to the target

after factoring in the 0.6 scaling (very important) we get a neat 17.5 fov
![[Pasted image 20260102213014.png]]
now the model is properly positioned and scaled in the scene with natural looking animations

## 26/1/3
- [ ] Map DOM to page texture


# ABANDONED
why?
- Shaders not applying to DOM elements
- lack of clarity needed for a portfolio webpage

might revisit to learn react webgl threej and react