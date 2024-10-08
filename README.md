# Background

Image data acquisition is desired, but no software interfaces for it exist.  Hence, we need to write a program to read images from a raw stream.

The raw stream consists of:

- A `uint12` (12 bit) value per pixel.  They represent the grayscale reading for
  a pixel.  A value of 0 means no signal, whereas a value of 4095 means
  saturation.
- There are 640x480 = 307200 pixels represented per reading.  It's a rolling
  shutter camera scanning horizontally.  Hence, the first 640 values in the stream
  represent the top most row in the image.  The second 640 values represent the
  second from the top row in an image
- There is no padding between values.  In other words, one capture of an image
  consists of 640\*480\*12/8 = 460800 bytes.

Attached is a raw stream dump of the sensor readings.  Please feel free to
compile as you go.

1. Load this data into memory (filename is `image_binary_dump`).
2. Create a function to access pixel values by x,y coordinates.  For example,
  (x=10,y=200) corresponds to the 10th pixel from the left, and 200th pixel from
  the top.  The bit significance of each `uint12` pixel is
  11,10,9,8,7,6,5,4,3,2,1,0.
3. A small crop of the image needs to be passed through.  Please write a
  function that can crop out a box from this data.  It takes 4 parameters:
  `x`,`y`,`w`,`h`.  The `x`,`y` is the upper left starting location of the
  subset, `w` is width of the box (in pixels), and `h` is the height of the box
  (in pixels).  Crop the image using the following parameters (x=10, y=200,
  w=50, h=100).

4. Often times, ML models consume a floating-point representation.  The float
  value of each pixel is linearly dependent on the `uint16` value, where a fully
  saturated pixel maps to a value of 1.0 and a non-activated pixel maps to a
  value of 0.0.  Convert the image to a `float16` image.

5. Output the cropped float16 image to stdout.

6. Given this will be a video stream, we need to constantly load (#1), crop
  (#3), convert (#4), output (#5), unload, and repeat for the next frame -
  design functionality for this.  It's assumed the resolution of a single image
  will not change.  (filename is `video_binary_dump`).

7. How would you validate the results and the code you wrote?

## Submission

1. Please include a Makefile and build instructions.  Any compiler or operating
  system is OK, but GNU/Linux is preferred.  Please use only the stdlib for
  questions 1 to 6.  You can use whatever libraries (if wanted) for #7.
