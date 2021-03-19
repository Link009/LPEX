# LPEX - License Plate EXtractor

License Plate EXtractor is a software which extract vehicles license plates. No neural networks, SVM, or other sophisticated algorithms are involved at this stage (for now).

This version is based on Chris Dahms version [available here](https://github.com/MicrocontrollersAndMore/OpenCV_3_License_Plate_Recognition_Python)

![An example](https://raw.githubusercontent.com/Link009/LPEX/master/examples/ok_examples.png)

What I did was to copy his repository and extract the code which do the detection of plates (his code contains also a character recognition system which worked pretty bad for me). After that I did some refactoring.

To use it:

> python Extraction.py --image car.png﻿


# Original blog post about this code

This project is divided mainly in two parts: plate detection and character recognition.

The code you will find next is an adaptation of Chris Dahms original License Plate Recognition. A video of his work can be found [here](https://www.youtube.com/watch?v=fJcl6Gw1D8k) as longs as his [original source code](https://github.com/MicrocontrollersAndMore/OpenCV_3_License_Plate_Recognition_Python).

I made this work (pretty much some refactoring) mainly because I was in need of a simple system to detect the license plate area in images. His aproach is good but his code contained also a character recognition system which I don’t need actually. I plan to use my own in the second part of this project.

For someone this code will be easier to understand due to it’s “straight to the point” way, for someone else will be like I did an awful job. The goal was for me to understand what I was doing and to create a ready-to-go system. This was achieved and it’s enough.

Code has been tested only with european license plates, especially with italian ones. I don’t know if it’s gonna work with other country plates. To do that you will have to adapt the code to your situation and, eventually, stick with a totally different approach.

Code is in Python 3.6 and OpenCV 3.4\. You will find the whole source at the end of this article. Any suggestion is widely accepted.

This is the image we are working on. Later I’ll show you the result with other images.

[![original_image](https://github.com/Link009/LPEX/blob/master/examples/1-original.png "Original image")](https://github.com/Link009/LPEX/blob/master/examples/1-original.png)

Then some basic preprocessing is applied.

[![gray](https://github.com/Link009/LPEX/blob/master/examples/2-gray.png "Gray")](https://github.com/Link009/LPEX/blob/master/examples/2-gray.png)

After that morphological operations called Top Hat and Black Hat are applied.

Morphological transformations are some simple operations based on the image shape.

It is normally performed on binary images. It needs two inputs, one is our original image, second one is called **structuring element** or **kernel** which decides the nature of operation.

We built the kernel before Top and Black hat processing. More about these operations are [available here](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_morphological_ops/py_morphological_ops.html).

[![top_hat](https://github.com/Link009/LPEX/blob/master/examples/3-topHat.png "Top Hat")](https://github.com/Link009/LPEX/blob/master/examples/3-topHat.png)

[![black_hat](https://github.com/Link009/LPEX/blob/master/examples/4-blackHat.png "Black Hat")](https://github.com/Link009/LPEX/blob/master/examples/4-blackHat.png)

We are going to subtract values between add value (in the code we are adding gray and TopHat) and BlackHat.

[![subtraction](https://github.com/Link009/LPEX/blob/master/examples/5-subtract.png "Subtraction")](https://github.com/Link009/LPEX/blob/master/examples/5-subtract.png)

Next Gaussian blur is added with a kernel of 5.

[![gaussian_blur](https://github.com/Link009/LPEX/blob/master/examples/6-blur.png "Gaussian blur")](https://github.com/Link009/LPEX/blob/master/examples/6-blur.png)

Threshold operations is applied on gaussian blur.

[![threshold](https://github.com/Link009/LPEX/blob/master/examples/7-thresh.png "Threshold")](https://github.com/Link009/LPEX/blob/master/examples/7-thresh.png)

Contours are checked. From here we are starting to look for characters. Indeed this method is based in searching for contours that might look like alphanumeric chars. This is the process from now:  

– check each contour  
– if it look like a char then save it in a list  
– don’t consider any contour which don’t respect this condition  
– detect the group of characters with a ROI and save it

Further on, in a coming article, I’ll show you how to process this image and do OCR on it.

[![image_contours](https://github.com/Link009/LPEX/blob/master/examples/8-imageContours.png "Image Contours")](https://github.com/Link009/LPEX/blob/master/examples/8-imageContours.png)

After the first wave of cleaning operations (aka mathematical operations on each contour ) this is the result.

[![contours_chars](https://github.com/Link009/LPEX/blob/master/examples/9-contoursPossibleChars.png "Contours chars")](https://github.com/Link009/LPEX/blob/master/examples/9-contoursPossibleChars.png)

A second filter is applied and our plate seems to be detected, somehow.

[![final_contours](https://github.com/Link009/LPEX/blob/master/examples/10-finalContours.png "Final contours")](https://github.com/Link009/LPEX/blob/master/examples/10-finalContours.png)

Finally, we construct a ROI around these contours. This will be cropped and used next for OCR.

[![detected](https://github.com/Link009/LPEX/blob/master/examples/11-detected.png "Detected")](https://github.com/Link009/LPEX/blob/master/examples/11-detected.png)

Here the ROI was applied on the original image for a more scenic result

[![detected_original](https://github.com/Link009/LPEX/blob/master/examples/12-detectedOriginal.png "Detected original")](https://github.com/Link009/LPEX/blob/master/examples/12-detectedOriginal.png)

Final cropped image, our plate.

[![cropped_plate](https://github.com/Link009/LPEX/blob/master/examples/13-plate.png "Cropped plate")](https://github.com/Link009/LPEX/blob/master/examples/13-plate.png)

These are other results I achieved.

Some good results...

[![ok_examples](https://github.com/Link009/LPEX/blob/master/examples/ok_examples.png "OK examples")](https://github.com/Link009/LPEX/blob/master/examples/ok_examples.png)

...and some fails.

[![ko_examples](https://github.com/Link009/LPEX/blob/master/examples/ko_examples.png "KO examples")](https://github.com/Link009/LPEX/blob/master/examples/ko_examples.png)

As you can see, for some images (even some “easy” and “obviously”) the recognition fails miserably. This because the system is a general one and, to work on more plates it have to be tweaked more. Also plates with different backgrounds, fonts and dimensions create confusion because of many variables.

Now you can understand why this isn’t an easy task by using traditional methods. Applying machine learning technics might highly improve the result.
