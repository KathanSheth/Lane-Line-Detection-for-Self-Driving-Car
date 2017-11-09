# **Lane Lines Detection on the Road** 


---



[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_Gray.jpg "Grayscale"
[image2]: ./test_images_output/solidWhiteCurve_Blur.jpg "Blur"
[image3]: ./test_images_output/solidWhiteCurve_Canny.jpg "Canny"
[image4]: ./test_images_output/solidWhiteCurve_masked.jpg "ROI Masked"
[image5]: ./test_images_output/solidWhiteCurve_Hough.jpg "Hough"
[image6]: ./test_images_output/solidWhiteCurve.jpg "Final Output Image"

---

### Introduction

The very first requirement of a Self-driven car is to detect lanes so that it can steer properly. This project shows how to detect road lanes using OpenCV and Python. 

This project includes the concept of Color channel in an image, Image Blurring, Edge detection, Region of Interest, Hough transformation to detect lines and 'Line of Best fit'.

This file contain theoretical explanation of this project. Refer the Code for explanation about each function and how it works.

### Explanation

### **1. Pipeline**

My pipeline consisted of 5 steps. 


   __1. Reading images and then convert it to Gray Scale__

   I am reading an image using image package from matplotlib and convert that image to _grayscale_. Here, I am converting the image to grayscale because Canny Edge detection (Explained in next step) required input image as grayscale image.

   Below is the output of my grayscale image.

![alt text][image1]


   __2. Canny Edge Detection__

   I am using _Canny edge detection_ to detect the edges in images. It takes a gray scale image as an input. The output shows positions of traced intesity discontinuities. Before applying Canny edge detection, I blurred the image with a filter of kernel size 3. For that I have used _Gaussian blurring_ technique.

   _Why Gaussian Blurring?_
   Gaussian blur(smoothing) is used to reduce image noise and reduce detail. Typically, it is a low pass filter which attenuates high frequency signals. Here, before using Canny edge detection I performed blurring. The reason behind that is edge-detection algorithms are sensitive to noise. Gaussian blur will reduce this noise in the image which ultimately improve the result of edge detection algorithm.

   _Why Edge Detection?_
   Edge detection is used to find the boundaries of objects withing image. It works by detecting discontinuities in brightness. The larger the size of gaussian kernel, the lower the detector's sensitivity to noise.

   Below is the output of Blur and Canny images.

   ![alt text][image2] ![alt text][image3]

   __3. Region of Interest and Hough Transformation__

   Third part of my pipeline is ROI and _Hough transformation_. ROI is simply the area we are interested in. Region other than a trapazoid is neglected. 
   Hough transform is a technique used to detect the shape in an image which can be represented as mathematical equation. In this project, hough transform is used to detect line. (y = mx + c) or (xcos(theta) + ysin(theta) = rho)

   Hough transform will return all the lines in our ROI in form of start point(x1,y1) and end point (x2,y2). 

   Below is the output of ROI and Hough images.

   ![alt text][image4] ![alt text][image5]

   __4. Best fit line and draw right/left lane line.__

   This part is the most import part of my pipeline. As explained, Hough transform will return many lines with start and end points. First, we have to differenciate left lines and right lines. Then, we have to find right and left average line (_Best fit line_) which will be our final result.

   The idea is to find slope and intercept of each individual lines. 
   Slope is calculated with `slope = delta(y) / delta(x)` and intercept is calculated as `intercept = y - (slope) * x`.
   I have collected (slope < 0) lines in one list and (slope>0) in other list. Then I have calculated the __weighted average__ for both right and left lists.
   At this point we will have __average slope__ and __average intercept__ for both right and left line.

   Now with the reasonable y values (y1 and y2) we can calculate related x values (x1 and x2) from `x1 = (y1 - intercept)/slope` `x2 = (y2 - intercept)/slope` and draw right and left lines with the end points as (x1,y1) and (x2,y2)

   __5. Store and Show images__

   Last part is to store each images in a directory and show them on a screen for visualization.

   Below is the output of final image.

   ![alt text][image6]



### 2. Potential Shortcoming for this project


One potential shortcoming would be what would happen when if we have snow on road!! Also, I doubt that the performance of this algorithm depends on weather condtion. What if we are travelling in traffic!!


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to select Hough transformation parameter automatically. Another potential improvement could be to design more robust and powerful algorithm for slope and averaging all the lines. Also, color scheme can be improved to detect the lanes in dark and bright condition.
