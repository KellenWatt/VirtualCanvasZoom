# VirtualZoom

A sorely needed example of how to zoom and pan in an HTML canvas, without relying on `scale()` and `translate()`, 
unlike the normal recommendations. 

## How it works

Normally, the recommended idiom is something to the effect of 

    context.translate(pt.x,pt.y);
    context.scale(factor,factor);
    context.translate(-pt.x,-pt.y);

Credit for sample to (this)[https://stackoverflow.com/questions/5189968/zoom-canvas-to-mouse-cursor] Stack Overflow answer.

This can work, but is very tied to the canvas itself. While the example here is still somewhat tied to the canvas, the logic itself 
can carry across to Canvas-like constructs in other languages and graphics libraries.

### Zooming

As the name implies, the way this system works is by having a virtual coordinate system that underlies all of the drawing. 
To accurately draw to this virtual system, the coordinates must be mapped from the real coordinate system (i.e. the actual 
coordinates of the canvas), to the virtual coordinate system. This is done by way of a scaling factor, which is initially 
the ratio between the two, and adding an offset to the result. The offset is set as the upper left corner of the virtual 
viewport, and is adjusted as the canvas is zoomed. If any operation on the offset would result in canvas going out-of-bounds, 
there are checks to put it back at the closest limit of the virtual system. After these changes are made, the canvas is redrawn, 
according to the instructions in the `draw()` method.

### Panning

This works by changing the previously mentioned offset by the change in the mouse's position, scaled to the virtual coordinate system. 
After this change, the canvas is redrawn, as before.
