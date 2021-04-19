# ESP32-CAM for OV2640 in RGB/YUV mode

- RGB565 capture, stream via BMP (lossless)
- Foto Quality
- Full Resolution
- Low Framerate

## Overview

To do HTTP-streaming or capture we have 3 choices:
- capture YUV422, convert it to JPG by software, then stream it to the browser. (low network payload)
- capture RGB565, convert it to JPG by software, then stream it to the browser. (low network payload)
- capture RGB565, put a BMP-header in front, then stream it to the browser      (high network payload, but no conversion)  

Framerate is between 1 and 10, depending on size.

## The Webpage

There are 2 new controls.
- streamMode (JPG/BMP) = **JPG** streams as jpg and does software conversion. **BMP** streams direct as BMP (only if RGB565 is selected)
- PixFormat (rgb565/yuv) = This selects the cameras pixelformat that it delivers. RGB565 or YUV422. (YUV422 seems better contrast in jpg)

## Colors

**RGB565** = R(5/32), G(6/64), B(5/32) Bits/Values per Color. Colordepth is 16Bit = 32*64*32=65536 different colors
This low resolution per color results in lines/areas shown on high color-diff borders.
Dithering should be applied to smoothen the image.
Because Green has 6-Bits, a greenish tint might be visible.

By encoding an image to JPEG dithering is somehow implied in the jpeg algorithm, so jpeg looks nicer.
For text-images jpeg is bad and RGB565 is the prefered.

YCrCb as **YUV422** = YUYV422,  is a downsample strategy. 4 Pixels share 2 Cr and 2 Cb values. (Cb,Cr are colordifference values)
422 is historical, actually its 211, as 2 Pixels share 1Cb and 1Cr value.
Pixeldata is stored as YUYV: Y0Cr0Y1Cb0 , so we have reduced the croma info by 2, resulting in 2Bytes per Pixel. (as in RGB565)
So 2 different 8Bit luminanzvalues(Y0,Y1) share the same color? No, because Y also carrys a "gammacorrection" part in it.

JPEG uses YCrCb.



IDF-V3.3.1 c code.
git clone -b v3.3.1 --recursive https://github.com/espressif/esp-idf.git

adapted from:
https://github.com/xenpac/ESP32-CAM-Linux-Motion


Have fun ;) xenpac