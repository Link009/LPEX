# LPEX - License Plate EXtractor

License Plate EXtractor is a software which extract vehicles license plates. No neural networks, SVM, or other sophisticated algorithms are involved at this stage (for now).

This version is based on Chris Dahms version [available here](https://github.com/MicrocontrollersAndMore/OpenCV_3_License_Plate_Recognition_Python)

![examples](http://cvisiondemy.com/wp-content/uploads/2019/01/ok_examples.png)

What I did was to copy his repository and extract the code which do the detection of plates (his code contains also a character recognition system which worked pretty bad for me). After that I did some refactoring.

To use it:

> python Extraction.py --image car.png﻿


