---
layout: post
title: Athens short tutorial
permalink: blog/athens
date: '2021-02-28 21:15:55 -0500'
categories: pharo gtoolkit gui morphic athens
comments_id: 2
draft: true
---

# Integration Athens and Spec.

# Introduction

There are two different computer graphics: vector and raster graphics. 
Raster graphics represents images as a collection of pixels. Vector graphics 
is the use of geometric primitives such as points, lines, curves, or polygons 
to represent images. These primitives are created using mathematical equations.

Both types of computer graphics have advantages and disadvantages. 
The advantages of vector graphics over raster are:
 *   smaller size
 *  ability to zoom indefinitely
 *  moving, scaling, filling, and rotating does not degrade the quality of an image

Ultimately, picture on a computer are displayed on a screen with a specific 
display dimension. However, while raster graphic doesn't scale very well when
the resolution differ too much from the picture resolution, vector graphics
are rasterized to fit the display they will appear on. Rasterization is the 
technique of taking an image described in a vector graphics format and 
transform it into a set of pixels for output on a screen.

Note: You have the same concept when doing 3D programming with an API like 
openGL. You describe your scene with point, vertices, etc..., and in the end,
you rasterize your scene to display it on your screen.

Morphic is currently the way to go on pharo for Graphics. However, all existing canvas
are pixel based, and not vector based. This can be an issue with current screen,
where the resolution can differ from machine to machine.

Enter Athens, a vector based graphic API. Under the scene, it can either use
balloon Canvas, or the cairo graphic library for the rasterization phase.

When you integrate Athens with Morphic, you'll use the rendering engine to 
create your picture. It's then transformed in a Form and displayed using on 
the screen using BitBlt.

# Athens with Morphic
We'll see how to use Athens directly integrated with Morphic. So will be the 
base class we'll use after for all our experiment:

First, we define a class, which inherit from Morph:
```smalltalk
Morph subclass: #AthensHello
	instanceVariableNames: 'surface'
	classVariableNames: ''
	package: 'Athens-Hello'
```

During the initialization phase, we'll create our Athens surface:
```smalltalk
AthensHello >> initialize
	super initialize.
	self extent: self defaultExtent.
	surface := AthensCairoSurface extent: self extent.
```
where defaultExtent is simply defined as
```smalltalk
AthensHello >> defaultExtent
	^ 400@400
```
The drawOn: method, mandatory in Morph subclasses, will ask Athens to render
its drawing, and it'll then display it in a Morphic canvas as a Form (a bitmap 
pictures)

```
AthensHello >> drawOn: aCanvas

	self renderAthens.
	surface displayOnMorphicCanvas: aCanvas at: bounds origin.
```

Our actual Athens code is located into renderAthens method:, and the result is
stored in the surface instance variable.
```smalltalk
AthensHello >> renderAthens
|font|
font := LogicalFont familyName: 'Arial' pointSize: 10.

	surface drawDuring: [:canvas | 
		"canvas pathTransform loadIdentity."
		surface clear. 
		canvas setPaint: ((LinearGradientPaint from: 0@0  to: self extent) colorRamp: {  0 -> Color white. 1 -> Color black }).
		canvas drawShape: (0@0 extent: self extent). 
		canvas setFont: font. 
		canvas setPaint: Color pink.
		canvas pathTransform translateX: 20 Y: 20 + (font getPreciseAscent); scaleBy: 2; rotateByDegrees: 25.
		canvas drawString: 'Hello Athens in Pharo/Morphic'
		
	].
```
To test your code, let's add an helper method. This will add a button on the left
of the method name. When you click on it, it'll execute the content of the 
script instruction.
```smalltalk
AthensHello >> open
	<script: 'self new openInWindow'>
```
On last things. You can already create the window, and see a nice gradient, with 
a greeting text. However, you'll notice, if you resize your window, that the 
Athens content is not resized. To fix this, we'll need one last method.
```smalltalk
AthensHello >> extent: aPoint
	| newExtent |
	newExtent := aPoint rounded.
	(bounds extent closeTo: newExtent)
		ifTrue: [ ^ self ].
	self changed.
	bounds := bounds topLeft extent: newExtent.
	surface := AthensCairoSurface extent: newExtent.
	self layoutChanged.
	self changed
```

Congratulation, you have now created your first morphic windows where content
is rendered using Athens.


# Athens vocabulary

The reason you are using Athens in a program is to draw. 
The *source* and *mask* are freely placed somewhere over the *destination* surface. 
The layers are all pressed together and the paint from the source is 
transferred to the destination wherever the mask allows it. To that extent there 
are five drawing verbs, or operations: *Stroke*, *fill*, *drawString*, *paint*
and *mask*. They are all similar, they differ by how they construct the mask.

## Destination
The destination is the *surface* on which you're drawing. This surface collects 
the elements of your graphic as you apply them, allowing you to build up a 
complex work as though painting on a canvas.
![Athens destination](/media/destination.png)

## Source
The source is the "paint" you're about to work with. I show this as it is—plain 
black for several examples—but translucent to show lower layers. Unlike real 
paint, it doesn't have to be a single color; it can be a pattern or even a 
previously created destination surface (see How do I paint from one surface to 
another?). Also unlike real paint it can contain transparency information—the 
Alpha channel.
![Athens source](/media/source.png)

## Mask
The mask is the most important piece: it controls where you apply the source to 
the destination. I will show it as a yellow layer with holes where it lets the 
source through. When you apply a drawing verb, it's like you stamp the source 
to the destination. Anywhere the mask allows, the source is copied. Anywhere 
the mask disallows, nothing happens.
![Athens mask](/media/source.png)

## Path
The path is somewhere between part of the mask and part of the context. 
I will show it as thin green lines on the mask layer. It is manipulated by path 
verbs, then used by drawing verbs.

## stroke
