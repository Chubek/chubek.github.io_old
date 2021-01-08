---
layout: post
title:  "Dilation, Erosion, Opening and Closing of Binary Images in Python"
date:   2020-12-30 09:12:17 +0330
categories: vision
---

# Binarizing the Image

A binary image is a 1-bit image in which each pixel, instead of being a value from 1 to 255 or an array of numbers [r, g, b], is either 0 or 1 --- 0 being completely black and 1 being completely white. This may be reversed according to your image decoder.

Using Python's PIL package we can write an algorithm that binarizes RGB images based on a *threshold*. If the mean of red, green and blue values is higher than the threshold, we go with either 0 or 1, or vice versa.

```
def binarize_image(threshold, inimage, outimage):
  for i in range(inimage.size[0]):
    for j in range(inimage.size[1]):
      if np.mean(inimage.getpixel((i, j))) > threshold:
        outimage.putpixel((i, j), 1)
      else:
        outimage.putpixel((i, j), 0)
```

So if we open this image:

![Screws](/assets/img/screws.jpeg)

And create a blank image:

```
im = Image.open("/content/screws.jpeg")   
out = Image.new('1', im.size, 0xffffff)
```

After thresholding it with a value of 125 we get:

![Thresholded](/assets/img/thresholded.png)

We're going to use this image for 4 widely used operations on binary images: erosion, dilation, opening, closing.

# Erosion

Erosion is basically retracting of the black pixels. Imagine a Matrix A and a 3x3 matrix B:

```
    1 1 1 1 1 1 1 1 1 1 1 1 1        
    1 1 1 1 1 1 0 1 1 1 1 1 1    
    1 1 1 1 1 1 1 1 1 1 1 1 1    
    1 1 1 1 1 1 1 1 1 1 1 1 1   
    1 1 1 1 1 1 1 1 1 1 1 1 1                
    1 1 1 1 1 1 1 1 1 1 1 1 1               1 1 1
    1 1 1 1 1 1 1 1 1 1 1 1 1               1 1 1
    1 1 1 1 1 1 1 1 1 1 1 1 1               1 1 1
    1 1 1 1 1 1 1 1 1 1 1 1 1        
    1 1 1 1 1 1 1 1 1 1 1 1 1    
    1 1 1 1 1 1 1 1 1 1 1 1 1    
    1 1 1 1 1 1 1 1 1 1 1 1 1   
    1 1 1 1 1 1 1 1 1 1 1 1 1
```

If for each pixel, we superimpose the origin of B, if B is completely contained by A the pixels are retained, else, turned to the white pixel.

```
    0 0 0 0 0 0 0 0 0 0 0 0 0
    0 1 1 1 1 0 0 0 1 1 1 1 0
    0 1 1 1 1 0 0 0 1 1 1 1 0
    0 1 1 1 1 1 1 1 1 1 1 1 0
    0 1 1 1 1 1 1 1 1 1 1 1 0
    0 1 1 1 1 1 1 1 1 1 1 1 0 
    0 1 1 1 1 1 1 1 1 1 1 1 0
    0 1 1 1 1 1 1 1 1 1 1 1 0 
    0 1 1 1 1 1 1 1 1 1 1 1 0 
    0 1 1 1 1 1 1 1 1 1 1 1 0
    0 1 1 1 1 1 1 1 1 1 1 1 0
    0 1 1 1 1 1 1 1 1 1 1 1 0
    0 0 0 0 0 0 0 0 0 0 0 0 0
```

The code for this in Python is:

```
def erosion(imin):
  imout = imin.copy()
  for i in range(1, imin.size[0] - 1):
    for j in range(2, imin.size[1] - 1):
      if imin.getpixel((i, j)) == 1:
        for m in [0, 1, -1]:
          for n in [0, 1, -1]:
            imout.putpixel((i + m, j + n), 1)
      else:
        continue

  return imout
```

The result is:

![Erosion](/assets/img/eroded.png)

# Dilation

Dilation is the exact opposite of erosion, it *expands* the black pixels. The code for it is:

```
def dilation(imin):
  imout = imin.copy()
  for i in range(1, imin.size[0] - 1):
    for j in range(2, imin.size[1] - 1):
      if imin.getpixel((i, j)) == 0:
        for m in [0, 1, -1]:
          for n in [0, 1, -1]:
            imout.putpixel((i + m, j + n), 0)
      else:
        continue

  return imout
```

The result is:

![Dilation](/assets/img/dilated.png)

# Mathematical Notation for Erosion and Dilation and their Duality

There's a *duality* between dilation and erosion. if A is the image and B is the 3x3 mask we saw above:


![Erosion and Dilation](/assets/latex/er_di.png)

# Closing and Opening

Closing and opening are summarily defined as:

![Opening and Closing](/assets/latex/oc.png)

The codes for them are:

```
def opening(imgin):
  imgout = imgin.copy()

  for i in range(2, imgin.size[0] - 2):
    for j in range(2, imgin.size[1] - 2):
      if imgin.getpixel((i, j)) == 1:
        for m in [0, 2, -2]:
          for n in [0, 2, -2]:
            if imgin.getpixel((i + m, j + n)) == 0:
              imgout.putpixel((i + abs(m) - 1, j + abs(n) - 1), 1)

  return imgout

def closing(imgin):
  imgout = imgin.copy()

  for i in range(2, imgin.size[0] - 2):
    for j in range(2, imgin.size[1] - 2):
      if imgin.getpixel((i, j)) == 0:
        for m in [0, 2, -2]:
          for n in [0, 2, -2]:
            if imgin.getpixel((i + m, j + n)) == 1:
              imgout.putpixel((i + abs(m) - 1, j + abs(n) - 1), 0) 

  return imgout
```

The results are:

![Opening](/assets/img/opened.png)

For opening and for closing:

![Closing](/assets/img/closed.png)


Hope you enjoyed this!


