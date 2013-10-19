Houdini/SOP/Melt
================

### Introduction

An implementation of a melt SOP in Houdini that allows users to melt an object using a 0-1 slider.

[![ScreenShot](http://cg.skeelogy.com/images/houdini-sop-melt-screenshot-youtube.jpg)](http://www.youtube.com/watch?v=EKkzOzK98ag)

### How It Works

1. Move the points down proportionally according to the melt amount.

2. If a point gets below the bounding box, set its Y value to the minimum Y of the bounding box, and spread the point outwards in the XZ plane. This spreading-out motion could be in a radial direction away from the centroid, or along the point’s normal
  * For an object with holes, such as a torus, using normals would be a better choice so that melted pool will spread into the holes as well.
  * For an object like a cube, spreading radially outwards from the centroid would be a better choice because the normals for each side are facing the same direction, making the spread look stretched at the corners and unnatural
  * Noise is added to this outward spread to give a bit of variation.

3. Lastly, we need to give the melted pool a thickness, otherwise all the faces will be at the same level and undesired flickering will occur during shading due to Z-fighting. This is done by moving the points of the pool down proportionally according to how much they are supposed to be below the bounding box. This will cause the object to go below the ground, thus we have to move ALL the points up so that the bottom of the object stays stationary at the same level.

### Files

* **skeelMeltScript.vfl:** The VEX scripted implementation of the melt effect
* **skeelMeltScript.otl:** OTL file compiled from skeelMeltScript.vfl using `vcc`
* **skeelMelt.otl:** The same effect implemented using VOP nodes (you can see the VOP network if you dive into the Skeel Melt SOP node)

skeelMeltScript.otl and skeelMelt.otl have slightly different parameters but should give you a similar melt effect.

OTLs are generated from Houdini FX 12.5.533 Apprentice.

### Installation Instructions

* In Houdini's main menu bar, select File > Install Digital Asset Library...
* Select the path to the desired .otl file, then click the Accept button

If the above steps do not work for any reason (perhaps due to a different version of Houdini?), you might have to create the OTLs on your own from the .vfl file. There are two ways to do this:

1) Using command line

`/path/to/vcc -l skeelMeltScripted.otl skeelMeltScripted.vfl`

2) Using GUI

* In Houdini's main menu bar, select File > New Operator Type...
	* Operator Name: skeelMeltScripted
	* Operator Label: Skeel Melt Scripted
	* Operator Style: VEX Type
	* Network Type: Geometry Operator
	* Save To Library: /path/to/your/preferred/location/skeelMeltScripted.otl
* In the dialog that follows, select the Code tab
* Copy the text contents of skeelMeltScripted.vfl into the "VEX Code" space
* Click on the "Test Compile" button to make sure that it compiles with `vcc`
* Click on the Accept button to complete the operation

### Usage Instructions

* In a SOP/Geometry context, drop in a Skeel Melt SOP
* Connect a **polygonal** geometry (e.g. a Sphere SOP) to the Skeel Melt SOP, then activate the Skeel Melt SOP
* In the Parameters pane, adjust the meltAmount slider and you should see the geometry melt in the viewport
* Tweak the other parameters accordingly

### Important Notes

* Only works with polygonal geometries

### Links

[http://cg.skeelogy.com/melt-sop-using-vop-vex/](http://cg.skeelogy.com/melt-sop-using-vop-vex/)

### License

Released under The MIT License (MIT)<br/>
Copyright (c) 2011 Skeel Lee ([http://cg.skeelogy.com](http://cg.skeelogy.com))