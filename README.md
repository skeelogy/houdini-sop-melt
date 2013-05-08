Houdini/SOP/Melt
================

An implementation of a melt SOP in Houdini that allows users to melt an object using a 0-1 slider.

[![ScreenShot](http://cg.skeelogy.com/images/houdini-sop-melt-screenshot-youtube.jpg)](http://www.youtube.com/watch?v=EKkzOzK98ag)

The basic idea:

1. Move the points down proportionally according to the melt amount.

2. If a point gets below the bounding box, set its Y value to the minimum Y of the bounding box, and spread the point outwards in the XZ plane. This spreading-out motion could be in a radial direction away from the centroid, or along the point’s normal
  * For an object with holes, such as a torus, using normals would be a better choice so that melted pool will spread into the holes as well.
  * For an object like a cube, spreading radially outwards from the centroid would be a better choice because the normals for each side are facing the same direction, making the spread look stretched at the corners and unnatural
  * Noise is added to this outward spread to give a bit of variation.

3. Lastly, we need to give the melted pool a thickness, otherwise all the faces will be at the same level and undesired flickering will occur during shading due to Z-fighting. This is done by moving the points of the pool down proportionally according to how much they are supposed to be below the bounding box. This will cause the object to go below the ground, thus we have to move ALL the points up so that the bottom of the object stays stationary at the same level.

http://cg.skeelogy.com/melt-sop-using-vop-vex/
